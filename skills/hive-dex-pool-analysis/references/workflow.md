# DEX And Pool Analysis Workflow

Use this reference for pool, pair, liquidity, swap, trade, OHLCV, and DEX-flow
questions.

## Required identifiers

- Chain/network.
- Pair/pool address, token contract, or both token sides.
- Optional: DEX name, time window, candle interval, trade direction.

If the user gives only one token, search candidate pools and prefer the pool
most relevant to liquidity/volume.

## Tool selection order

1. Resolve chain and pool/pair/token identifiers.
2. Use `search_tools` for pair search, pool info, liquidity, trades, OHLCV, and
   trending pool tools.
3. Inspect schemas before invocation.
4. Start with pool/pair metadata and liquidity.
5. Add trades/OHLCV for flow or trend questions.

## Bounded calls

- Limit trade lists and candle counts.
- Do not infer token-wide liquidity from one pool without saying so.
- Keep pool-level and token-level metrics separate.

## Report template

- Summary: pool/liquidity/trade-flow read.
- Calls made: endpoints, chain, pair/pool/token identifiers.
- Evidence: liquidity, volume, trades, OHLCV, provider/freshness/runtime status.
- Caveats: missing pools, stale candles, thin liquidity, degraded provider.
- Next action: compare pools, widen time window, or add token diligence.

## Gotchas

- Same token pair can have multiple pools with different fee tiers and liquidity.
- Trending pools are not necessarily safe or liquid.
- OHLCV data can be unavailable even when pair metadata exists.
