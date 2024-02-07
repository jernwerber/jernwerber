---
title: "Data-Driven Disappointment Part 3: Dealing with data"
---

## The starting point

Recall that we have a TSV (tab-separated values) formatted data string in the variable `rawData` (provided below)

{% include clc-data-string.html %}

And the following starter/setup code (with the data loading integrated):

```html
<!DOCTYPE html>
<script type="module">
    import * as d3 from "https://cdn.jsdelivr.net/npm/d3@7/+esm";
    // your code goes here

    const rawData = `...`; // put the actual data string here

    const data = d3.tsvParse(rawData);

    const svgWidth = 400;
    const svgHeight = 400;

    const d3svg = d3.create("svg")
        .attr("width", svgWidth)
        .attr("height", svgHeight)
        .attr("viewBox", [ -svgWidth / 2, -svgHeight / 2, svgWidth, svgHeight ])
        ;

</script>
<div id="d3-container">
    <!-- this will hold whatever D3 generates -->
</div>
```

Which boils down to:
1. `import` everything from `d3`.
1. Parse the data out of the tsv string.
1. Setup the `<svg>` element that will be the canvas for the chart. Here, `svgWidth` and `svgHeight` are used as variables of convenience.
1. Use `d3.create("svg")` to create an `<svg>` element.
1. Assign values using the `attr()` method to the `<svg>` element's `width` and `height` attributes, which determine the size of the `<svg>` element.
1. Assign a value to the `viewBox` attribute[^viewBox]. This takes an array of 4 values corresponding to `min-x`, `min-Y`, `width`, and `height`. `min-x` and `min-y` are the lowest values that `x` and `y` can take inside the visible area of the `<svg>` element, i.e. they correspond to the top-left corner. In essence, this is defining the coordinate system within the `<svg>` element.

[^viewBox]: More detail about the `viewBox` attribute from MDN: https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/viewBox

If you're relatively new to JavaScript, this code (especially for the `d3.create()` method) might look a little odd, but _method chaining_ (whereby a method is immediately executed using the result of the previous method) is a major/common pattern in `d3`. You could also write it on a single line (as below) but I think it's easier to understand what's going on and what's being called when it's separated out as above.

```js
const d3svg = d3.create("svg").attr("width", svgWidth).attr("height", svgHeight).attr("viewBox", [ -svgWidth / 2, -svgHeight / 2, svgWidth, svgHeight ]);
```

## Grouping the data

Right now our data is stored as an array of objects. To be able to represent it as a pie chart, we need some way to represent it as single, graphable values. Take the question, 

> "_How many people were laid off from Canada Learning Code in January of 2024?_" 

What we would need from our data are counts of roles where `LaidOff="TRUE"` and `LaidOff="FALSE"`. If you're familiar SQL, you might be thinking about the `GROUP BY` clause, which you can use to summarize data by the values of a column. We can achieve something similar using `d3.group()`: