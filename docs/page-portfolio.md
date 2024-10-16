---
title: Portfolio
permalink: /portfolio
---

## Selected works of @jernwerber

_This page is gradually being updated! Check back later! (Last updated: 2024-03-17)_

Below are some projects that I've worked on. I have also prepared a package of [Instructional Design Work Samples (PDF, 6.4 MB, Dropbox Link)](https://www.dropbox.com/scl/fi/qdh6dztzyx37z6eql0f2e/Jonathan-Weber-Instructional-Design-Samples.pdf?rlkey=ycc9p50mypo042xw9jagcjqg1&st=m0axm2bl&dl=0) that you can peruse!

{% for p in site.portfolio %}
<style>
.grid-container {
  /* max-width:960px; */
  display: grid;
  grid-auto-flow: row dense;
  gap: 10px;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 250px;
  /* transition:300ms; */
}

.portfolio-card.wide {
  grid-column: span 2;
}

.portfolio-card.ultrawide {
  grid-column: span 3;
}

.portfolio-card.tall {
 grid-row: span 2;
}

</style>
<div class="grid-container">
{% for port in site.portfolio %}
{% if port.portfolio_cards %}
  {% for p in port.portfolio_cards %}
  <div class="portfolio-card {{ p.card_mod }}">
    <img src="{{ p.card_uri }}">
  </div>
  {% endfor %}
{% endif %}
{% endfor%}
</div>

<!--
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
-->

