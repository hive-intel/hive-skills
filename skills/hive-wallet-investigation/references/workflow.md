# Wallet Investigation Workflow

Use this reference for wallet balances, transfers, PnL, NFTs, DeFi positions,
and notable activity.

## Required identifiers

- Wallet address.
- Chain/network, unless the user explicitly asks for multi-chain.
- Optional: time window, asset filter, counterparty, protocol, or NFT
  collection.

Ask for chain when an EVM address appears without context. For Solana wallets,
use the Solana skill when the request is Solana-specific.

## Tool selection order

1. Validate the identifier shape for the requested chain.
2. Use `search_tools` for wallet balance, native balance, transfer, NFT, DeFi,
   and PnL tools.
3. Inspect schemas before endpoint invocation.
4. Start with balances/native balance.
5. Add transfers for activity questions.
6. Add NFTs, DeFi positions, or PnL only when relevant.

## Bounded calls

- Use explicit `limit`, `page`, `per_page`, or time filters for transfers.
- Do not fetch full history for a quick portfolio read.
- Avoid mixing chains unless the user requested multi-chain analysis.

## Report template

- Summary: wallet posture and notable finding.
- Calls made: endpoints, chain, wallet, filters.
- Evidence: balances, transfers, exposure, provider/freshness/runtime status.
- Caveats: missing chains, truncated history, unavailable enrichment, degraded
  providers.
- Next action: chain expansion, transfer drilldown, risk check, or PnL detail.

## Gotchas

- Wallet labels and owner identity are not guaranteed.
- Token balances can include spam or illiquid assets.
- Absence of DeFi/NFT data may reflect provider coverage, not true absence.
