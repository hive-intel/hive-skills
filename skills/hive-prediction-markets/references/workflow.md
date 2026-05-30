# Prediction Markets Workflow

Use this reference for prediction market discovery, events, outcomes, prices,
liquidity, stats, traders, holders, and trades.

## Required identifiers

- Topic/search query, event id, market id, token id, or trader address.
- Optional: status filter, limit, date range, outcome, or liquidity threshold.

If the user starts with a topic, search candidates first and ask for selection
when multiple markets match.

## Tool selection order

1. Use `search_tools` for prediction market search, event, market, stats,
   holders, traders, or trade tools.
2. Inspect schemas before invocation.
3. Search events/markets for topic prompts.
4. Resolve exact market/event/token ids before stats, holders, or trades.
5. Add liquidity/volume and freshness context before interpreting prices.

## Bounded calls

- Use small search limits and page through only when needed.
- Do not fetch all trades or holders unless the user asks for a deep dive.
- Keep outcome token prices separate from event-level interpretation.

## Report template

- Summary: market/event read and top outcomes.
- Calls made: endpoints, market/event/token/trader identifiers.
- Evidence: price, liquidity, volume, outcomes, holders/traders, freshness.
- Caveats: market probability is not truth, thin liquidity, stale/incomplete
  stats, candidate ambiguity.
- Next action: inspect selected market, holders, trades, or related events.

## Gotchas

- Market prices reflect trading/liquidity, not verified probability.
- Similar markets can have different resolution criteria.
- Event-level and outcome-token-level data are not interchangeable.
