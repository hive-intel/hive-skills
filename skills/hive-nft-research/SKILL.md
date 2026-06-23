---
name: hive-nft-research
description: Use this skill when the user asks about NFT collections or assets on EVM chains — ownership, metadata, traits, floor prices, sales, rarity, spam/authenticity checks — like "what's the floor on this collection", "who owns this NFT", "is this collection spam". Use it whenever the user needs current NFT evidence rather than a general explanation of NFTs. For Solana-native assets (mints, DAS, compressed NFTs) use hive-solana-analysis.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "nft"
  requires_network: "true"
version: 1.1.0
---

# hive-nft-research — NFT Research

Research NFT collections and assets — ownership, metadata, floors, sales,
rarity, spam — with collection-level and token-level facts kept separate.

## Task toolset and identifiers

Toolset: `nft_research` (confirm against `hive://toolsets` if routing fails).

- Required: chain/network plus collection contract, token id, asset id, or
  wallet address.
- Optional: marketplace, time window, trait filter, owner.

Ask for chain and contract/asset identifiers when the user gives only a
collection name — collection names are not unique and copycat collections are
common.

## Procedure

1. Resolve collection contract, token id, wallet, or asset id.
2. Call `search_tools` for metadata, owner, floor, sales, rarity, and spam
   capabilities.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Start with metadata/identity; add floor/sales or owner/rarity only when
   relevant.
5. Preserve raw metadata when the user asks about traits or provenance.

## Bounded calls

- Limit owner and sales lists.
- Keep collection-level and token-level evidence separate.
- Do not treat one marketplace floor as the entire market without a caveat.

## Worked example

User: "What's the floor and recent sales for this collection? Contract is
0x… on Ethereum."

1. `search_tools` → `{"query": "nft collection metadata floor price sales ethereum", "limit": 5}`
2. `get_api_endpoint_schema` for the metadata, floor, and sales endpoints
   returned, then `invoke_api_endpoint` with schema-valid arguments and a
   bounded sales window.
3. Report floor (with marketplace scope), recent sales, and freshness using
   the template below.

## Report template

```markdown
## Summary
[Collection/token ownership or market read in one or two sentences.]

## Calls made
- Toolset: nft_research
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, contract, token/asset ids]

## Evidence
- Identity/metadata: [name, supply, verification]
- Market: [floor + marketplace, sales in window]
- Ownership/rarity/spam: [if requested]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Marketplace coverage, stale floor, missing metadata, degraded data.]

## Next action
[Trait drilldown, ownership verification, or market comparison — only if needed.]
```

## Gotchas

- NFT metadata can be mutable or stale.
- Spam checks can lag new collections — "not flagged" is not "authentic".
- Floor price is marketplace-scoped unless the provider aggregates.

## Runtime status handling

If market data is `degraded`, still return the metadata/ownership that
succeeded and label floor or sales metrics as unavailable.

## Hand-offs

- Solana mints, DAS assets, compressed NFTs → `hive-solana-analysis`.
- Wallet-wide NFT exposure → `hive-wallet-investigation`.
- Mint/contract safety before buying → `hive-security-risk`.
