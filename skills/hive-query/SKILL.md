---
name: hive-query
description: Use this skill as the default entry point for any live crypto intelligence question before answering from memory. Route wallet, token, DeFi, NFT, Solana, security, market, DEX, network, or prediction-market requests through Hive task toolsets and bounded endpoint invocation. If routing does not surface the exact tool or schema, hand off to hive-tool-discovery.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "discovery"
  requires_network: "true"
version: 1.0.0
---

# hive-query — Route Crypto Questions Through Hive

Use this skill when Hive MCP is connected and the user asks a crypto question.
Do not answer from model memory when the answer depends on live data.

## Routing rule

Read `references/root-mcp-workflow.md` when the user asks how Hive MCP is
organized, what tools are available, or how an agent should navigate the root
endpoint without loading the full provider catalog into context.

1. Read `hive://toolsets` or call `search_tools` with the user's intent.
2. Select one canonical task toolset.
3. Ask for missing identifiers before execution if the toolset requires them.
4. Call `get_api_endpoint_schema` for the exact tool you plan to call.
5. Call the exact endpoint through `invoke_api_endpoint`.
6. Report freshness, provider/runtime status, and any missing-data caveats.

## Canonical task toolsets

| Intent | Toolset id |
| --- | --- |
| Price, exchange, OHLC, liquidity, derivatives | `market_research` |
| Token metadata, holders, liquidity, enrichment, risk | `token_diligence` |
| Wallet balances, transfers, PnL, NFTs, DeFi positions | `wallet_investigation` |
| Token, approval, phishing, address, simulation risk | `security_risk` |
| Pools, pairs, trades, OHLCV, DEX flow | `onchain_dex_pool_analysis` |
| TVL, fees, yields, stablecoins, bridges | `defi_protocol_analysis` |
| NFT ownership, metadata, floors, sales, rarity | `nft_research` |
| Blocks, gas, logs, receipts, RPC diagnostics | `network_infrastructure` |
| Solana wallets, SPL accounts, DAS assets, priority fees | `solana_analysis` |
| Events, markets, outcomes, traders, holders | `prediction_markets` |
| Durable monitors, alerts, scheduled reports, agent memory | `stateful_monitoring` |
| Ambiguous request or schema lookup | `search_discovery` |

## Example

User: "Investigate this wallet on Ethereum."

Use `wallet_investigation`. Required identifiers are wallet address and chain.
Then search for the current wallet-balance endpoint, inspect its schema, and
only then invoke it. Do not hardcode stale endpoint names in the skill. Use this
sequence: `search_tools` for `wallet investigation ethereum balances`,
`get_api_endpoint_schema` for the selected endpoint, then `invoke_api_endpoint`
with schema-valid `address` and `network` or chain arguments.

Then use schema lookup and add transfer, NFT, or DeFi-position tools only when
the user's question needs that detail.

## Report template

Use this structure for Hive-backed answers:

```markdown
## Summary
[One or two sentences answering the user.]

## Calls made
- Toolset: [task toolset]
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, wallet, contract, market, protocol, etc.]

## Evidence
- Provider/source: [provider names]
- Freshness: [Hive execution fetched_at plus provider timestamp, block, or slot when relevant]
- Runtime status: [ok/missing_key/plan_required/rate_limited/degraded/failing]

## Caveats
[Fallbacks, stale data, unavailable providers, missing identifiers, limits.]

## Next action
[Only include if a retry, deeper check, or user choice is needed.]
```

## Runtime status handling

Hive uses `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, and
`failing`. Do not treat a non-`ok` provider state as a missing tool. Tell the
user which task/tool failed, why, and what can be retried or upgraded.

## Guardrails

- Never invent token, wallet, market, or event identifiers.
- Prefer exact contract addresses, wallet addresses, pair addresses, market ids,
  or exchange ids over fuzzy names.
- Keep raw provider endpoints callable; task toolsets are only the selection
  layer.
- If a tool is absent from the skill, use `search_tools` instead of guessing.
