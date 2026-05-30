# Token Diligence Workflow

Use this reference when a token needs metadata, liquidity, holder, enrichment,
and risk evidence.

## Required identifiers

- Chain/network.
- Exact token contract or provider-specific token id.
- Optional: quote token, pool address, wallet holder, or time window.

If the user gives only a ticker or name, resolve the exact contract before
execution. Do not assume a ticker maps to one asset.

## Tool selection order

1. Resolve chain and contract from the prompt or ask for them.
2. Use `search_tools` for token metadata, market, holder, DEX liquidity, and
   risk capabilities.
3. Inspect schemas before calling each endpoint.
4. Start with metadata and market/liquidity checks.
5. Add holder distribution and enrichment only when the user needs depth.
6. Add security/risk checks when the user asks whether to trade, approve, or
   trust the token.

## Bounded calls

- Limit holder lists and pool searches.
- Separate token-level data from pool-level data.
- Preserve provider-specific risk fields when they drive the conclusion.

## Report template

- Summary: token identity and overall diligence posture.
- Calls made: endpoints and identifiers.
- Evidence: metadata, liquidity, holders, risk/enrichment, source/freshness.
- Caveats: missing provider keys, plan gates, stale liquidity, unresolved
  contract ambiguity.
- Next action: security drilldown, liquidity venue check, or holder analysis.

## Gotchas

- A verified name/logo is not proof of safety.
- High FDV or high volume without pool depth can still be risky.
- Missing enrichment is not the same as a clean risk result.
