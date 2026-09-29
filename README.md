# Scenario D — Cube Core plus Contoso business Context Layer

This repository is the Nao Multi-project context for formal scenario **D**.

## Boundary

- No native PostgreSQL database is declared.
- Cube Core is accessed only through the `cube_semantic` MCP.
- Available MCP tools are `cube_metadata` and `cube_query`.
- The Contoso business skill is available in Nao's Context Layer.
- The skill defines `effective_business_days = weekdays + (weekend_days × 0.25)`.
- Monthly revenue, expense, cost, and other observed amounts are not modified; only the denominator for per-effective-business-day metrics changes.
- Q036/Q089 recency policy is specific to the benchmark's `FactOnlineSales` source: online-channel purchase dates and sales/product facts only, not all-channel customer history. The fixed D cutoff and 90/180-day thresholds stay in `RULES.md`; Cube contains only neutral observed members/measures.
- Across D questions at customer grain, exclude only placeholder records whose first and last names both equal `Not Provided`. Keep their transactions in general sales/product/channel/date aggregates unless the question explicitly requests a customer-qualified subset.
- Q033's D-only interpretation is also context-only: count distinct online order numbers containing both products, exclude frequencies at or below 3, rank descending, and return top 20. The shared Cube model exposes separate distinct-order and source-row-pair measures; it does not encode the D threshold.
- Q088's D-only top-20 policy ranks eligible customers by exact 2009 spend with shared places; include every customer tied at the twentieth place, even if that yields more than 20 rows. Keep this policy in context and preserve the original prompt/gold; A/B/C are unchanged.
- `direct_postgres` must not be present or usable in this project.

## Project mapping

- Nao project: `tesis-condition-d`
- Repository: `cfocoder/cube_nao_semantic_layer_d`
- Expected route: `cube_semantic` → Cube REST → Cube Core → PostgreSQL, plus the Contoso skill
