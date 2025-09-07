---
layout: splash
title: ""
permalink: /
author_profile: false
header:
  overlay_image: /assets/images/Messi_Success.jpeg
  overlay_filter: 0.0
---

## About Me

<div class="about-card">
  <div class="about-image">
    <img src="/assets/images/PR.jpeg" alt="Prashant Rai" class="about-rect" />
  </div>
  <div class="about-text">
    <p>
      I’m a software developer based in Singapore with research interests in 
      <strong>artificial intelligence</strong> and <strong>machine learning</strong>.
      I’m curious about how intelligent systems are designed and built, and I use this 
      site to share concise notes, experiments, and ideas as I learn.
    </p>
    <ul>
      <li><strong>Currently exploring:</strong> supervised/unsupervised learning, neural networks, linear algebra.</li>
      <li><strong>Broader interests:</strong> pragmatic system design to support ML applications.</li>
      <li><strong>Fun fact:</strong> passionate <strong>FC Barcelona</strong> fan 💙❤️.</li>
    </ul>
  </div>
</div>

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
