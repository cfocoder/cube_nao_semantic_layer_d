# Scenario D rules

This project tests Cube Core combined with business context in Nao's Context Layer.

## Required behavior

1. Use only `cube_semantic`, specifically `cube_metadata` and `cube_query`.
2. Do not use direct PostgreSQL, SQL against the source database, or another connection.
3. Use only measures and dimensions returned by Cube metadata for observed data.
4. Retrieve and follow `agent/skills/contoso-business-days/SKILL.md` for the Contoso business policy.
5. Apply `effective_business_days = weekdays + (weekend_days × 0.25)` only when calculating metrics expressed per effective business day.
6. Never multiply, discount, or otherwise change monthly revenue, expense, cost, or other observed amounts.
7. Do not put the `0.25` policy, an equivalent metric, or business-policy documentation into Cube configuration or metadata.
8. If Cube cannot provide the required observed values, say so instead of guessing.
9. Report the semantic route, policy application, denominator, and unchanged numerator in the answer.
