---
layout: single
title: "Prashant Rai"
permalink: /
author_profile: false
header:
  image: /assets/images/Messi_Resilience1.jpg
  caption: ""
  overlay_filter: 0.3       # optional dark overlay
  teaser: /assets/images/Messi_Resilience1.jpg

---


## About

I’m a software developer based in Singapore with research interests in **artificial intelligence** and **machine learning**.  
I’m curious about how intelligent systems are designed and built, and I use this site to share concise notes, experiments, and ideas as I learn.

- Currently exploring: supervised/unsupervised learning basics, evaluation, small-scale experiments.
- Broader interests: pragmatic system design to support ML applications.
- Fun fact: passionate **FC Barcelona** fan.

<!-- Icon-only social row -->
<div class="social-inline" style="margin: 0.75rem 0 1.25rem 0; font-size: 1.5rem;">
  <a href="mailto:kprashantrai@gmail.com" aria-label="Email" style="text-decoration:none; margin-right:14px;">
    <i class="fas fa-envelope"></i>
  </a>
  <a href="https://www.linkedin.com/in/whysoshant/" target="_blank" rel="noopener" aria-label="LinkedIn" style="text-decoration:none; margin-right:14px;">
    <i class="fab fa-linkedin"></i>
  </a>
  <a href="https://github.com/whysoshant" target="_blank" rel="noopener" aria-label="GitHub" style="text-decoration:none;">
    <i class="fab fa-github"></i>
  </a>
</div>


---

## Recent Posts

<ul>
{% for post in site.posts limit:5 %}
  <li>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <span class="page__meta" style="margin-left:6px;">{{ post.date | date: "%b %-d, %Y" }}</span>
  </li>
{% endfor %}
</ul>

<p><a class="btn" href="/blog/">See all posts</a></p>

---

## Recent Projects

<ul>
{%- assign recent_projects = site.projects | sort: "date" | reverse -%}
{%- for project in recent_projects limit:5 -%}
  <li>
    <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
    {%- if project.date -%}
      <span class="page__meta" style="margin-left:6px;">{{ project.date | date: "%b %-d, %Y" }}</span>
    {%- endif -%}
    {%- if project.excerpt -%}
      <div class="archive__item-excerpt" style="margin-top:2px;">{{ project.excerpt }}</div>
    {%- endif -%}
  </li>
{%- endfor -%}
</ul>

<p><a class="btn" href="/projects/">See all projects</a></p>
