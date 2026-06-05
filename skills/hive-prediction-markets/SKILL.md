---
name: hive-prediction-markets
description: Use this skill when the user asks about Polymarket or Kalshi prediction markets — events, markets, outcomes, odds, prices, liquidity, stats, traders, holders, or trades. Never present market probability as ground truth.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "prediction-markets"
  requires_network: "true"
version: 1.0.0
---

# hive-prediction-markets — Prediction Markets

Use this skill for prediction market discovery, including Polymarket, Kalshi,
odds, pricing, event stats, traders, holders, trades, or outcome questions.

## Task toolset

Use `prediction_markets`.

Required identifiers: market id, event id, token id, trader address, or search
query depending on the request.

## Procedure

Read `references/workflow.md` when the request needs market/event discovery,
liquidity/probability caveats, trader context, or a structured prediction
market report.

1. Search/filter markets or events when the user starts with a topic.
2. Resolve exact market/event/token ids before stats, holders, or trades.
3. Report market price, volume/liquidity, outcomes, provider/source, and freshness.
4. Do not present market probability as ground truth.

## Example

For "Bitcoin election prediction markets" or "Polymarket vs Kalshi odds",
search markets/events first, then ask which specific market to inspect if
multiple candidates match.

## Runtime status handling

If a market search succeeds but stats are `degraded`, return the candidates and
label the missing market details clearly.
