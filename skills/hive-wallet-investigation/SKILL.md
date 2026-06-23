---
name: hive-wallet-investigation
description: Use this skill whenever the user wants to look inside a wallet or address — portfolio, holdings, balances, transfers, PnL, NFT exposure, DeFi positions, whale moves, "what does this address hold", "trace this wallet's activity" — even if they just paste an address. Requires wallet address and chain before executing Hive wallet tools. For Solana wallets use hive-solana-analysis; for "is this address malicious" risk checks use hive-security-risk.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "wallet"
  requires_network: "true"
version: 1.1.0
---

# hive-wallet-investigation — Wallet Investigation

Investigate a wallet's balances, transfers, PnL, NFT exposure, DeFi positions,
and notable activity with live Hive data, scoped to exactly what the user
asked.

## Task toolset and identifiers

Toolset: `wallet_investigation` (confirm against `hive://toolsets` if routing
fails).

- Required: wallet address and chain/network — unless the user explicitly asks
  for multi-chain.
- Optional: time window, asset filter, counterparty, protocol, NFT collection.

Ask for the chain when an EVM address appears without context: the same
address exists on every EVM chain with different state, so guessing the chain
silently changes the answer. For Solana wallets, hand off to
`hive-solana-analysis`.

## Procedure

1. Validate the address format for the requested chain.
2. Call `search_tools` for wallet balance, native balance, transfer, NFT,
   DeFi-position, and PnL capabilities as the question requires.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Call `invoke_api_endpoint` with schema-valid, bounded arguments.
5. Start with balances and native balance. Add transfers for activity
   questions; add NFTs, DeFi positions, or PnL only when relevant.
6. Summarize exposure, concentration, notable activity, and missing data.

## Bounded calls

- Use explicit `limit`, `page`, `per_page`, or time filters for transfers.
- Do not fetch full history for a quick portfolio read.
- Avoid mixing chains unless the user requested multi-chain analysis.

## Worked example

User: "What does 0x28C6c06298d514Db089934071355E5743bf21d60 hold on Ethereum,
and did anything big move this week?"

1. `search_tools` → `{"query": "wallet token balances native balance transfers ethereum", "limit": 5}`
2. `get_api_endpoint_schema` for the balance and transfer endpoints returned.
3. `invoke_api_endpoint` for balances first, then transfers with a 7-day
   window and an explicit limit — argument names come from the schema, not
   from memory.
4. Answer with the report template, flagging spam tokens and truncated
   history.

## Report template

```markdown
## Summary
[Wallet posture and the most notable finding.]

## Calls made
- Toolset: wallet_investigation
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, wallet, filters]

## Evidence
- Balances: [native + top token holdings]
- Activity: [transfers in window, counterparties]
- Exposure: [NFTs, DeFi positions, PnL if requested]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Missing chains, truncated history, unavailable enrichment, degraded providers.]

## Next action
[Chain expansion, transfer drilldown, risk check, or PnL detail — only if needed.]
```

## Gotchas

- Wallet labels and owner identity are not guaranteed.
- Token balances can include spam or illiquid assets — say so rather than
  inflating portfolio value.
- Absence of DeFi/NFT data may reflect provider coverage, not true absence.

## Runtime status handling

Classify provider failures as `missing_key`, `plan_required`, `rate_limited`,
`degraded`, or `failing`. A single blocked enrichment tool should not suppress
balances or transfers that did succeed.

## Hand-offs

- Solana wallet (base58 address) → `hive-solana-analysis`.
- "Is this address dangerous / phishing / sanctioned" → `hive-security-risk`.
- Token-specific deep dive on one holding → `hive-token-diligence`.
