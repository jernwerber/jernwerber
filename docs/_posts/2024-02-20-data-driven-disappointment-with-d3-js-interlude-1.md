---
title: "Data-Driven Disappointment in Pictures: Layoffs × Hierarchy"
excerpt_separator: <!--more-->
tags:
    - data visualization
    - JavaScript
    - Data driven documents (D3)
    - d3.js
    - Canada Learning Code
    - Layoffs
---

We saw previously that **25 out of 42 staff**, or approximately **59.5%** of the entire staff (or, at any rate, of the staff listed on the web page), were laid off from Canada Learning Code in January of 2024. This is neatly visualized in the pie (donut?) chart below 👇: 

{% include d3-js-clc-chart-997.html %}

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-997.html %}
```
</div>
</details>

The overall proportion statistic, while _staggering in its magnitude_, doesn't give us all that much more detail as to the impact of the layoffs on the company's overall structure: We _do_ know that the company didn't shutdown, so there must be some intention to stick around at least for a little longer, but stick around for what purpose--who's left and what do they do? 

To get a better handle on what happened, we need to ask more questions.

**Perhaps the next questions we might ask should be:** _Who_ was laid off? Or, _how were layoffs distributed across the company_?
{: .call-out .larger-font .tight-spacing}

<!--more-->

## Lay offs by Hierarchy

To help us answer these questions, let's take a look at the first chart, below 👇:

{% include d3-js-clc-chart-999.html %}

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-999.html %}
```
</div>
</details>

### Chart description

- The **inner ring** in the nested chart below is separated by a role's `Hierarchy`, a number from `0` to `4` indicating an inferred (based on role title) relative power level of an employee within the organization (`0` corresponds to non-managers, while `1` to `4` corresponds to increasing degrees of decision-making power and responsbility)
- The **outer ring** is each `Hierarchy` segment divided further into those laid off (coloured in `lightgrey`) and those not laid off (coloured in some shade of red).

### Summary of data

1. **84% of non-manager employees** (the group that I was part of) were laid off. It was a _bad time_™ to not be a manager.
1. **30% of managers** were laid off, so not a super great time for them either, but they were less than half as likely to be laid off as non-managers. _Note: because this hierarchy level was assigned based on the use of the word "manager" in a role's title, there may be some non-people-managers mixed in here who might make more sense at the non-manager level._
1. **20% of directors** (1 out of 5) were laid off, so they fared slightly better than managers.
1. **0% of executives**, of which there were only 2 (CEO + 1) at the time of data collection, were laid off.

### Key takeaways

1. **Non-managers--the lowest level of employee--were significantly disproportionately affected by the Canada Learning Code lay offs compared to other hierarchy groups.** Non-managers include
roles such as `Learning facilitator`, `Data analyst`, and `Learning experience designer` as well as their `Senior` and `Bilingual` variants.
1. Given the relative safety of manager-level employees, **there are almost certainly people managers who were not laid off who now have greatly reduced (or non-existent) teams.**
1. Regardless of any sentiments around "leaders eating last", **the safest place to be during these layoffs was in senior leadership or higher (directors and up)**, with only _1 out of 7 (or approx. 14.3%)_ of that group being laid off. Truly inspiring leadership `o7` (saluting emoji 🫡).

_Hang tight, analysis to be continued!_
{: .call-out}

{% include d3-js-clc-chart-998.html %}

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-998.html %}
```
</div>
</details>

