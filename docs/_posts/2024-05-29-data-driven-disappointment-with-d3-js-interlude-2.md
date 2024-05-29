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

Sometimes it can be useful to change the way that we're looking at the same data to be able to present other insights as to what the data could mean. Previously, I had use hierarchy as the inner ring _and then_ drilled down using LAID OFF status. Here, I've done the opposite:

{% include d3-js-clc-chart-998.html %}

<!--more-->

<details><summary><em>See the code</em>:</summary>
<div markdown="1">
```html
{% include d3-js-clc-chart-998.html %}
```
</div>
</details>

## LAID OFF status by Hierarchy

### Chart description

- The **inner ring** in the nested chart above is all roles separated by laid off status with those laid off coloured in dark red and those not laid off coloured in a brighter shade of red.
- The **outer ring** further separates the roles already grouped by LAID OFF status by a role's `Hierarchy`, a number from `0` to `4` indicating an inferred (based on role title) relative power level of an employee within the organization (`0` corresponds to non-managers, while `1` to `4` corresponds to increasing degrees of decision-making power and responsbility). These are coloured in different shades of aqua, with the darkest shades being for those with the least relative power.


### Summary of data

1. **59.5% of employees were laid off**, which we have already seen but bears repeating.
1. **84% of laid off employees were non-manager level** (21 out of 25), which might seem like a familiar figure but is, in fact, a coincidence with the proportion of non-manager employees who were laid off. 
1. **12% of laid off employees were managers** (3 out of 25), or _1/7th_ the number of non-manager employees. Of course, every other group will seem comparatively small to the first group, since the first group has already accounted for so many of the laid off employees. Only **4% of laid off employees were directors** (1 out of 25), while **0% of laid off employees were executives** (0 out of 25).
1. **40.5% of employees were retained** (17 out of 42).
1. **23.5% of the employees who were retained were non-manager level** (4 out of 17) and likewise **23.5% of the employees who were retained were director level** (4 out of 17 again).
1. **41% of the employees who were retained were manager level** (7 out of 17).
1. **12% of the employees who were retained were executive level** (2 out of 17).

### Key takeaways

1. Again, we see that **non-managers--the lowest level of employee--were significantly disproportionately affected** by the Canada Learning Code lay offs compared to other hierarchy groups, with around a **5:1 likelihood of non-managers being laid off versus not**.
1. The ratio of retained non-managers to managers is also quite skewed, with there being **1.75 managers retained for every non-manager employee retained**.
1. **The organization formed by the retained employees is very top-heavy**, with **76.5% of retained employees being of manager level or higher** or a ratio of **3.25 retained manager-level or higher employees for every non-manager employee**. If this doesn't raise 🚩 red flags 🚩 for potential sponsors, donors, or partners, it should at the very least raise eyebrows. _Why is so much management necessary for so few people?_

## Conclusions from each analysis

What conclusions can we draw about Canada Learning Code's current state from examining the data and key takeaways as we have?

1. Looking at this data, the first obvious conclusion one can draw is that **Canada Learning Code, in its current form, is excessively top heavy**. I think this is something that would concern sponsors, donors, or partners especially, since not only are there _way more manager+ employees than non-managers now_ (3.25:1), but that manager+ employees tend to command higher salaries than non-managers do. Thus the organization, from a salary mass perspective is _even more so top heavy_ than the already skewed ratios found in the hierarchy levels of retained employees.
1. For a second conclusion, **the damage done from gutting the non-manager level likely makes it very difficult for Canada Learning Code to achieve impact numbers anywhere near its previous years, especially at comparable dollar-to-impact value**. In terms of value, non-manager level employees represent the best bang for the donor buck: not only do they tend to be the cheapest employees, but also _most directly responsible for generating impact_. Getting rid of most of them not only reduces capacity overall, but it means that that same impact is now likely being done by higher level (i.e. more expensive) employees.
1. A third conclusion, then, is actually more of a pondering: let's say a donor, sponsor, or partner is fine with a bad value spend for one reason or another. _Why should that spend go towards Canada Learning Code? What value is being added that might convince an external party to give Canada Learning Code money? What does Canada Learning Code still do?_

_Note:_ I've mostly avoided talking about Canada Learning Code's volunteer base or chapter (club) model in these analyses for the simple reason that they're volunteers. They're free and any costs associated with them, or hosting events, would be utterly eclipsed by the money that would be spent keeping the lights on and keeping folks' paycheques on time. And besides, how much money would be _worth it to run a bunch of volunteer-based clubs_? How much infrastructure (or donor dollars) would you _really need_ to support that?
{: .call-out}

_That's it for now! Back to your regularly scheduled programming. This may get updated with further insights, but I believe it paints a pretty clear picture, based on freely available data and observations, on the strange state Canada Learning Code finds itself in._