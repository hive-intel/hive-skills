---
name: hive-network-infrastructure
description: Use this skill for chain infrastructure and RPC questions covering gas, blocks, receipts, logs, transaction status, supported networks, fee data, and diagnostics. Use it when the user needs current chain state or transaction evidence.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "network"
  requires_network: "true"
version: 1.0.0
---

# hive-network-infrastructure — Network Infrastructure

Use this skill for block, gas, transaction receipt, logs, supported network, or
RPC diagnostic questions.

## Task toolset

Use `network_infrastructure`.

Required identifiers: chain. Optional identifiers include block number,
transaction hash, contract address, or log filter.

## Procedure

Read `references/workflow.md` when the request needs transaction diagnostics,
RPC/provider comparison, gas/fee context, or a structured network report.

1. Confirm the chain/network id.
2. Use block/gas/chain-id tools for current state.
3. Use transaction, receipt, or logs tools only when the user provides exact
   identifiers.
4. Report the block/slot/fetched time so the answer is auditable.

## Example

If the user asks whether a transaction succeeded, ask for chain and transaction
hash before selecting a receipt or transaction lookup tool.

## Runtime status handling

Use `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, and
`failing`. For transient RPC failures, retry once or fall back to another
network read and state the limitation.
