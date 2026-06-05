---
name: hive-dex-pool-analysis
description: Use this skill when the user asks about a DEX pool or trading pair, its liquidity depth, recent swaps or trades, OHLCV candles, trending pools, or token-level DEX flow on a given chain.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "dex"
  requires_network: "true"
version: 1.0.0
---

# hive-dex-pool-analysis — DEX And Pool Analysis

Use this skill for pool, pair, swap, liquidity, DEX trend, or token flow
questions.

## Task toolset

Use `onchain_dex_pool_analysis`.

Required identifiers: chain and pair address or token contract.

## Procedure

Read `references/workflow.md` when the request needs pool/pair resolution,
liquidity context, trade-flow comparison, or a structured DEX report.

1. Resolve the chain and pair/token identifier.
2. Pull pool info, liquidity, trades, OHLCV, and pair stats only as needed.
3. Distinguish pool-level facts from token-level facts.
4. Report liquidity depth, recent flow, and data freshness.

## Example

If the user provides only a token contract, first search for relevant pools or
pairs, then choose the pool with the most relevant liquidity/volume.

## Runtime status handling

If a DEX/pool endpoint is `degraded`, fall back to pair search or token-pool
tools and state which exact metric is missing.
