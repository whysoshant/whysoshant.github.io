---
layout: splash
title: "Prashant Rai"
permalink: /
author_profile: false
header:
  overlay_image: /assets/images/banner.jpg   # <-- use a wide banner (2400x1000px works best)
  overlay_filter: 0.3
intro:
  - excerpt: "Software developer in Singapore exploring AI and machine learning. I write to learn in public."
feature_row:
  - image_path: /assets/images/projects.jpg
    alt: "Projects"
    title: "Projects"
    excerpt: "Mini-projects, experiments, and notes."
    url: "/projects/"
    btn_label: "View Projects"
    btn_class: "btn--primary"
  - image_path: /assets/images/blog.jpg
    alt: "Blog"
    title: "Blog"
    excerpt: "Posts about AI, learning, and exploration."
    url: "/blog/"
    btn_label: "Read Blog"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}
{% include feature_row %}

---

## About Me
I’m a software developer based in Singapore with research interests in **artificial intelligence** and **machine learning**.  
I use this site to share concise notes, experiments, and reflections as I learn.  

- **Currently exploring:** supervised/unsupervised learning, evaluation, small projects.  
- **Broader interests:** system design to support ML applications.  
- **Fun fact:** passionate FC Barcelona fan.

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
