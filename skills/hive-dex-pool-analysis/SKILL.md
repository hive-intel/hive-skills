---
name: hive-dex-pool-analysis
description: Use this skill when the user asks about a DEX pool or trading pair — liquidity depth, recent swaps/trades, OHLCV candles, trending pools, fee tiers, or token-level DEX flow on a chain — including "how deep is the X/Y pool", "what's trading on Uniswap", or a pasted pair address. For CEX prices, order books, and funding use hive-market-research; for whole-token diligence use hive-token-diligence.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "dex"
  requires_network: "true"
version: 1.1.0
---

# hive-dex-pool-analysis — DEX And Pool Analysis

Analyze on-chain DEX pools and pairs: liquidity, swaps, OHLCV, trends, and
token-level DEX flow, keeping pool-level and token-level facts clearly
separate.

## Task toolset and identifiers

Toolset: `onchain_dex_pool_analysis` (confirm against `hive://toolsets` if
routing fails).

- Required: chain/network plus pair/pool address, token contract, or both
  token sides.
- Optional: DEX name, time window, candle interval, trade direction.

If the user gives only one token, search candidate pools and prefer the pool
most relevant by liquidity/volume — and say which pool you chose, because the
same pair can have several pools with different fee tiers.

## Procedure

1. Resolve chain and pool/pair/token identifiers.
2. Call `search_tools` for pair search, pool info, liquidity, trades, OHLCV,
   and trending-pool capabilities.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Start with pool/pair metadata and liquidity; add trades/OHLCV for flow or
   trend questions.
5. Report liquidity depth, recent flow, and data freshness.

## Bounded calls

- Limit trade lists and candle counts.
- Do not infer token-wide liquidity from one pool without saying so.
- Keep pool-level and token-level metrics separate.

## Worked example

User: "How deep is the main PEPE/WETH pool on Ethereum, and which way has
flow gone today?"

1. `search_tools` → `{"query": "dex pair search pool liquidity trades ethereum", "limit": 5}`
2. Resolve candidate pools for PEPE
   (0x6982508145454Ce325dDbE47a25d4ec3d2311933), pick the deepest PEPE/WETH
   pool, and state the choice.
3. `get_api_endpoint_schema` then `invoke_api_endpoint` for pool info,
   liquidity, and a bounded trade list (explicit limit, today's window).
4. Report depth, net flow direction, and freshness with the template below.

## Report template

```markdown
## Summary
[Pool/liquidity/trade-flow read in one or two sentences.]

## Calls made
- Toolset: onchain_dex_pool_analysis
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, pair/pool/token addresses]

## Evidence
- Liquidity: [depth, fee tier, venue]
- Flow: [trades, volume, OHLCV in window]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Missing pools, stale candles, thin liquidity, degraded provider.]

## Next action
[Compare pools, widen time window, or add token diligence — only if needed.]
```

## Gotchas

- The same token pair can have multiple pools with different fee tiers and
  liquidity.
- Trending pools are not necessarily safe or liquid.
- OHLCV data can be unavailable even when pair metadata exists.

## Runtime status handling

If a DEX/pool endpoint is `degraded`, fall back to pair search or token-pool
tools and state which exact metric is missing.

## Hand-offs

- CEX prices, order books, funding, derivatives → `hive-market-research`.
- Whole-token diligence (holders, risk, metadata) → `hive-token-diligence`.
- "Is this pool's token safe to buy" → `hive-security-risk`.
