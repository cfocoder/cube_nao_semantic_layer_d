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

Do not add `direct_postgres` to this project's `nao_config.yaml`, environment, or deployment override. C and D must use the same Cube model, dataset, prompt, model/provider, and question set; D differs only by its D-only Context Layer policies (the effective-business-day rule, customer-recency rule, Q033 product-pair interpretation, Q088 tie-inclusive top-20 rule, and placeholder-customer exclusion in `RULES.md`). Keep the original prompts and frozen golds unchanged. The upstream SQL for Q036 and Q089 uses `FactOnlineSales`, so D's separate recency references are **online-channel** analyses; they must not be labeled all-channel or combine `FactOnlineSales` with `FactSales`. Cube exposes neutral observed customer/date/sales/product members and does not store the D-only cutoffs, segment classifications, Q033 threshold, Q088 ranking/tie rule, or placeholder-customer policy. `OnlineCustomerMetrics` is the customer-grain surface for recency queries: use date-filtered aggregate measures rather than requesting a full customer list, which can be truncated by the Cube response limit. For any D analysis at customer grain, exclude customers with both first and last name exactly `Not Provided`; preserve those transactions in general sales/product/channel/date aggregates. For Q033, use the new neutral `OnlineProductPairs` cube; source-row pair counts and distinct online-order counts are separate measures, and only D's context applies the `>3` rule/top-20 policy. For Q088, rank eligible 2009 spend with shared places and include all customers at rank 20, even if the result has more than 20 rows, as defined in `RULES.md`. Store D-specific reference results separately from the original gold, and report policy compliance separately from baseline accuracy.
