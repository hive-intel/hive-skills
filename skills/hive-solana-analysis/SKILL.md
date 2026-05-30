---
name: hive-solana-analysis
description: Use this skill for Solana-specific analysis covering wallets, SPL token accounts, DAS assets, parsed transactions, priority fees, launch data, mints, or Solana NFT assets. Prefer Solana-native identifiers and providers over EVM assumptions.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "solana"
  requires_network: "true"
version: 1.0.0
---

# hive-solana-analysis — Solana Analysis

Use this skill for Solana wallets, mints, SPL accounts, DAS assets, parsed
transactions, priority fees, or Solana token safety questions.

## Task toolset

Use `solana_analysis`.

Required identifiers: Solana wallet, mint, asset id, or transaction signature.

## Procedure

Read `references/workflow.md` when the request needs Solana-native identifier
handling, DAS/SPL routing, priority-fee context, or a structured Solana report.

1. Confirm the identifier type: wallet, mint, asset id, program id, or
   transaction signature.
2. Use Solana-native tools instead of EVM assumptions.
3. For wallets, combine balances, token accounts, assets, and parsed history.
4. For execution context, include slot/priority fee/freshness.

## Example

If the user gives a mint, use token/security or asset search paths. If the user
gives a wallet, use owner/balance paths.

## Runtime status handling

Classify Helius or Solana provider issues as `missing_key`, `rate_limited`,
`degraded`, or `failing`. Ask for a retry only when the status is transient.
