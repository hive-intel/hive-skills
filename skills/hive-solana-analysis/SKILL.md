---
name: hive-solana-analysis
description: Use this skill for anything Solana-native — wallets, mints, SPL token accounts, DAS assets, parsed transactions, priority fees, pump.fun launches, Solana NFTs, Solana token safety. Prefer it over EVM-shaped skills whenever the identifier is a base58 address, mint, or transaction signature, even if the user never says "Solana". Uses Solana-native identifiers and providers instead of EVM assumptions.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "solana"
  requires_network: "true"
version: 1.1.0
---

# hive-solana-analysis — Solana Analysis

Analyze Solana wallets, mints, SPL accounts, DAS assets, parsed transactions,
priority fees, and launch data with Solana-native tools — never EVM
assumptions.

## Task toolset and identifiers

Toolset: `solana_analysis` (confirm against `hive://toolsets` if routing
fails).

- Required: Solana wallet, mint, token account, asset id, program id, or
  transaction signature.
- Optional: slot/time window, owner, collection, token filter.

Wallets, mints, token accounts, and asset ids are different identifier types —
classify before selecting tools. Do not apply EVM address, chain-id, or
approval assumptions to Solana requests.

## Procedure

1. Classify the identifier: wallet, mint, token account, asset, program, or
   transaction signature.
2. Call `search_tools` for Solana wallet, SPL token, DAS, parsed-transaction,
   priority-fee, or launch-data capabilities.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Use wallet paths for owner/balance/history questions; mint/asset paths for
   token/NFT questions.
5. For execution context, include slot, priority fee, and freshness.

## Bounded calls

- Limit parsed history, asset lists, and token accounts.
- Include slot or fetched time where available.
- Avoid multi-provider mixing without source labels.

## Worked example

User: "This pump.fun token is mooning — can you check the mint
<base58 mint> for safety and who holds it?"

1. Classify: a base58 mint → token/security and asset paths, not wallet paths.
2. `search_tools` → `{"query": "solana mint token security holders launch data", "limit": 5}`
3. `get_api_endpoint_schema` for the mint-security and holder endpoints
   returned, then `invoke_api_endpoint` with schema-valid arguments.
4. Report safety flags, holder concentration, and launch context with slot or
   `fetched_at` freshness.

If the user gives a wallet instead, use owner/balance/token-account paths and
add parsed history only for activity questions.

## Report template

```markdown
## Summary
[Solana-specific read in one or two sentences.]

## Calls made
- Toolset: solana_analysis
- Endpoint(s): [exact endpoint names]
- Identifiers: [wallet/mint/asset/signature, filters]

## Evidence
- Balances/assets/transactions/fees: [as relevant]
- Provenance: [provider, slot or fetched_at, runtime status per call]

## Caveats
[Missing key, rate limit, DAS coverage, stale slot, truncated history.]

## Next action
[Transaction drilldown, asset verification, or risk check — only if needed.]
```

## Gotchas

- Wallets, mints, token accounts, and asset ids are different identifiers.
- Priority-fee estimates are time-sensitive.
- Parsed-transaction coverage can vary by provider and transaction type.

## Runtime status handling

Classify Helius or other Solana provider issues as `missing_key`,
`rate_limited`, `degraded`, or `failing`. Ask for a retry only when the status
is transient.

## Hand-offs

- EVM wallets and tokens → `hive-wallet-investigation` / `hive-token-diligence`.
- EVM NFT collections → `hive-nft-research`.
- Cross-chain market prices → `hive-market-research`.
