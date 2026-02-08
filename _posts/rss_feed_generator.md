---
title: "Building an ML-based Article Extractor and RSS Feed generator (and why HTML made me question everything)"
excerpt: "A detailed write-up on an RSS feed generator I built, covering the mistakes, trade-offs, and lessons learned."
date: 2026-02-08
categories: [ml, project]
tags: [ml, rss, web-scraping, logistic-regression]
---

I built a hobby project that extracts the main article content from news webpages and generates RSS feeds.

The motivation came from a real client requirement. During a solutioning discussion, the client needed to identify and extract news articles from sources that did not provide RSS feeds. In the production solution, we used third-party extraction and RSS APIs and then used Azure OpenAI for summarization. Back then, the client asked if we could build our own RSS generator, but due to various other constraints it was not feasible.

The problem stayed in my head. After work, I kept coming back to it as a learning challenge and it became my first end-to-end ML project that touched the full loop: data collection, labeling, feature design, training, evaluation, failure analysis, and iteration.

Repo: https://github.com/whysoshant/rssgen

---

## What I wanted to build

Given a raw HTML page, I wanted a system that could:
- find the main article content reliably
- extract title, body, publish date, and a lead image
- generate an RSS feed from the cleaned articles

The important constraint for me was generality. I did not want to write a separate set of rules for each website. I wanted a single approach that could work across multiple publishers, and improve by retraining on more examples.

---

## The pipeline

At a high level the repo does four things:
1. download pages (homepages + article pages)
2. train a DOM “article block” classifier from saved HTML
3. run inference to extract cleaned article JSON
4. generate RSS XML

This is not a production scraper. It is a learning project where the goal is to understand what breaks, why it breaks, and how to design something that improves with iteration.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/8324c180-96c3-4993-8cfe-8e1302ddf628" />

*RSS Feed Generation Pipeline diagram*

---

## Data collection (and why saving raw HTML matters)

The first practical decision that helped me was saving everything locally:
- homepage HTML pages
- article HTML pages

This made experiments reproducible. When a site failed, I could replay the exact same HTML, inspect candidates, change logic, retrain, and test again without re-scraping.

<img width="1078" height="154" alt="image" src="https://github.com/user-attachments/assets/6470d2b9-4f8f-4d0b-8a27-1a11bc0d66ec" />

<img width="1075" height="181" alt="image" src="https://github.com/user-attachments/assets/ed183cd6-99b9-4424-b377-cd28b296e9d3" />

*Screenshot from pages_home and pages_article directory, which stored the page html as json*

---

## The messy reality of HTML

I underestimated how hard it is to extract “the article” from a modern news page.

Some recurring things that broke naive approaches:
- navigation-heavy layouts where menu and “related stories” sections dominate the DOM
- repeated newsletter prompts like “Sign up now”
- paywall and marketing wrappers that look like content blocks
- pages with multiple long blocks (article + “recommended” + “most read”) that all have lots of text
- inconsistent structures across publishers, even within the same site

At this point I tried the obvious simple heuristic: pick the largest text block.

It worked on some sources and failed badly on others.

The failures were annoying, but also the main reason I learned a lot. They forced me to stop guessing and start building a method that could be trained and improved.

---

## Turning HTML into a learning problem

### Candidate blocks

For each article page, I collect many candidate containers:
- `<article>`, `<section>`, `<div>`

Each candidate block is a potential “this is the article” hypothesis.

That gives a classic ranking/classification framing:
- score each candidate
- pick the best one


<img width="304" height="60" alt="image" src="https://github.com/user-attachments/assets/359219e5-ddc4-427e-a92e-9f8e1371a46d" />

*Candidate blocks extracted from a typical page per site*

### What the model sees (features)

For each candidate block, I compute a small set of structural signals:
- **n_words**: how much text the block contains
- **n_paras**: number of `<p>` tags
- **n_heads**: number of headings (`h1/h2/h3`)
- **n_imgs**: image count
- **link_density**: fraction of words inside links

I also include lightweight DOM identity hints from tag/id/class tokens, like:
- `tag=article`
- `class=story`
- `id=main-content`

These are intentionally simple. The goal was not to build a perfect extractor, but to learn how far you can go with basic signals and good labeling.

<img width="599" height="491" alt="image" src="https://github.com/user-attachments/assets/d5111c77-ac54-4b6a-924e-f72f1c88567f" />

*A snippet of feature extraction code from train_dom.py*

---

## The hardest part: labels

This is where I spent the most time.

If I wanted an ML model, I needed training labels:
- which candidate block is the true article block?

Manually labeling blocks for hundreds of pages is painful and does not scale.

So I used weak labeling: label generation from signals available in the HTML.

### Attempt 1: “largest block” labeling

My earliest training labels were basically:
- label the candidate with maximum text as positive
- everything else negative

It sounds reasonable. It also silently creates garbage labels for navigation-heavy pages.

A key mistake here was assuming “longest equals article”. When it fails, the model learns the wrong pattern and then fails more confidently.

This is where I learned that weak supervision is only useful if the weak label is actually correlated with the truth.

### Attempt 2: “gold text” labeling from metadata

A better labeling idea was to use metadata when available:
- JSON-LD (`application/ld+json`) fields like `articleBody` or `description`
- fallback meta tags (`description`, `og:description`)

I call this “gold text” in my code, not because it is perfect, but because it is closer to the article content than raw DOM noise.

Label generation approach:
- normalize words from gold text
- for each candidate block, compute word overlap recall with gold words
- select the candidate with highest recall
- if recall ties, pick the smaller block to reduce noise

Important note: gold text is used only to generate training labels. It is not used at inference time. The model still learns from DOM structure signals.

This helped a lot compared to “largest block” labeling because it anchors labeling to something closer to the article.

<img width="793" height="113" alt="image" src="https://github.com/user-attachments/assets/2f789596-32ef-4d87-ad2f-2b010be29a14" />

*Example of a meta-description "gold text" from a NYTimes article*

<img width="583" height="513" alt="image" src="https://github.com/user-attachments/assets/477cb226-d765-4fc1-8c15-de1ae36f3236" />

*Code snippet from gold-text extraction logic*

### Fallback labeling when metadata is missing or unreliable

Metadata is not always present or trustworthy.

So when gold text could not be used (missing or low match), I used a fallback heuristic score that favors:
- more text
- more paragraphs
- lower link density
- bonus if tag is `<article>`

This became a practical compromise:
- use metadata overlap when possible
- otherwise use a structured heuristic rather than “largest block”

<img width="580" height="527" alt="image" src="https://github.com/user-attachments/assets/cf075787-c94a-4701-afb3-dd1b6d101772" />

*Code snippet from fallback scoring block based on heuristics*

---

## A problem I had to debug: separating label generation from inference

This was one of the most important lessons for me.

In early iterations, I mixed ideas like:
- “use metadata overlap to choose the best block”
- “use the model to choose the best block”

That can accidentally turn into leakage or a pipeline that behaves differently during training vs inference.

The clean separation is:
- training time: metadata can help produce labels (weak supervision)
- inference time: only use features that will always exist (DOM signals)

Once I treated those as two separate stages, the pipeline became much easier to reason about and debug.

---

## Training the model

I trained a lightweight Logistic Regression classifier:
- fast to train
- easy to iterate
- good baseline for simple structured features
- with class balancing (`class_weight="balanced"`) because each page has one positive candidate and many negatives

The model predicts a probability for each candidate block.
At inference time, I select the candidate with the highest probability.

<img width="739" height="360" alt="image" src="https://github.com/user-attachments/assets/eef5c2cc-82e7-41bf-b305-7fe9f6e5c7b5" />

*training report output in terminal*

---

## Extraction: converting the chosen block into a clean article

Once I pick the best candidate block, I extract:
- **body**: stitched paragraphs from `<p>` and related text tags
- **title**: from `<title>` or `<h1>` fallback
- **published date**: from `<time>`, meta tags, or JSON-LD datePublished
- **lead image**: from within the block or `og:image` fallback

This is implemented in `scripts/article_extractor.py` and is called from the pipeline when building cleaned articles.

<img width="941" height="190" alt="image" src="https://github.com/user-attachments/assets/390e8543-a88e-4acf-a98d-c691497986e5" />

*Example of an extracted cleaned article JSON from data/articles_clean* 

---

## Debug loop: what I actually did week to week

Most of the project was iteration:
1. collect pages
2. train
3. run extraction
4. inspect bad outputs
5. adjust labels/features/extraction rules
6. repeat

The common pattern in failures was that the chosen block looked “article-like” by the signals but was actually:
- newsletter prompt
- “recommended stories”
- a wrapper that included too much navigation

What worked best was:
- improving weak label generation (gold text overlap + fallback scoring)
- focusing on features that penalize navigation-like containers (link density helped a lot)
- collecting more diverse training pages

This gave me a practical lesson: more data only helps if your labels are not lying.

---

## Results (current snapshot)

From my saved runs:
- processed 10+ sources
- extracted 281 cleaned articles
- published 260 RSS items end-to-end
- publish date coverage: 95.4%
- lead image coverage: 100%
- median cleaned body length: ~1,010 words

<img width="952" height="657" alt="image" src="https://github.com/user-attachments/assets/5b1944c4-d2a9-4672-a92c-28634438ed49" />

*Sample RSS Feed final output generated*

---

## What I learned

This project taught me a few things in a very real way:
- HTML is difficult data. Not malicious, just messy and inconsistent.
- weak supervision can save huge time, but label quality becomes the bottleneck
- features matter, but only if they reflect structure that generalizes
- iteration beats guessing, especially when you can reproduce failures locally
- keeping the pipeline simple made it easier to debug and improve

---

## Tech stack

- Python
- BeautifulSoup4 + lxml
- scikit-learn (Logistic Regression)
- NumPy, SciPy
- joblib
- YAML
- regex utilities



