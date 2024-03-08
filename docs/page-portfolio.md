---
title: Portfolio
permalink: /portfolio
---

## Selected works of @jernwerber

{% for p in site.portfolio %}
  <h2>
    <a href="{{ p.url }}">
      {{ p.title }}
    </a>
  </h2>
  <p>{{ p.content | markdownify }}</p>
{% endfor %}

_This page is under construction! Check back later! (2024-03-08)_