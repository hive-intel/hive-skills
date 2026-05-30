# NFT Research Workflow

Use this reference for NFT collections, ownership, metadata, floors, sales,
rarity, spam checks, and Solana assets.

## Required identifiers

- Chain/network.
- Collection contract, token id, asset id, wallet address, or mint.
- Optional: marketplace, time window, trait filter, or owner.

Ask for chain and contract/asset identifiers when the user gives only a
collection name.

## Tool selection order

1. Resolve collection, token, wallet, or asset id.
2. Use `search_tools` for metadata, owners, floor, sales, rarity, spam, and
   Solana DAS asset tools.
3. Inspect schemas before invocation.
4. Start with metadata/identity.
5. Add floor/sales or owner/rarity only when relevant.

## Bounded calls

- Limit owner and sales lists.
- Keep collection-level and token-level evidence separate.
- Do not treat one marketplace floor as the entire market without caveat.

## Report template

- Summary: collection/token ownership or market read.
- Calls made: endpoints, chain, contract/asset/token identifiers.
- Evidence: metadata, owner/floor/sales/rarity/spam, freshness/runtime status.
- Caveats: marketplace coverage, stale floor, missing metadata, degraded data.
- Next action: trait drilldown, ownership verification, or market comparison.

## Gotchas

- NFT metadata can be mutable or stale.
- Spam checks can lag new collections.
- Solana assets often require Solana-native DAS identifiers.
