# Market Research Workflow

Use this reference for live prices, venue data, order books, OHLC, derivatives,
and liquidity context.

## Required identifiers

- Asset: token id, symbol, or contract address.
- Quote currency, usually `usd`, when asking for prices.
- Venue/exchange id when the request is order-book, ticker, or derivatives
  specific.
- Time window and candle interval for OHLC or trend questions.

Ask for missing venue or chain details when the choice changes the answer. For
general price questions, choose a broad market-data toolset and state the source.

## Tool selection order

1. Use `search_tools` with the asset, venue, and market-data intent.
2. Inspect the exact schema with `get_api_endpoint_schema`.
3. For spot context, prefer price, ticker, and liquidity tools.
4. For chart context, prefer OHLC/OHLCV tools with explicit interval and limit.
5. For venue context, use exchange/order-book tools only after venue resolution.
6. For derivatives, include funding/open-interest context when available.

## Bounded calls

- Use small limits for candles and order-book depth unless the user asks for
  more.
- Avoid fetching all exchanges or all markets for a single-asset question.
- Do not compare data from different timestamps without saying so.

## Report template

- Summary: current market read in one sentence.
- Calls made: task toolset and exact endpoints.
- Evidence: provider, venue, identifiers, freshness, and key metrics.
- Caveats: stale data, missing venue, fallback, degraded provider, or thin
  liquidity.
- Next action: deeper venue, time-window, or liquidity drilldown if needed.

## Gotchas

- Symbols collide. Prefer contract addresses, CoinGecko ids, or venue ids.
- Order-book liquidity is venue-specific; do not generalize it to the full
  market.
- Market data can be fresh but still incomplete if a venue/provider is degraded.
