---
title: "Data-Driven Disappointment Part 3: Dealing with data"

first_post: 2024-02-01-data-driven-disappointment-with-d3-js-part-1
previous_post: 2024-02-04-data-driven-disappointment-with-d3-js-part-2 
---

In part 3 of this series on using the D3 JavaScript library (d3.js), we will look at using the functionality provided by D3 to group our data, then use that grouped data as the basis for a simple pie chart showing the relative sizes (proportions) of the groups. We will look at a couple common D3 patterns, including select-data-join and creating and calling generated functions.

_Note: if you're just coming to this series or need a refresher on the story so far, why not check out the [first post]({%- post_url page.first_post -%}) or the [previous post (part 2)]({%- post_url page.previous_post -%})?_

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

```js
// d3.group( dataSet, property )
const groupedData = d3.group(data, d => d.LaidOff);
```

This separates the data in our `data` variable into two groups, based on the values of the property, `LaidOff`, specified by the function provided as the second argument[^arrowFunc] of `d3.group`: `"TRUE"` and `"FALSE"`.

[^arrowFunc]: Many of the methods in `d3` are looking for some sort of function to define _how_ to make sense of the data that's been provided-- of the properties available for each, which should be used to be able to answer the question that we've asked?

We can get a sense of the structure of `groupedData` using the spread operator `...` or `Array.from()`[^mapObjects]:

[^mapObjects]: `d3.group()` produces a Map object, so using `console.log()` directly on the result will just log an empty object `{}` to the console. Learn more in the MDN Web Docs entry on Maps: [MDN Web Docs: Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map).

```js
console.log(...groupedData); // or console.log(Array.from(groupedData))
// [
//     [ "TRUE", [ // all data where LaidOff === "TRUE" ] ],
//     [ "FALSE",[ // all data where LaidOff === "FALSE" ] ]
// ]

```

The size (`length`) of the arrays contained in each group would be the number of data points in each group (i.e., the number of roles/positions belonging to each group). The equivalent SQL might look someting like:

```sql
SELECT LaidOff, COUNT(*) FROM data GROUP BY LaidOff
;
```

## Generating the pie (and) slices with `d3.pie()` and `d3.arc()`

{% include d3-js-clc-chart-1.html %}

<details><summary>👆 The endpoint for this section. <em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-1.html %}
```
</div>
</details>

&nbsp;
We're _almost_ there. This is where we encounter some `d3` specific functionality in the form of data binding (with the `data()` method) and data joining (with the `join()`) method. Now we need to do the following:

1. Use `d3.pie()` to generate the start and stop angles for each slice, based on the size of the groups in our data.
1. Create and append an SVG group element, `<g>`, to act as a container for the pie slices.
1. Bind the generated pie slice data to `<path>` elements in the group that we just created.
1. Set the `<path>` element's `fill` and `stroke` colors by way of the `attr()` method.
1. Use `d3.arc()` to populate the `<path>` element's `d` attribute for the path, which determines how the `<path>` is drawn.
1. Append a `<title>` element to each `<path>` that describes the data that the pie slice is representing.
1. _Finally,_ append the SVG to the container `<div>`.

```html
<!DOCTYPE html>
<script type="module">
    import * as d3 from "https://cdn.jsdelivr.net/npm/d3@7/+esm";
    // your code goes here

    const rawData = `...`; // put the actual data string here

    const data = d3.tsvParse(rawData);
    const groupedData = d3.group(data, d => d.LaidOff);

    const svgWidth = 400;
    const svgHeight = 400;

    const d3svg = d3.create("svg")
        .attr("width", svgWidth)
        .attr("height", svgHeight)
        .attr("viewBox", [ -svgWidth / 2, -svgHeight / 2, svgWidth, svgHeight ])
        ;

    d3svg.append("g")
    .selectAll("path")
    .data(
        d3.pie().value(d => d[1].length)(groupedData)
        )
    .join("path")
        .attr("fill", "white")
        .attr("stroke", "black")
        .attr("d", d3.arc().innerRadius(50).outerRadius(100))
        .append("title")
            .text(d => d.data[1].length)
    ;

    d3.select("#d3-container").node()
        .append(d3svg.node())
    ;

</script>
<div id="d3-container">
    <!-- this will hold whatever D3 generates -->
</div>
```

### Binding and joining data

There's a bit to unpack in that code. First is the `d3` pattern of 

```js
selection.selectAll().data().join();
```

which is how some of the future `d3` magic works: it makes less sense right now since there are no existing elements that match `selectAll("path")`, _but if there were_, this is how `d3` would be able to select chart elements and update them with new data based on `.data().join("path")`.

### Generating pie slice start and end angles

Next is how data is being provided to the `data()` method: `d3.pie()` returns a _function_ that can be used with a dataset to generate the appropriate values for `startAngle` and `endAngle`. The function created by `d3.pie()` returns an array of objects based on the input data with the following structure[^d3jsPie]:

[^d3jsPie]: [`d3.pie()` documentation (https://d3js.org/d3-shape/pie#_pie)](https://d3js.org/d3-shape/pie#_pie)

```js
// these are generic values, but the structure is what's important
[
    {
        "data":  1, // the original data point. in our case, this would be an object.
        "value":  1, // the value derived from the original data point
        "index": 6,
        "startAngle": 6.050474740247008, // angle in RADIANS 
        "endAngle": 6.166830023713296, // angle in RADIANS
        "padAngle": 0 // gap between slices also in RADIANS
    },
    // ...
]
```
  
As for using/creating/calling `d3.pie()`, the two approaches below are equivalent:

```js
// create and call the function on one line
d3.pie().value(d => d[1].length)(groupedData);
// ----- pie() function -------|-- argument --|

// or

// create the function and assign it to a variable
const pieGenerator = d3.pie().value(d => d[1].length);
// call the function using that same variable
pieGenerator(groupedData);

```

We also need to instruct `d3` on how to extract (access) the necessary `value` from the data being provided, the sum of which determine the proportion of the pie that the slice should take. We can use an arrow function (`=>`) for this purpose.

Recall that our `groupedData` Map can be accessed as an array of arrays, i.e.:

```js
[    
    [ "TRUE", [ /* datapoints as objects in this array */ ] ],
    [ "FALSE", [ /* datapoints as objects in this array */ ] ]
]
```

For each group:

- `d[0]` is the group label, `"TRUE"` or `"FALSE"`
- `d[1]` is the datapoint array, the length of which is how many points are in the array

So, `d => d[1].length`, as a count of the datapoints belonging to a group, could be used as the `value` that would determine a group's slice of the pie.

### Using `d3.arc()` to generate paths

The shape of a `<path>` element is set by the code found in the value of the `d` attribute. The code for each slice of a pie chart can be generated by the `d3-shape` helper function, `d3.arc()`, which itself returns a function. 

```js
selection.join("path")
    .attr("fill", "white")
    .attr("stroke", "black")
    .attr("d", d3.arc().innerRadius(50).outerRadius(100))
```

Like the function created by `d3.pie()`, the function created by `d3.arc()` needs to be provided data to be able to generate the code for the `<path>` elements. We can specify inner and outer radii by chaining the methods `innerRadius()` and `outerRadius()` onto `d3.arc()` (if the `innerRadius()` is greater than 0, the created `d3.arc()` function will generate a donut shape). When using this function, if we don't explicitly specify what data to use, it will implicitly use the data that has already been bound-- which is actually a good thing in this case since that contains both a `startAngle` and an `endAngle`.

---