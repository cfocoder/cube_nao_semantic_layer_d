---
name: contoso-business-days
description: Use when calculating Contoso metrics expressed per effective business day.
---

# Contoso effective business days

## Scope

This skill is the only location in the A/B/C/D repositories that defines Contoso's weekend work policy. It is intentionally present in D and absent from C.

## Business policy

For a complete month:

```text
effective_business_days = weekdays + (weekend_days × 0.25)
```

## Calculation rules

- Obtain `weekdays`, `weekend_days`, and the monthly observed amount through `cube_semantic` and `cube_query`.
- Keep monthly revenue, expense, cost, and other observed amounts unchanged.
- Apply the factor `0.25` only to weekend days in the denominator.
- For a per-effective-business-day metric:

```text
amount_per_effective_business_day = monthly_amount / effective_business_days
```

- Do not divide by calendar days unless the question explicitly asks for calendar-day averages.
- Do not multiply weekend sales, revenue, expense, or cost by `0.25`.
- Do not invent missing weekday/weekend counts or observed amounts.
- If required members are unavailable in Cube, report that the metric cannot be verified.

## Tool boundary

Use only `cube_metadata` and `cube_query` from the `cube_semantic` MCP. Do not use `direct_postgres` or free SQL.

## Explanation requirement

When applying this policy, state that the numerator is the unchanged monthly observed amount and that the 25% factor affects only the denominator.
