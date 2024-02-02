---
title: "Data-Driven Disappointment: Looking at the 2024 Canada Learning Code Layoffs with D3 (d3.js)"
---

_Note: This post is currently under construction. Any questions can be directed to jon.weber@gmail.com and I might or might not respond._

![Meme of the scene from Mean Girls where Regina George (played by Rachel McAdams) calls out to Cady Heron (played by Lindsay Lohan) and says "Get in loser, we're going shopping!", but with a new caption that reads "Get in loser, we're doing layoffs!"](/assets/img/get-in-loser-clc-meme.png)

These days in tech, it feels like layoffs are the new pandemic overhiring. I don't consider [Canada Learning Code]("https://canadalearningcode.ca") to be a tech company, but, as a company whose mission is to

> _[bring] accessible computer science to communities across Canada so everyone can create with technology_

I think it would be fair to call it tech-adjacent. The New Year confetti had hardly settled when the email message[^1] landed in my inbox first thing in the morning on Thursday, January 11, 2024 (a.k.a. Judgment Day):

[^1]: This is paraphrased as I (_foolishly_) did not send myself a copy of the email message.

> _Effective immediately, your role as Learning Experience Designer for Adult Programs at Canada Learning Code no longer exists. You will be locked out of your company accounts at the end of the work day._

I wasn't the only one affected by this: across the organisation, short meetings titled "Your role at CLC" were popped into calendars for those of us that were being laid off. These were perfunctory engagements that were followed up with an official termination letter that detailed, among other things, what we should expect in terms of statutory (i.e., legally mandated) compensation as well as what additional severance was being offered-- standard layoff stuff, I imagine.

I say "I imagine" because this is the first time that I've been laid off from a job. I've worked fixed term contracts, where the end date is known from the get go; and I've voluntarily left (resigned from) organisations for one reason or another, but this is the first time I've been suddenly terminated while in a (supposedly) permanent role. It's shitty and I think it feels _especially_ shitty when you're taken by surprise. Unlike when you're anticipating an end date, when you're surprised the processing can only start as early as the day of your termination.[^2]

It wouldn't be fair to say that it took me _totally_ by surprise. As any accomplished calendar snoop or nosy person would know, you can tell when something is afoot. The evidence is there: meetings getting cancelled or moved; company-wide events getting delayed. Still, the scope of such plans can be hard to predict when all you have is the equivalent of digital tea leaves or animal bones. Perhaps it all amounts to nothing–a misreading of the signs; not so much a portent of things to come but a poor interpretation of normal scheduling churn (_spoiler: it wasn't nothing_).

I think part of how I deal with upsetting or unsavoury occurrences like this is seeking to understand what happened. I know what happened to _me_--I was laid off--but what happened _organisationally_? This is something of an intellectual exercise since no matter the conclusions drawn or the lessons learned I will be no less laid off than at the start, but I will rise to the challenge no less, and a challenge it will be: naturally, I will only be able to use publicly available information regardless of whatever inside baseball I may or may not have been privy to (_spoiler: none_). This will also be (perhaps more importantly) a fine excuse to continue learning about a JavaScript library with which I have only dabbled: [Data-Driven Documents (or D3.js)](https://d3js.org).

[^2]: I have other thoughts as to why it feels _extra_ shitty to get laid off from a mission-driven organisation (and about mission-driven organisations in general), but those will go somewhere (or nowhere) else so as to not derail this post too much.

## What data will we use?

The main constraint for data is that it has to be publicly available. Being a registered Canadian not-for-profit and charity, there is actually a lot of information that can be accessed by way of the organisation's annual statutory filing, the _T3010 Registered Charity Information Return_. I don't need to go that far to find what I want: the information that I want can be found on Canada Learning Code's own website at the bottom of the [_Our Team_ page (https://www.canadalearningcode.ca/our-team/)](https://www.canadalearningcode.ca/our-team/). 

### The plan

The plan is simple and easily reproducible:

1. Grab a historic version of the Our Team page from the Internet Archive's [WayBack Machine (https://web.archive.org/)](https://web.archive.org/) from some 1. time around Judgment Day but before the webpage was updated.
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

## What is D3 (and D3.js)?

[D3 (https://d3js.org)](https://d3js.org), short for _Data-Driven Documents_, is a JavaScript library that is used for data visualisation. The "_Documents_" of D3 refer to the [document object model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) of a webpage, with which this library interacts. From D3's own website page (_"What is D3?"_, [https://d3js.org/what-is-d3](https://d3js.org/what-is-d3):

> _D3 is not a charting library in the traditional sense. It has no concept of "charts". When you visualize data with D3, you compose a variety of primitives.
…
D3 makes things possible, not necessarily easy; even simple things that should be easy are often not. To paraphrase Amanda Cox: "Use D3 if you think it's perfectly normal to write a hundred lines of code for a bar chart."_

Sounds _perfect_.

## Getting D3 Set Up

As a JavaScript library, you can use a `<script>` tag to pull it in from a JavaScript CDN like `jsdelivr.net` which is what I will be doing [^3]:

[^3] D3 also plays nice with other popular frameworks such as React and Svelte. Check out their [Getting Started page](https://d3js.org/getting-started) for more details on the different ways you can use D3.

```js
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
```

From there, we can start using the different D3 components on our page throught the `d3.` object/namespace.

### Loading the data

__Did you know that if you drag-select and copy cells from Google Sheets, they will be pasted as a tab-separated values (`tsv`) string?__ I've gathered the data that I'll be working with as detailed above and I've staged it in a Google Sheets document structured as follows:

| Role | LaidOff | Functional | Hierarchy |
| ---- | ------- | ---------- | --------- |
| `string`, role title/name as written on website | `bool`-ish [^4], `"TRUE"` if the role disappeared between updates otherwise `"FALSE"`   `string`, The role's functional unit that's been inferred from the role title, e.g. `MARCOM`, `HR`, `FUND`, etc.
| `int`, value from `0` to `4` codifying the role's relative position in the organisation's control structure. |

<details>
    <summary>
    Copying and pasting the data into an IDE yields the following string, which I'm assigning to a variable, `rawData`.
    </summary>

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
Marketing Coordinator	FALSE	MARCOM	0
`
```

</details>

[^4]: `bool`_-ish_ because it's a string that has the vale of `"TRUE"` or `"FALSE"` but not a true `bool` as of writing.

---

