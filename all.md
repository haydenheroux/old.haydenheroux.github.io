---
layout: default
title: Tags
---

<main>
  {% for tag in site.tags %}
    {% assign heading = tag[0] %}
    {% assign posts = tag[1] %}
    {% include section.html %}
  {% endfor %}
</main>
