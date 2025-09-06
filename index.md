---
layout: splash
title: "Prashant Rai"
permalink: /
author_profile: false
header:
  overlay_image: /assets/images/Messi_Resilience.jpg   # change if you like
  overlay_filter: 0.35
---

## About

I’m a software developer in Singapore with academic interests in **AI** and **machine learning**. I use this site to share concise notes, experiments, and ideas as I learn.

<!-- Icon-only social row -->
<p style="margin: 0.75rem 0 1.25rem 0;">
  <a href="mailto:kprashantrai@gmail.com" aria-label="Email" class="no-mark">
    <i class="fas fa-envelope fa-lg" style="margin-right: 10px;"></i>
  </a>
  <a href="https://www.linkedin.com/in/whysoshant/" target="_blank" rel="noopener" aria-label="LinkedIn" class="no-mark">
    <i class="fab fa-linkedin fa-lg" style="margin-right: 10px;"></i>
  </a>
  <a href="https://github.com/whysoshant" target="_blank" rel="noopener" aria-label="GitHub" class="no-mark">
    <i class="fab fa-github fa-lg"></i>
  </a>
</p>

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
