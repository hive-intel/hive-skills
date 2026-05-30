# DeFi Research Workflow

Use this reference for protocol TVL, fees, revenue, yields, stablecoins,
bridges, and chain-level DeFi metrics.

## Required identifiers

- Protocol slug or chain.
- Optional: time window, metric type, yield pool, stablecoin, or bridge.

Ask for a protocol slug or choose a provider-supported slug only when the user
clearly named the protocol.

## Tool selection order

1. Resolve protocol or chain.
2. Use `search_tools` for protocol, TVL, fee/revenue, yield, stablecoin, bridge,
   or chain metrics.
3. Inspect schemas before invocation.
4. Pull only the metrics needed for the question.
5. For comparisons, normalize metric type and timestamp.

## Bounded calls

- Limit protocol lists and yield pools.
- Avoid comparing TVL snapshots from different dates without caveat.
- Mark missing fee/yield/stablecoin metrics instead of omitting them.

## Report template

- Summary: protocol or chain DeFi read.
- Calls made: endpoints, protocol/chain, metric filters.
- Evidence: TVL, fees/revenue, yield, bridge/stablecoin metrics, freshness.
- Caveats: unavailable metrics, stale snapshots, methodology differences.
- Next action: compare peers, inspect yield pool, or add token/pool diligence.

## Gotchas

- TVL, revenue, and fees answer different questions.
- High APY without liquidity/risk context is not a recommendation.
- Provider methodology can differ across chains and protocols.
