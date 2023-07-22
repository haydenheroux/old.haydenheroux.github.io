---
layout: default
title: Tags
---

<main>
  {% for tag in site.tags %}
    <h1 class="tag">{{ tag[0] }}</h1>
    <ul>
      {% for post in tag[1] %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
      {% endfor %}
    </ul>
  {% endfor %}
</main>
