---
title: "Data-Driven Disappointment Part 4: Colour and title text"

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

With all that out of the way, here's where we're headed (over this part and the next):

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

D3 also provides utilities for working with colours through `d3.color()`, for example, the method `darker()` return a darker shade of a colour, based on an arbitrary input value, i.e., `d3.color("crimson").darker(1)` would produce a shade of `crimson` that is darker by `1` arbitrary unit. Combine this with knowing the _index of a datum_ (that thing I said to keep in mind 👀) and we can have a colour get progressively darker in correspondence with its index, for example:

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

The special sauce is in the arrow function being used for `fill`: 

```javaScript
(d, i) => d3.color("crimson").darker(i)
```
- The first argument, `d`, is as we've seen already--a reference to the datum or current datapoint (or group).
- The second argument, `i`, is the index. For each subsequent datapoint, this value increases, this means for our first datapoint, we get `d3.color("crimson").darker(0)` and for our second datapoint, `d3.color("crimson").darker(1)`, and so on, if we had more datapoints.

We can think of `darker()` (or its counterpart, `brighter()`) _like_ an interpolator in that it's creating a new colour based on a starting colour. D3 also has true colour interpolators (e.g., `d3.interpolateRgb()`), where a colour is interpolated within a colour range or spectrum, but with only 2 groups (for now), we would end up with both extremes of the spectrum. If you're so inclined, try, for example:

```javascript
(d, i) => d3.interpolateRgb("white","crimson")(i / (groupedData.size-1))
```

Yields:

![Pie chart with one white slice and one crimson slice, demonstrating use of the interpolateRgb() method for defining fill colour.](/assets/img/d3-js-red-white-interpolate.png)

## Adding a chart title

Between adding labels and adding a chart title, the chart title is _by far_ the more straightforward operation. Conceptually, it goes as follows:

1. Create a `<text>` element and append it to the `<svg>` element.
1. Position the `<text>` element using its `transform` and `text-anchor` attributes.
1. Style the `<text>` element using the `style()` method and CSS properties such as `font-size`.
1. Use the `text()` method set the `<text>` element's inner text.

Problems arise fairly quickly because of how rudimentary text handling is for SVGs: as far as I've seen, each line of a multiline text block needs to be its own element, so if you have a long title--_and chart titles often are_--, you'll need to break it into multiple elements, such as `<tspans>` inside the `<text>` element. So now the process looks like:

1. Create a `<text>` element and append it to the `<svg>` element.
1. Position the `<text>` element using its `transform` and `text-anchor` attributes.
1. Style the `<text>` element using the `style()` method and CSS properties such as `font-size`.
1. Create a `<tspan>` for each line of text.
1. Set the `x` and `dy` attributes of each `<tspan>` element so that the lines are positioned properly relative to each other. `x` controls the x-position of the element, while `dy` (delta y) is the offset distance in the y-direction, relative to the starting y-position. 
1. Use the `text()` method set each `<tspan>` element's inner text.

Labels are even more complicated since neither `<text>` nor `<tspan>` support CSS styling such as `background-color`, meaning that if we want a background behind our label to help with visibility/contrast then we need to draw it ourselves, for example using the `<rect>` element to place a rectangle. We'll see this later. For now, let's turn our attention to the code we need for the title:

```javascript
d3svg.append("text")
    .attr("id", "title-text")
    .attr("text-anchor", "middle")
    .attr("transform","translate(0 -270)") // instead of -270, you can also use 
//  .attr("transform",`translate(0 ${ -svgHeight/2 + shiftY })`) where shiftY is a y-offset. Don't forget the backticks for a template string!
    .style("font-size","20")
.selectAll("tspan")
.data(["CLC Layoffs 2024:", "NOT LAID OFF vs LAID OFF"])
    .join("tspan")
        .text(d => d)
        .attr("x", 0)
        .attr("dy", (d,i) => 24 * i)
;
```
- I'm making use of the same `selectAll-data-join` pattern here that we used for the pie slices.
- Each line of text for the title is an array element (`["CLC Layoffs 2024:", "NOT LAID OFF vs LAID OFF"]`) that's provided in the `data()` method so that we can use D3's utilities to loop over them and apply/calculate attributes.
- As children of the `<text>` element, the `<tspan>` elements will inherit the values for `text-anchor`, `transform` and `font-size` so we don't need to repeat them when we add in the `<tspan>` elements.
- We can assign the inner text of each `<tspan>` using the `text()` method and an arrow function that returns the value of each datum. In this case, that value is a line of text.
- We see the same arrow function structure, `(d,i) => 24 * i`, we just saw earlier, except the index, `i`, is being used to calculate the y-offset of each line. The first line (`i = 0`) has no offset (i.e., `24 * 0 = 0`), while the second line (`i = 1`) is offset by `24` units since `24 * 1 = 24`.

_Whew! That's a lot to take in, and there's still a bit more depth to cover, as alluded to, for adding labels to the chart. Until next time! (Part 5 in progress, but you can see the code above)_