---
title: Portfolio
permalink: /portfolio
---

## Selected works of @jernwerber

_This page is gradually being updated! Check back later! (2024-03-11)_

{% for p in site.portfolio %}
  <h2>
    <a href="{{ p.url }}">
      {{ p.title }}
    </a>
  </h2>
  {% if p.excerpt %}
  <p>{{ p.excerpt | markdownify }}</p>
  <a href="{{ p.url }}">Continue reading <em>{{ p.title }}</em>...</a>
  {% endif %}
{% endfor %}