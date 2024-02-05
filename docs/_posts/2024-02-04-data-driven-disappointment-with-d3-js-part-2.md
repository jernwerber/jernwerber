---
title: "Data-Driven Disappointment Part 2: Getting set up and getting started"
---

## What is D3 (and D3.js)?

[D3 (https://d3js.org)](https://d3js.org), short for _Data-Driven Documents_, is a JavaScript library that is used for data visualisation. The "_Documents_" of D3 refer to the [document object model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) of a webpage, with which this library interacts. From D3's own website page (_"What is D3?"_, [https://d3js.org/what-is-d3](https://d3js.org/what-is-d3):

> _D3 is not a charting library in the traditional sense. It has no concept of "charts". When you visualize data with D3, you compose a variety of primitives._
> …
> _D3 makes things possible, not necessarily easy; even simple things that should be easy are often not. To paraphrase Amanda Cox: "Use D3 if you think it's perfectly normal to write a hundred lines of code for a bar chart."_

Sounds _perfect_.

## What data will we (can we) use?

The main constraint for data is that it has to be publicly available. Being a registered Canadian not-for-profit and charity, there is actually a lot of information that can be accessed by way of the organisation's annual statutory filing, the _T3010 Registered Charity Information Return_. I don't need to go that far to find what I want: the information that I want can be found on Canada Learning Code's own website at the bottom of the [_Our Team_ page (https://www.canadalearningcode.ca/our-team/)](https://www.canadalearningcode.ca/our-team/). 

### The plan

The plan is simple and easily reproducible:

1. Grab a historic version of the Our Team page from the Internet Archive's [WayBack Machine (https://web.archive.org/)](https://web.archive.org/) from some time around Judgment Day but before the webpage was updated.
1. Grab an up-to-date or updated version of the Our Team page.
1. Tabulate the roles of the people listed for the HQ Team.
1. Cross-reference the people listed for the HQ Team between page versions: If they aren't present in the updated version, they're likely (enough) to have been laid off. Flag these people.

There are few enough folks that I will do this manually, though it would probably be quite simple to scrape this information and set up a few rules to automatically cross-reference it as well. 

### Additional data notes

- Since there exists no publicly-accessible organisational chart (AFAIK), I won't be -grouping people into their organisational units.
- People can still be functionally grouped through reasonable inference based on their role title and a rough hierarchy will be coded, from 0 to 4, where:
  - 1 = "manager"
  - 2 = "director"
  - 3 = "chief" (non-executive)
  - 4 = "chief executive"
  - 0 = everyone else ("non-manager")   
- The hierarchy coding doesn't distinguish between the different types of managers, (e.g., people managers versus product or program managers), nor does it include within-level seniority (e.g., managers versus senior managers).
- I won't be using names, though they are publicly listed alongside the rest of the information being used.
- I won't be differentiating between full-time and part-time people because that information doesn't seem outwardly apparent or available.

## Getting D3 Set Up

There are lots of ways to load D3, however I will be using an `import` statement to grab it as an ECMAScript module (ESM) bundle from the JavaScript CDN `jsdelivr.net`[^3]:

[^3]: D3 also plays nice with other popular frameworks such as React and Svelte. Check out their [Getting Started page](https://d3js.org/getting-started) for more details on the different ways you can use D3.

```html
<!DOCTYPE html>
<script type="module">
    import * as d3 from "https://cdn.jsdelivr.net/npm/d3@7/+esm";
    // your code goes here
</script>
<div id="d3-container">
    <!-- this will hold whatever D3 generates -->
</div>
```

1. Note how the `<script>` tag has a `type` attribute of `module`[^5]: this is necessary to be able to make use of the `import` statement. Without it, you'll get an error message along the lines of: `SyntaxError: Cannot use import statement outside a module`. After the `import` statement, we can start using the different D3 components on our page throught the `d3` object/namespace.
1. I've also created a `<div>` with `id="d3-container"`, which will be the target for the elements created with D3.

[^5]: Learn more about the differences between JavaScript modules and standard scripts here: _[MDN: JavaScript Modules # Other differences between modules and standard scripts](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#other_differences_between_modules_and_standard_scripts)_

## Loading the data

__Did you know that if you drag-select and copy cells from Google Sheets, they will be pasted as a tab-separated values (tsv) string?__ I've gathered the data that I'll be working with as detailed above and I've staged it in a Google Sheets document structured as follows:

| Role | LaidOff | Functional | Hierarchy |
| ---- | ------- | ---------- | --------- |
| `string`, role title/name as written on website | `bool`-ish [^4], `"TRUE"` if the role disappeared between updates otherwise `"FALSE"` | `string`, The role's functional unit that's been inferred from the role title, e.g. `MARCOM`, `HR`, `FUND`, etc. | `int`, value from `0` to `4` codifying the role's relative position in the organisation's control structure. |

[^4]: `bool`_-ish_ because it's a string that has the vale of `"TRUE"` or `"FALSE"` but not a true `bool` as of writing.

Copying and pasting the data into an IDE yields the following string, which I'm assigning to a variable, `rawData`.

<details><summary>Show data string</summary>
<div markdown="1">
```js
const rawData = `Role	LaidOff	Functional	Hierarchy
Chief Executive Officer	FALSE		4
Chief Strategy & People Officer	FALSE		3
Director, Fund Development	FALSE	FUND	2
Director, Partnerships & Program Facilitation	FALSE		2
Director of Marketing and Communications	FALSE	MARCOM	2
Director of Finances & Accounting	TRUE	FIN	2
Director, Programs	FALSE		2
K-12 Program Manager	TRUE		1
Adult Program Manager	FALSE		1
Manager, Chapters	FALSE		1
Senior Manager, Partnerships	FALSE		1
Senior Manager, Program Facilitation	FALSE		1
Senior Project Manager	TRUE		1
Sr Manager Evaluation & Impact Measurement	TRUE	EVAL	1
Senior People and Culture Manager	FALSE	HR	1
Senior Marketing Manager	FALSE	MARCOM	1
Instructional Training Manager	FALSE		1
Senior Fund Development Specialist	TRUE		0
Sr. Learning Experience Designer	TRUE		0
Senior Partner Development Lead	TRUE		0
Senior Learning Facilitator	TRUE		0
Senior Learning Facilitator	TRUE		0
Senior Learning Facilitator	TRUE		0
Senior Fund Development Lead	FALSE	FUND	0
Senior Fund Development Lead	FALSE	FUND	0
Partnership Development	TRUE		0
Lead, Teen Ambassador Program (TAP)	TRUE		0
Senior Partnership Development Lead	TRUE		0
Senior Bilingual Learning Facilitator	TRUE		0
Senior Bilingual Learning Facilitator	TRUE		0
Bilingual learning Facilitator	TRUE		0
Learning Experience Designer, Adult Programs	TRUE		0
Learning Facilitator	TRUE		0
Learning Facilitator	TRUE		0
Learning Facilitator	TRUE		0
Learning Facilitator	TRUE		0
Bilingual, Partnership Development	TRUE		0
Accountant	FALSE	FIN	0
People and Culture Coordinator	TRUE	HR	0
Data Analyst	TRUE	EVAL	0
Partnerships Coordinator	TRUE		0
Marketing Coordinator	FALSE	MARCOM	0`
```
</div>
</details>

With a well formed tsv string, I can use the `d3.tsvParse()` method to--_you guessed it_--parse the tsv string into an array of objects, using each column name as a property.

```js
const data = d3.tsvParse(rawData);
//console.log(data); 
// [ 
//     { "Role": "Chief Executive Officer", "LaidOff": "FALSE", "Functional": "", "Hierarchy": "4" },
//     { "Role": "Chief Strategy & People Officer", "LaidOff": "FALSE", "Functional": "", "Hierarchy": "3" },
//     // and 40 other entries ...    
// ]
```

## Our first (pie) chart

{% include d3-js-example-simple.html %}

From here, we can begin to use (some of) the rest of D3's functionality to explore the data. The "100 lines of code" is, in this case, a bit of an exaggeration, but it's true that there's more than a little bit of setup and preparation necessary to make even a simple chart. Let's take a pie chart[^6], for example:

[^6]: A pie chart is a way of representing fractions or proportions, with each group carving out a certain amount of "chart angle" (pie slice) based on its size relative to other groups.

1. A pie chart is made of slices. In D3, a pie slice (or circle fraction) is called an `arc`.
1. An `arc` is defined by an `innerRadius`, an `outerRadius`, a `startAngle`, and a `stopAngle`.
1. It would be a hassle to have to determine these ourselves, but D3 has a pie generator, `d3.pie()` which can calculate the appropriate values for `startAngle` and `stopAngle` based on data that we provide.

Even with those tools to help us, we're still responsible for creating the scalable vector graphics (SVG) elements and inserting them into the DOM. SVGs are images that are made of paths defined in code. Behind the scenes, each pie segment in the example above looks a little something like:

```html
<path 
  fill="rgb(211, 238, 206)" 
  stroke="none" 
  d="M-48.808,-109.625A120,120,0,0,1,0,-120L0,-68.4A68.4,68.4,0,0,0,-27.821,-62.487Z">
</path>
```

The `fill` and `stroke` attributes are probably self-explanatory, but the `d` attribute is where the magic happens: this defines what shape the path will take when drawn--in our case, how the pie segment will appear. Thankfully, we're not the ones coming up with these numbers: we can provide data to `d3.arc()` and have it compute the appropriate paths.

### Planning the next move

1. ~`import` D3 ECMAScript module inside a `<script>` element with `type="module"`~ ✅
1. ~Create a container `<div>` for our future chart.~ ✅
1. ~Format data as tsv string and load with `d3.tsvParse()`.~ ✅
1. Create a root `<svg>` element to contain the chart graphics, defining a `width` and `height`.
1. Create a `<g>` (group) element to contain the `<path>` elements that will make up the pie chart.
1. Use `d3.pie()` and `d3.arc()` to generate the code for the pie chart slices, based on some data that we'll provide.
1. Put the generated pie chart into the container `<div>`.
1. Admire our handiwork! 💪

---