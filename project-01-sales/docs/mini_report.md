# Sales Performance Analysis — UCI Online Retail II

## Background
A UK-based online gift retailer operating December 2009 to December 2011
sought to understand revenue drivers, product performance, and regional
mix across 38 countries. This analysis covers ~824,000 valid transactions
totalling £20.97M in revenue, built using Power BI Desktop with a star
schema data model and 18 DAX measures.

## Key Findings

1. **Revenue declined -4.5% YoY in 2011 to £9.84M**, finishing -24.5%
   below the £13M annual target. Despite 18K orders at an average order
   value of £534.91 — slightly higher than the two-year average of
   £523.31 — volume alone could not offset the revenue shortfall,
   suggesting a structural demand issue rather than a pricing problem.

2. **The UK accounts for 69% of 2011 revenue (£6.8M) but is itself
   declining at -6.7% YoY.** Meanwhile France (+32.4%) and Australia
   (+331.9%) are growing strongly. Notably, international customers
   order at significantly higher values — Netherlands AOV of £3,040
   and Australia AOV of £3,026 vs the UK's £443 — suggesting
   international orders are bulk or wholesale in nature and represent
   a high-value growth opportunity.

3. **The General category dominates at 55.69% of 2011 revenue (£5.3M)
   but is declining YoY.** Bags (+~18% YoY) and Seasonal (+~15% YoY)
   are the only growing categories. Garden and General are both in
   decline, signalling a shift in customer demand that the current
   catalogue mix has not yet responded to.

4. **Product concentration risk is high.** The top product — PAPER
   CRAFT, LITTLE BIRDIE — generated £168K alone, with REGENCY
   CAKESTAND 3 TIER (£146K) and PARTY BUNTING (£98K) rounding out
   the top three. Heavy reliance on a small number of SKUs exposes
   the business to significant revenue risk if any top product loses
   traction.

5. **Lighting products show critical quality issues.** Flamingo Lights,
   White Cherry Lights, and Antique Lily Fairy Lights all show ~100%
   return rates. This points to a systematic product defect or
   misleading product description issue that is directly eroding
   revenue, inflating logistics costs, and damaging customer
   trust — requiring immediate investigation.

## Recommendations

1. **Prioritise international market development**, particularly
   France and Australia. These markets are growing rapidly and
   ordering at 6–7x the UK average order value. Even modest volume
   growth in these markets would meaningfully offset the UK decline.
   A targeted wholesale or B2B outreach programme should be evaluated.

2. **Rebalance the catalogue toward Bags and Seasonal categories.**
   These are the only two growing categories in 2011. Investment in
   expanding SKU depth in these ranges — combined with reduced
   promotional spend on declining General SKUs — would better align
   supply with demand signals.

3. **Conduct an urgent quality review of Lighting products.**
   A ~100% return rate across multiple Lighting SKUs is not a
   coincidence — it indicates either a manufacturing defect, incorrect
   product specifications, or misleading photography and descriptions.
   These products should be delisted or corrected immediately to stop
   the revenue and reputational damage.

## If This Were Production

- Connect to a live ERP or order management system via Power BI
  Gateway for daily scheduled refresh, enabling the Sales Director
  to monitor revenue vs target in real time rather than on a
  two-year historical dataset.
- Add customer RFM segmentation (Recency, Frequency, Monetary) using
  the existing CustomerID data to identify high-value customers at
  risk of churning — enabling targeted retention campaigns before
  revenue is lost.

## Data Limitations

- ~25% of transactions have no CustomerID (guest checkout orders) —
  these are excluded from all customer-level metrics but included
  in revenue totals.
- No cost or margin data is available in the source dataset —
  this analysis covers revenue only and cannot speak to
  profitability or contribution margin.
- Product categories are rule-based text classifications derived
  from product descriptions, not from source data — edge cases
  may be misclassified, particularly within the large General bucket.
- Monthly targets are illustrative figures created for portfolio
  demonstration purposes and do not reflect actual business targets.