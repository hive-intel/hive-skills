---
name: hive-token-diligence
description: Use this skill when the user asks whether a token is real, liquid, risky, enriched, investable, tradeable, or worth researching further. Investigate exact chain and token identifiers across metadata, market context, holders, DEX liquidity, enrichment, and risk signals.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "token"
  requires_network: "true"
version: 1.0.0
---

# hive-token-diligence — Token Diligence

Use this skill when the user asks whether a token is real, liquid, risky, or
worth researching further.

## Task toolset

Use `token_diligence`.

Required identifiers: chain and token contract or exact token id.

## Procedure

Read `references/workflow.md` when the request needs multi-source diligence,
risk/liquidity prioritization, fallback handling, or a structured token report.

1. Resolve the token with exact chain and contract.
2. Pull metadata, price, pair/liquidity, holder, and risk tools as needed.
3. Separate facts from interpretation: metadata, market, liquidity, holders,
   security, enrichment.
4. Preserve raw provider details when a risk flag depends on source-specific
   fields.

## Example

For Ethereum USDC, use the chain plus contract address fixture rather than just
the symbol `USDC`.

## Runtime status handling

Use Hive's runtime statuses directly. If Moralis enrichment is
`plan_required`, keep the rest of the diligence report and mark enrichment as
unavailable rather than claiming token diligence failed.
