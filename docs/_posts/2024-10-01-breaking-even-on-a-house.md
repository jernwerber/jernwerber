---
title: "Breaking even on a house: Rising prices are an inevitability because of transaction costs"
excerpt_separator: <!--more-->
math: true
tags:
    - financial literacy
    - housing
    - real estate
    - tableau
    - data
---

We recently bought a house. Now, with all the hemming and hawing about telework/remote work from the folks who had just recently been touting "Digital first!" and "The new normal!", I got to thinking about about what it would take to sell this house should we be forced to move for work. Even if I just wanted to _breakeven_ on this transaction (i.e. net cost of 0, just recouping the money spent on the purchase, nevermind the opportunity cost of the money committed to this endeavour), it isn't as simple as pricing the house for the same price that we bought it for: to reach breakeven, the price needs to go up for every successive transaction, _regardless of if the value of the property changed_. You can see this in the visualization below. The key takeaways:

1. Sale price needs to **increase by around 5% transaction-over-transaction** to reach breakeven on successive transactions.
1. After 3 sales (starting at $785,400 and selling at $915,708), there is a **cumulative sale price increase of around 16.6%** required to breakeven.

<div class='tableauPlaceholder' id='viz1727828927444' style='position: relative'><noscript><a href='#'><img alt='Breaking even on a house: Selling price required to breakeven for successive transactions ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;2X&#47;2XCK9KJ8D&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='path' value='shared&#47;2XCK9KJ8D' /> <param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;2X&#47;2XCK9KJ8D&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>
<script type='text/javascript'>var divElement = document.getElementById('viz1727828927444'); var vizElement = divElement.getElementsByTagName('object')[0]; vizElement.style.width='100%';vizElement.style.height=(divElement.offsetWidth*0.75)+'px'; var scriptElement = document.createElement('script'); scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js'; vizElement.parentNode.insertBefore(scriptElement, vizElement);</script>

_Note: Commission rate & property transfer tax rate were simplified to flat percentages for ease of creation, but they are described (and calculated) in greater detail below the fold, so the values might not be exact matches._

<!--more-->

## Property Transfer Tax (PTT)

Property purchases in British Columbia are subject to a Property Transfer Tax (PTT), payable by the buyer, based on the purchase price of the property. As of April 1, 2024, properties with a fair market value less than $835,000 are fully exempt from this tax for _first time home buyers_, while properties that are valued between $835,000 and $860,000 might qualify for a partial exemption (again, for _first time home buyers_. [See: First time home buyers' program: Do I qualify for an exemption?](https://www2.gov.bc.ca/gov/content/taxes/property-taxes/property-transfer-tax/exemptions/first-time-home-buyers#qualify)). Before April 1, 2024, the exemption threshold was a _laughable_ $500,000 (up to $525,000), compared to the provincial average price of around $845,000 (at the beginning of 2021), so most houses being purchased would have had to pay the PTT. This means that at the very least, you would need to include this amount in your sale price to recoup the money that you spent on it.

$$ PTT = 1\% \text{ of FMV up to }$200,000 + 2\% \text{ of the remaining FMV up to }$2,000,000 $$

For most purchases (i.e. ones that exceed $200,000), this would look like:

$$ PTT = $2000 + (FMV - $200,000) * 2\% $$

On a house at the August 2024 benchmark price of $785,400 (per [VIREB/CREA's August 2024 market report](https://vireb.com/market-report), _"Favourable conditions trending towards buyers' market"_), this would be **$13,708** and would need to be added to the sale price to recover all costs.

## Commissions

While it isn't strictly necessary to use realtors as part of the buying & selling process, many (most, even?) do, so there would be commissions that need to be paid to realtors participating in the transaction. While the statement that "sellers pay both both realtors" might be one of the most repeated (and believed) misrepresentations of a real estate transaction (if you believe this, think for just one moment, "Which is the only party bringing any money to the deal?" _Hint: It's the buyer._), the reality is that this cost does need to be built into the consideration of the sale price. There isn't a set commission rate or structure, but there are conventions and expectations around the amount. For the purposes of setting a sale price, the split between the buyer's and seller's agents doesn't matter, only the total commission (TC). For example:

$$ TC = 7\% \text{ of sale price (SP) for the first }$100,000 + 3\% \text{ of the remaining sale price } $$

For most purchases (i.e. ones that exceed $100,000), this would look like:

$$ TC = $7000 + (SP - $100,000) * 3\% $$

For a sale price at the August 2024 benchmark of $785,400, a total commission of **$27,562** would need to be paid out.