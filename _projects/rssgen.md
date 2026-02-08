---
title: "ML-Based DOM Article Block Classification for RSS Feed Generation"
excerpt: "This project crawls news websites, extracts article content from raw HTML using a trained DOM classifier, and generates RSS feeds — without per-site CSS selectors or hardcoded parsing rules"
---
## [ML-Based Article Classifier and RSS Generator](https://github.com/whysoshant/rssgen/blob/master/README.md)

## How It Works (Pipeline)

1. **Download pages**
   - Fetch homepages and article pages
   - Save raw HTML locally

2. **Train a DOM “article block” model**
   - Parse each article HTML into candidate DOM blocks (`div/section/article`)
   - Create training labels automatically (weak labels)
   - Train a simple classifier and save it

3. **Extract cleaned articles**
   - Load the trained model
   - Score candidate blocks and select the most “article-like” block
   - Extract title/body/date/lead image
   - Save cleaned JSON

4. **Generate RSS**
   - Convert cleaned articles into RSS XML
   - Write feeds to the output folder

### Tech Stack Used
   *Python, scikit-learn (logistic regression), NumPy/SciPy, joblib, BeautifulSoup4/lxml, YAML*

<!-- ### You can find detailed writeup in [this](https://whysoshant.github.io/notes/Project-1/) blog post -->
