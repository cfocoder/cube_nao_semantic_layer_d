# Scenario D rules

This project tests Cube Core combined with business context in Nao's Context Layer.

## Required behavior

1. Use only `cube_semantic`, specifically `cube_metadata` and `cube_query`.
2. Do not use direct PostgreSQL, SQL against the source database, or another connection.
3. Use only measures and dimensions returned by Cube metadata for observed data.
4. Retrieve and follow `agent/skills/contoso-business-days/SKILL.md` when a metric is expressed per effective business day.
5. Apply `effective_business_days = weekdays + (weekend_days × 0.25)` only to the denominator of those metrics; never change observed sales, revenue, expense, or cost amounts.
6. For customer lifecycle questions that explicitly ask for recency-based `Active`, `At Risk`, and `Churned` segments (including the original benchmark's Q036 and Q089), apply the D-only policy below. Do not reinterpret unrelated wording such as a generic “customer segment” as recency.
7. If Cube cannot provide the observed customer purchase dates or other required values, report the limitation instead of guessing or falling back to direct SQL.
8. Keep both business policies in D's Nao Context Layer only; do not encode the weekend factor or customer-recency thresholds in the database, Cube configuration, or Cube metadata.
9. Report the semantic route, the applicable policy, its cutoff/denominator, and unchanged observed amounts in the answer.

## D-only customer recency policy

The cutoff and thresholds below are context-only and are not stored in the physical dataset, Cube model, or Cube configuration. Purchase dates, sales amounts, and product keys remain observed data. The Cube model exposes neutral online-sales measures; it does not assign lifecycle segments or store these thresholds. This rule does not change the original question wording and applies only when a question explicitly asks for customer lifecycle classification by last-purchase recency (Q036 and Q089 in this benchmark).

- Q036 and Q089 use the benchmark's `FactOnlineSales` source, so this policy is explicitly **online-sales recency**, not all-channel customer recency. Do not describe it as all-channel, and do not combine `FactOnlineSales` with `FactSales` (which has no customer key and would risk duplicating online sales).
- Use `2009-12-31` as the fixed as-of date for this Contoso benchmark; include transactions dated on or before that date.
- Obtain each customer's latest online purchase date from `FactOnlineSales` through `cube_semantic` (`cube_metadata`/`cube_query`) using the neutral `latestOnlinePurchaseDate` measure at customer grain. Use the same online-sales fact for spend and product variety.
- Compute elapsed **calendar days** from the latest online purchase date to the as-of date.
- Classify `0–90` days as `Active`, `91–180` days as `At Risk`, and `more than 180` days as `Churned`. A customer with no qualifying online purchase is `Churned` under this D-only policy; report the customer cohort and do not silently drop such customers.
- For Q089, calculate each customer's online spend using the observed sales-amount sum and product variety using distinct product keys; then report average customer spend and average distinct products by D segment. Do not average transaction amounts or adjust observed amounts.
- Keep the original gold unchanged. Store separate D-policy references for Q036 and Q089 and score policy compliance separately from source-benchmark accuracy. Record explicitly that D's customer analysis is online-only.
- If Cube cannot provide the required customer/date/measure result through the semantic route, report the limitation instead of guessing or falling back to direct SQL.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
