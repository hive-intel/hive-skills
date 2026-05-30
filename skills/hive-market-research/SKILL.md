---
name: hive-market-research
description: Use this skill for live crypto market research including prices, liquidity, exchange data, OHLC candles, order books, tickers, funding rates, derivatives, and trading context. Use it whenever the user needs current market data or venue-specific market evidence.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "market"
  requires_network: "true"
version: 1.0.0
---

# hive-market-research — Market Research

Use this skill for live price, liquidity, exchange, OHLC, order book,
derivatives, or trading-context questions.

## Task toolset

Use `market_research`.

Required identifiers: token symbol or contract, plus chain or exchange id when
the request is venue-specific.

## Procedure

Read `references/workflow.md` when the request needs multi-step market
analysis, venue selection, freshness comparison, or a provenance-aware report
template.

1. Resolve ambiguous symbols with `search_tools`.
2. Inspect schemas for exact market tools before execution.
3. Prefer focused calls for price, ticker, OHLC, order book, or funding data.
4. Return freshness and avoid trading conclusions without liquidity context.

## Example

Ask for the exchange id if the user says "check the order book" without naming
a venue. Use `binance`, `kraken`, or another explicit exchange id only when the
user provided it or asked you to choose a fixture.

## Runtime status handling

Report `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, or
`failing` per tool/provider. If a venue endpoint is degraded, fall back to
another public market-data endpoint and state the substitution.
