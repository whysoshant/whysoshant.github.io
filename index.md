---
layout: splash
title:
permalink: /
author_profile: false
header:
  overlay_image: /assets/images/Messi_Success.jpeg   # <-- make sure this file exists
  overlay_filter: 0.30
---


## About Me
I’m a software developer based in Singapore with research interests in **artificial intelligence** and **machine learning**.  
I’m curious about how intelligent systems are designed and built, and I use this site to share concise notes, experiments, and ideas as I learn.

- **Currently exploring:** supervised/unsupervised learning basics, neural networks, linear algebra.  
- **Broader interests:** pragmatic system design to support ML applications.  
- **Fun fact:** passionate **FC Barcelona** fan 💙❤️.

---

## Recent Posts
<ul>
{% for post in site.posts limit:5 %}
  <li>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <span class="page__meta">{{ post.date | date: "%b %-d, %Y" }}</span>
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
      <span class="page__meta">{{ project.date | date: "%b %-d, %Y" }}</span>
    {%- endif -%}
    {%- if project.excerpt -%}
      <div class="archive__item-excerpt">{{ project.excerpt }}</div>
    {%- endif -%}
  </li>
{%- endfor -%}
</ul>
<p><a class="btn" href="/projects/">See all projects</a></p>
