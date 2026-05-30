# Solana Analysis Workflow

Use this reference for Solana wallets, SPL accounts, DAS assets, parsed
transactions, priority fees, launch data, mints, and Solana NFTs.

## Required identifiers

- Solana wallet, mint, token account, asset id, program id, or transaction
  signature.
- Optional: slot/time window, owner, collection, or token filter.

Do not apply EVM address, chain id, or approval assumptions to Solana requests.

## Tool selection order

1. Classify the identifier: wallet, mint, token account, asset, program, or
   transaction.
2. Use `search_tools` for Solana wallet, SPL token, DAS, parsed transaction,
   priority fee, or launch-data tools.
3. Inspect schemas before invocation.
4. Use wallet paths for owner/balance/history questions.
5. Use mint/asset paths for token/NFT questions.

## Bounded calls

- Limit parsed history, asset lists, and token accounts.
- Include slot or fetched time where available.
- Avoid multi-provider mixing without source labels.

## Report template

- Summary: Solana-specific read.
- Calls made: endpoints, identifiers, filters.
- Evidence: balances/assets/transactions/fees, source/freshness/runtime status.
- Caveats: missing key, rate limit, DAS coverage, stale slot, truncated history.
- Next action: transaction drilldown, asset verification, or risk check.

## Gotchas

- Wallets, mints, token accounts, and asset ids are different identifiers.
- Priority fee estimates are time-sensitive.
- Parsed transaction coverage can vary by provider and transaction type.
