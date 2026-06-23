---
name: hive-prediction-markets
description: Use this skill when the user asks about prediction markets — Polymarket or Kalshi events, markets, outcomes, odds, prices, liquidity, stats, traders, holders, or trades — including "what are the odds of X", "find markets about Y", or comparing venues. Use it whenever live prediction-market evidence is needed. Never present market probability as ground truth.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "prediction-markets"
  requires_network: "true"
version: 1.1.0
---

# hive-prediction-markets — Prediction Markets

Discover and analyze prediction markets — Polymarket, Kalshi — with prices,
liquidity, outcomes, traders, and trades, always framed as market-implied
odds rather than truth.

## Task toolset and identifiers

Toolset: `prediction_markets` (confirm against `hive://toolsets` if routing
fails).

- Required: market id, event id, token id, trader address, or a search query —
  depending on the request.
- Optional: status filter, limit, date range, outcome, liquidity threshold.

If the user starts with a topic, search candidates first and ask for selection
when multiple markets match — similar markets can have different resolution
criteria.

## Procedure

1. Call `search_tools` for prediction-market search, event, market, stats,
   holder, trader, or trade capabilities.
2. Call `get_api_endpoint_schema` for each endpoint before calling it.
3. Search events/markets for topic prompts; resolve exact market/event/token
   ids before pulling stats, holders, or trades.
4. Add liquidity/volume and freshness context before interpreting prices.
5. Report market price, volume/liquidity, outcomes, provider/source, and
   freshness — and never present probability as ground truth.

## Bounded calls

- Use small search limits and page through only when needed.
- Do not fetch all trades or holders unless the user asks for a deep dive.
- Keep outcome-token prices separate from event-level interpretation.

## Worked example

User: "What are the odds on the next Fed rate decision? Compare Polymarket
and Kalshi if both have markets."

1. `search_tools` → `{"query": "prediction markets search events fed rate decision", "limit": 5}`
2. `get_api_endpoint_schema` for the market-search endpoint returned, then
   `invoke_api_endpoint` with a bounded search per venue.
3. If several markets match, list candidates with resolution criteria and ask
   which to inspect — or pick the highest-liquidity exact match and say so.
4. Report outcome prices with volume/liquidity and freshness, framed as
   market-implied odds.

## Report template

```markdown
## Summary
[Market/event read and top outcomes in one or two sentences.]

## Calls made
- Toolset: prediction_markets
- Endpoint(s): [exact endpoint names]
- Identifiers: [market/event/token/trader ids]

## Evidence
- Outcomes and prices: [per outcome, per venue]
- Liquidity/volume: [values + as-of]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Market probability is not truth, thin liquidity, stale stats, candidate ambiguity.]

## Next action
[Inspect selected market, holders, trades, or related events — only if needed.]
```

## Gotchas

- Market prices reflect trading and liquidity, not verified probability.
- Similar markets can have different resolution criteria — quote them when
  comparing venues.
- Event-level and outcome-token-level data are not interchangeable.

## Runtime status handling

If a market search succeeds but stats are `degraded`, return the candidates
and label the missing market details clearly.

## Hand-offs

- General crypto market prices → `hive-market-research`.
- A trader's full wallet activity → `hive-wallet-investigation`.
- Standing "alert me when odds move" requests → `hive-stateful-monitoring`.
