---
layout: splash
title: "Prashant Rai"
permalink: /
author_profile: false
header:
  overlay_image: /assets/images/Messi_Resilience1.jpg   # <-- make sure this file exists
  overlay_filter: 0.30
---

## About

Software developer in Singapore with academic interests in **AI** and **machine learning**.  
I write to learn in public—sharing concise notes, experiments, and ideas as I go.

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
