---
name: hive-token-diligence
description: Use this skill whenever the user asks whether a specific token is real, legit, liquid, well-held, enriched, investable, or worth researching — "is this token a scam", "run diligence on 0x…", "who holds this", "does it have real liquidity" — even if they never say "diligence". Investigates metadata, market context, holders, DEX liquidity, enrichment, and risk signals for an exact chain and contract. For pre-transaction safety (approvals, signing, swap simulation) use hive-security-risk; for pool-level depth and trade flow use hive-dex-pool-analysis; for Solana mints use hive-solana-analysis.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "token"
  requires_network: "true"
version: 1.1.0
---

# hive-token-diligence — Token Diligence

Build a provenance-aware diligence picture of one token: identity, market
context, liquidity, holders, enrichment, and risk. Never answer from model
memory when the conclusion depends on live data.

## Task toolset and identifiers

Toolset: `token_diligence` (confirm against `hive://toolsets` if routing fails).

- Required: chain/network plus exact token contract or provider-specific token id.
- Optional: quote token, pool address, holder wallet, time window.

If the user gives only a ticker or name, resolve the exact contract before
execution. Tickers collide across chains, and scam tokens deliberately reuse
well-known names — an answer keyed to the wrong contract is worse than no
answer.

## Procedure

1. Resolve chain and contract from the prompt, or ask for them.
2. Call `search_tools` for token metadata, market, holder, DEX liquidity, and
   risk capabilities matching the question.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Call `invoke_api_endpoint` with schema-valid, bounded arguments.
5. Start with metadata and market/liquidity checks. Add holder distribution
   and enrichment only when the user needs depth, and security/risk checks
   when the user asks whether to trade, approve, or trust the token.

## Bounded calls

- Limit holder lists and pool searches; do not page through everything for a
  quick read.
- Keep token-level data separate from pool-level data.
- Preserve provider-specific risk fields when they drive the conclusion.

## Worked example

User: "Thinking about buying PEPE — contract
0x6982508145454Ce325dDbE47a25d4ec3d2311933 on Ethereum. Is it liquid and is
anything sketchy in the holders?"

1. `search_tools` → `{"query": "token metadata liquidity holders risk ethereum", "limit": 5}`
2. `get_api_endpoint_schema` for the metadata, liquidity, and holder endpoints
   the search returned.
3. `invoke_api_endpoint` per schema — argument names come from the schema you
   just fetched (typically a contract `address` plus a `network`/chain field),
   never from memory.
4. Read each response envelope's `meta` for `provider`, `fetched_at`, and
   `runtime_status`, then answer with the report template below.

## Report template

```markdown
## Summary
[Token identity and overall diligence posture in one or two sentences.]

## Calls made
- Toolset: token_diligence
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, contract, pools, wallets]

## Evidence
- Metadata: [name, symbol, supply, verification]
- Market/liquidity: [price, volume, pool depth + venue]
- Holders: [concentration, notable wallets]
- Risk/enrichment: [flags, provider-specific fields]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Missing provider keys, plan gates, stale liquidity, unresolved ambiguity.]

## Next action
[Security drilldown, liquidity venue check, or holder analysis — only if needed.]
```

## Gotchas

- A verified name/logo is not proof of safety.
- High FDV or high volume without pool depth can still be risky.
- Missing enrichment is not the same as a clean risk result.

## Runtime status handling

Use Hive's runtime statuses directly: `ok`, `missing_key`, `plan_required`,
`rate_limited`, `degraded`, `failing`. If one provider is gated (for example
enrichment returns `plan_required`), keep the rest of the diligence report and
mark that section unavailable rather than claiming diligence failed.

## Hand-offs

- User is about to sign, approve, or swap → `hive-security-risk`.
- Pool-level depth, trades, or OHLCV → `hive-dex-pool-analysis`.
- Solana mint or DAS asset → `hive-solana-analysis`.
