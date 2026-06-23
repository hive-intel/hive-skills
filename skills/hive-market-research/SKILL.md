---
name: hive-market-research
description: Use this skill for any live crypto market question — prices, 24h moves, liquidity, exchange/venue data, OHLC candles, order books, tickers, funding rates, derivatives, trading context — even casual asks like "what's BTC at" or "is ETH pumping". Use it whenever the answer needs current market numbers instead of memory. For on-chain pool depth and DEX trades use hive-dex-pool-analysis; for protocol TVL/fees/yields use hive-defi-research.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "market"
  requires_network: "true"
version: 1.1.0
---

# hive-market-research — Market Research

Answer live price, liquidity, exchange, OHLC, order-book, derivatives, and
trading-context questions with sourced, timestamped market data.

## Task toolset and identifiers

Toolset: `market_research` (confirm against `hive://toolsets` if routing
fails).

- Required: asset (token id, symbol, or contract address) and quote currency
  (usually `usd`).
- Venue/exchange id when the request is order-book, ticker, or derivatives
  specific.
- Time window and candle interval for OHLC or trend questions.

Ask for missing venue or chain details when the choice changes the answer. For
general price questions, choose a broad market-data tool and state the source.

## Procedure

1. Resolve ambiguous symbols first — call `search_tools` with the asset and
   intent.
2. Call `get_api_endpoint_schema` for each market endpoint before calling it.
3. Prefer focused calls: price, ticker, OHLC, order book, or funding data —
   not everything at once.
4. For derivatives questions, include funding/open-interest context when
   available.
5. Return freshness with every number, and avoid trading conclusions without
   liquidity context.

## Bounded calls

- Use small limits for candles and order-book depth unless the user asks for
  more.
- Avoid fetching all exchanges or all markets for a single-asset question.
- Do not compare data from different timestamps without saying so.

## Worked example

User: "What's the BTC funding picture across exchanges right now — anything
crowded?"

1. `search_tools` → `{"query": "funding rates derivatives bitcoin exchanges", "limit": 5}`
2. `get_api_endpoint_schema` for the funding-rate endpoint returned, then
   `invoke_api_endpoint` with schema-valid arguments and a bounded venue list.
3. Report per-venue funding with `fetched_at`, flag stale or degraded venues,
   and avoid a directional trade call unless asked.

If the user says "check the order book" without naming a venue, ask which
exchange — order-book liquidity is venue-specific.

## Report template

```markdown
## Summary
[Current market read in one sentence.]

## Calls made
- Toolset: market_research
- Endpoint(s): [exact endpoint names]
- Identifiers: [asset, venue, interval, quote]

## Evidence
- [Key metrics: price, volume, depth, funding — with venue and provider]
- Freshness: [fetched_at, candle close times]

## Caveats
[Stale data, missing venue, fallback, degraded provider, thin liquidity.]

## Next action
[Deeper venue, time-window, or liquidity drilldown — only if needed.]
```

## Gotchas

- Symbols collide. Prefer contract addresses, provider ids, or venue ids over
  bare tickers.
- Order-book liquidity is venue-specific; do not generalize it to the full
  market.
- Market data can be fresh but still incomplete if a venue/provider is
  degraded.

## Runtime status handling

Report `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, or
`failing` per tool/provider. If a venue endpoint is degraded, fall back to
another public market-data endpoint and state the substitution.

## Hand-offs

- On-chain pool depth, swaps, OHLCV per pool → `hive-dex-pool-analysis`.
- Protocol TVL, fees, revenue, yields → `hive-defi-research`.
- "Is this token safe" rather than "what's the price" → `hive-token-diligence`.
