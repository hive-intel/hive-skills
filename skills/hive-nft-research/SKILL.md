---
name: hive-nft-research
description: Use this skill for NFT collection, ownership, metadata, floor, sale, rarity, spam, or Solana asset research. Use it whenever the user needs current NFT evidence rather than a general explanation of NFTs.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "nft"
  requires_network: "true"
version: 1.0.0
---

# hive-nft-research — NFT Research

Use this skill for NFT ownership, metadata, collection, floor, sale, rarity, or
spam/authenticity questions.

## Task toolset

Use `nft_research`.

Required identifiers: chain and collection contract or asset id.

## Procedure

Read `references/workflow.md` when the request needs collection resolution,
ownership/floor/sale context, spam caveats, or a structured NFT report.

1. Resolve collection contract, token id, wallet, or asset id.
2. Pull metadata, owners, floors, sales, rarity, or spam checks as requested.
3. Separate collection-level and token-level facts.
4. Preserve raw metadata when the user asks about traits or provenance.

## Example

For a collection-level question, ask for the chain and contract if the user only
gave a collection name.

## Runtime status handling

If market data is `degraded`, still return metadata/ownership that succeeded
and label floor or sales metrics as unavailable.
