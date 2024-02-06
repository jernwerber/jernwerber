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

