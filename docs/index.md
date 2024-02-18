---
title: 'jernwerber.dev: words & code'
exclude: true
---

{% assign about = site.pages | where:"title", "About me: Front page" | first %}
{% if about %}
{{ about.content }}
{% endif %}

**Hi! 👋 My name is [Jonathan]({% link page-about.md %}){: style="text-decoration:underline wavy;"} and this is one of my little corners of the Internet. Please take your shoes off at the door.**{: style="font-size:1.5em;"}

<!-- (This is a place for me to put things to show other people.) -->