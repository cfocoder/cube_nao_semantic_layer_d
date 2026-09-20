# Scenario D — Cube Core plus Contoso business Context Layer

This repository is the Nao Multi-project context for formal scenario **D**.

## Boundary

- No native PostgreSQL database is declared.
- Cube Core is accessed only through the `cube_semantic` MCP.
- Available MCP tools are `cube_metadata` and `cube_query`.
- The Contoso business skill is available in Nao's Context Layer.
- The skill defines `effective_business_days = weekdays + (weekend_days × 0.25)`.
- Monthly revenue, expense, cost, and other observed amounts are not modified; only the denominator for per-effective-business-day metrics changes.
- `direct_postgres` must not be present or usable in this project.

## Project mapping

- Nao project: `tesis-condition-d`
- Repository: `cfocoder/cube_nao_semantic_layer_d`
- Expected route: `cube_semantic` → Cube REST → Cube Core → PostgreSQL, plus the Contoso skill
