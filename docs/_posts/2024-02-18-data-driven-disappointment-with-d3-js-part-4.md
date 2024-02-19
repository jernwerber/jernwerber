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

At the end of [part 3 of this series]({%- post_url 2024-02-10-data-driven-disappointment-with-d3-js-part-3 -%}), we had a functional--_if a little boring_--pie chart, constructed through the use of `d3.pie()` and `d3.arc()`. With it, we can see the relative proportion of how many people were laid off compared to not, but only if we hover over each of the pie slices and read the labels. Let's fix that by adding labels to the pie slices, as well as a title to describe the chart.

## But first: Loading data from a file

The data string that we've been hauling around has been useful as an illustrative device, but I think it's unfairly inflating the line count for our d3 chart. Instead, let's load our data from a file that's been hosted (e.g., on GitHub), using the method `d3.tsv()`:

```javascript
// const rawData = `...`
// const data = d3.tsvParse(rawData);

const data = await d3.tsv("https://jernwerber.dev/static/clc-laid-off-2024-01-20.tsv");
```

- The `d3.tsv()` method loads and parses a `tsv` file from a URL; we don't need to explicitly parse it afterwards like we have to do with the data string.
- The `await` keyword is necessary because `d3.tsv()` is an asynchronous method based on `fetch()` so instead of blocking code execution by default, as a normal (synchronous) method would, it would instead return a `Promise`. Using the `await` keyword instructs JavaScript to wait until the `Promise` resolves before continuing, allowing us to use `d3.tsv()` effectively as a synchronous method.

This will shorten our code significantly (40+ fewer lines!) and we can imagine other potential use cases for this, such as point this function at a file that gets periodically updated, meaning we could have a data visualization that's always up-to-date.

## Disclaimer: Complexity and experience

This is around the area where the complexity of our code can kind of... _explode_. This is also around the edge of my experience of D3--I've dabbled and created things, but I do have to spend significantly more time figuring things out. I say this because I don't want you learning any bad habits from me. We will be building code that _ostensibly_ works, but it might not necessarily be structured in a way that would be considered a best practice. The most I can hope for is that the experience that I'm detailing here will help you to figure your own issues out or help you to understand D3 a little bit deeper than before.

## Answering our first question about the Canada Learning Code 2024 layoffs
{: .tight-spacing}

With all that out of the way, here's where we're headed:

{% include d3-js-clc-chart-2.html %}

<details><summary>👆 A little bit has been added 😅. <em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-2.html %}
```
</div>
</details>

And with that, we can also visualize the answer to the question we posed earlier:

**The question:** _How many people were laid off from Canada Learning Code in January of 2024?_
{: .call-out .larger-font .tight-spacing}

**The answer:** We can infer that **25** out of **42** listed Canada Learning Code employees were laid off or otherwise removed from the Our Team page based on the changes to that page that occurred before and after the termination date of January 11, 2024. _This means that a whopping **59.5%** of the entire organization's staff was liquidated 11 days into the New Year._
{: .call-out .larger-font .tight-spacing}

Could we have determine that simply using a spreadsheet and some rather simple calculations? _Definitely_[^d3Calc]. Would we have had to write nearly 100 lines of JavaScript to do so? _Almost definitely not_. Visualizations of data are a powerful tool, though, and this is just a simple example that can be a starting point for more meaningful visualizations.

[^d3Calc]: Once we had grouped the data, it's also a fairly straightforward matter of using D3 to perform calculations using the data to figure this out--but _where's the fun in that_?

## Adding colour

- The filled-in colour for the SVG elements that we're using, `<path>`, `<text>`, and `<rect>`, is set via the attribute, `fill`.
- The colour around the edge of the elements is set via the `stroke` attribute.
- There are many valid values for these attributes, including the [CSS `<named-color>` colors](https://developer.mozilla.org/en-US/docs/Web/CSS/named-color).

```javascript
d3svg.append("g")
    .selectAll("path")
    .data(
    d3.pie().value(d => d[1].length)(groupedData)
    )
    .join("path")
        .attr("fill", "white") // white fill colour
        .attr("stroke", "black")
        .attr("d", d3.arc().innerRadius(50).outerRadius(100))
        .append("title")
            .text(d => d.data[1].length)
;
```

Changing the `fill` colour of the pie slices is as easy as changing the second argument, for example to have a `crimson` pie slice, we would instead write:

```javascript
d3svg.append("g")
    .selectAll("path")
    .data(
    d3.pie().value(d => d[1].length)(groupedData)
    )
    .join("path")
        .attr("fill", "crimson") // crimson fill colour
        .attr("stroke", "black")
        // etc.
;
```
Resulting in:

{% include d3-js-clc-chart-1a.html %}

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-1a.html %}
```
</div>
</details>

The astute among you will notice that this only lets us pick a single color for every pie slice. _How might we pick different colours for our pie slices, for example, darker versions of a starting colour for each subsequent pie slice?_ D3 has got you covered: the methods chained to the `join()` method (such as `attr()`) are a bit special because they run in the context of each specific datapoint or _datum_. They have access to the current datum as well as the index (position) of it in the datapoint collection. _👈 Keep this in mind._

D3 also provides utilities for working with colours through `d3.color()`, for example, the method `darker()` return a darker shade of a colour, based on an arbitrary input value, i.e., `d3.color("crimson").darker(1)` would produce a shade of `crimson` that is darker by `1` arbitrary unit. Combine this with knowing the index of a datum (_I told you to keep this in mind!_) and we can have a colour get progressively darker in correspondence with its index, for example:

```javascript
d3svg.append("g")
    .selectAll("path")
    .data(
    d3.pie().value(d => d[1].length)(groupedData)
    )
    .join("path")
        .attr("fill", (d, i) => d3.color("crimson").darker(i)) // crimson fill colour
        .attr("stroke", "black")
        // etc.
;
```

Which, in practice, looks like:

{% include d3-js-clc-chart-1b.html %}

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-1b.html %}
```
</div>
</details>
