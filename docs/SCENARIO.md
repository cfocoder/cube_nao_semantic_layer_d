# Scenario D deployment contract

## Required Nao project

```text
Project: tesis-condition-d
Repository: https://github.com/cfocoder/cube_nao_semantic_layer_d.git
Branch: main
```

## Required runtime variables

```text
CUBE_API_URL
CUBE_API_SECRET
OPENAI_API_KEY / OPENAI_BASE_URL
OPENROUTER_API_KEY / OPENROUTER_BASE_URL
```

The existing deployment bootstrap must provide `/app/context/agent/mcps/cube_server.mjs` and the MCP SDK. The repository supplies `agent/mcps/mcp.json` and the Contoso skill; it contains no credentials or generated adapter source.

Do not add `direct_postgres` to this project's `nao_config.yaml`, environment, or deployment override. C and D must use the same Cube model, dataset, prompt, model/provider, and question set; D differs only by its D-only Context Layer policies (the effective-business-day rule and the customer-recency rule in `RULES.md`). Keep the original prompts unchanged. The upstream SQL for Q036 and Q089 uses `FactOnlineSales`, so D's separate recency references are **online-channel** analyses; they must not be labeled all-channel or combine `FactOnlineSales` with `FactSales`. Cube exposes neutral observed customer/date/sales/product members and does not store the D-only cutoffs or segment classifications. Store D-specific reference results for Q036 and Q089 separately from the original gold, and report policy compliance separately from baseline accuracy.
