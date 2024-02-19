---
title: "Data-Driven Disappointment Part 4: Colour and text"

first_post: 2024-02-01-data-driven-disappointment-with-d3-js-part-1
previous_post: 2024-02-10-data-driven-disappointment-with-d3-js-part-3

tags:
    - D3
    - tutorial
    - JavaScript
    - "Data driven documents"
    - d3.js
---

In part 4 of this series on using the D3 JavaScript library (d3.js), we will look at how to use `d3.color()` and some of the built-in `d3` color interpolators to specify `fill` colours for our pie slices. We will also be adding a title and labels to help make the data being presented a little clearer.

_Note: if you're just coming to this series or need a refresher on the story so far, why not check out the [first post]({%- post_url 2024-02-01-data-driven-disappointment-with-d3-js-part-1 -%}) or the [previous post (part 3)]({%- post_url 2024-02-10-data-driven-disappointment-with-d3-js-part-3 -%})?_

{% include d3-js-clc-chart-1.html %}

<details><summary>👆 The starting point for this section. <em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-1.html %}
```
</div>
</details>

At the end of [part 3 of this series]({%- post_url 2024-02-04-data-driven-disappointment-with-d3-js-part-3 -%}), we had a functional--_if a little boring_--pie chart, constructed through the use of `d3.pie()` and `d3.arc()`. With it, we can see the relative proportion of how many people were laid off compared to not, but only if we hover over each of the pie slices and read the labels.