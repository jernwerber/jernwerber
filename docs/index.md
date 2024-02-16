---
title: 'jernwerber.dev: words & code'
exclude: true
---

{% assign about = site.pages | where:"title", "About me" | first %}

{% if about %}
{{ about.content }}
{% else %}
This is a place for me to put things to show other people.
{% endif %}