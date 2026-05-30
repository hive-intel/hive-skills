---
name: hive-wallet-investigation
description: Use this skill for wallet, portfolio, holder, whale, transfer, PnL, NFT exposure, DeFi position, or notable on-chain activity investigations. Require wallet address and chain before executing Hive wallet tools.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "wallet"
  requires_network: "true"
version: 1.0.0
---

# hive-wallet-investigation — Wallet Investigation

Use this skill when the user asks to investigate a wallet, portfolio, holdings,
transfers, PnL, NFT exposure, or DeFi positions.

## Task toolset

Use `wallet_investigation`.

Required identifiers: wallet address and chain.

## Procedure

Read `references/workflow.md` when the request needs multi-chain handling,
activity triage, risk caveats, or a structured wallet investigation report.

1. Validate the address format for the requested chain.
2. Start with balances and native balance.
3. Add transfers, NFT holdings, DeFi positions, or PnL only when relevant.
4. Summarize exposure, concentration, notable activity, and missing data.

## Example

If the user provides an EVM wallet but no chain, ask whether they want Ethereum,
Base, Arbitrum, Polygon, or a multi-chain investigation.

## Runtime status handling

Classify provider failures as `missing_key`, `plan_required`, `rate_limited`,
`degraded`, or `failing`. A single blocked enrichment tool should not suppress
balances or transfers that did succeed.
