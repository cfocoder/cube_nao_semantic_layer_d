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

The cutoff and thresholds are context-only and are not stored in the physical dataset or Cube configuration; purchase dates remain observed data. This rule does not change the original question wording and applies only when a question asks for customer lifecycle classification by last-purchase recency.

- Use `2009-12-31` as the fixed as-of date for this Contoso benchmark.
- Obtain each customer's latest purchase date on or before that date through the all-channel path exposed by Cube. Do not combine an all-channel source with a duplicate channel-specific source; do not use direct SQL.
- Compute elapsed **calendar days** from the latest purchase date to the as-of date.
- Classify `0–90` days as `Active`, `91–180` days as `At Risk`, and `more than 180 days` as `Churned`. Customers with no purchase on or before the as-of date are `Churned` under this test policy.
- For Q089, use the same classification for segment grouping; obtain spend and distinct-product variety from Cube without adjusting observed amounts.
- The original gold remains unchanged. Keep a separate D-condition reference result for the affected original questions (Q036 and Q089), and score whether D followed this policy separately from the source benchmark's original semantics.
- If Cube metadata/data cannot provide the necessary per-customer purchase date, say that the classification cannot be verified; never invent dates or thresholds.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
