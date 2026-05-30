# Network Infrastructure Workflow

Use this reference for gas, blocks, receipts, logs, transaction status,
supported networks, fee data, and RPC diagnostics.

## Required identifiers

- Chain/network.
- Optional: transaction hash, block number/hash, contract address, log topic,
  account address, or time window.

Ask for exact chain and transaction/block identifiers before status checks.

## Tool selection order

1. Resolve chain/network id.
2. Use `search_tools` for gas, block, transaction, receipt, log, supported
   network, or RPC diagnostic tools.
3. Inspect schemas before invocation.
4. Use current-state tools for gas and block questions.
5. Use transaction/receipt/log tools only with exact identifiers.

## Bounded calls

- Bound log queries by block range and topics.
- Do not run broad chain scans for a single transaction question.
- Retry transient RPC failures once when appropriate.

## Report template

- Summary: network or transaction state.
- Calls made: endpoints, chain, identifiers, filters.
- Evidence: block/slot, gas/fee, receipt/log/status, freshness/runtime status.
- Caveats: provider mismatch, stale block, rate limit, incomplete logs.
- Next action: narrower log filter, alternate provider, or retry.

## Gotchas

- A transaction hash is not globally unique without chain context.
- Logs require bounded block ranges.
- HTTP health does not prove provider data freshness.
