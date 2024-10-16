---
title: Portfolio
permalink: /portfolio
---

## Selected works of @jernwerber

_This page is gradually being updated! Check back later! (Last updated: 2024-03-17)_

Below are some projects that I've worked on. I have also prepared a package of [Instructional Design Work Samples (PDF, 6.4 MB, Dropbox Link)](https://www.dropbox.com/scl/fi/qdh6dztzyx37z6eql0f2e/Jonathan-Weber-Instructional-Design-Samples.pdf?rlkey=ycc9p50mypo042xw9jagcjqg1&st=m0axm2bl&dl=0) that you can peruse!

<style>

/* @property --portfolio-columns {
  syntax: "<integer>";
  initial-value: 6;
} */

.grid-container {
  /* max-width:960px; */
  --portfolio-columns: 6;
  display: grid;
  grid-auto-flow: row dense;
  gap: .5em;
  grid-template-columns: repeat(var(--portfolio-columns), 1fr);
  grid-auto-rows: min-content;
  /* transition:300ms; */
}


.portfolio-card-group {
  display:grid;
}

.portfolio-card {
  box-shadow:0 0 10px 2px darkgray;
}

.portfolio-card.subwide {
  grid-column: span 2;
}

.portfolio-card.wide {
  grid-column: span 3;
}

.portfolio-card.superwide {
  grid-column: span 4;
}

.portfolio-card.ultrawide {
  grid-column: span 6;
}

.portfolio-card.fullwide {
  grid-column: span var(--portfolio-columns);
}

.portfolio-card.tall {
 grid-row: span 2;
}

.portfolio-card.taller {
 grid-row: span 3;
}

</style>
<div class="grid-container">
{% for port in site.portfolio %}
  {% if port.portfolio_settings %}
    {% if port.portfolio_settings.is_group %}
      <div class="portfolio-card {{ port.portfolio_settings.group_class }} portfolio-card-group"
        style="{{ port.portfolio_settings.group_style }}"
        >
      {% for p in port.portfolio_cards %}
        <div class="portfolio-card-group-card {{ p.card_mod }}">
          <img src="{{ p.card_uri }}">
        </div>
      {% endfor %}
      </div>
    {% endif %}
  {% else if port.portfolio_cards %}
    {% for p in port.portfolio_cards %}
    <div class="portfolio-card {{ p.card_mod }}">
      <img src="{{ p.card_uri }}">
    </div>
    {% endfor %}
  {% endif %}
{% endfor %}
</div>

{% comment %}
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
{% endcomment %}

