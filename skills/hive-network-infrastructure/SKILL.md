---
name: hive-network-infrastructure
description: Use this skill when the user asks about chain state or transaction plumbing — gas prices, blocks, transaction receipts and status ("did my tx go through"), event logs, supported networks, fee data, or RPC diagnostics. Use it whenever the answer needs current chain-level evidence. For wallet holdings use hive-wallet-investigation; for Solana slots/fees use hive-solana-analysis.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "network"
  requires_network: "true"
version: 1.1.0
---

# hive-network-infrastructure — Network Infrastructure

Read chain state — gas, blocks, receipts, logs, transaction status, supported
networks — and run RPC diagnostics with auditable, timestamped evidence.

## Task toolset and identifiers

Toolset: `network_infrastructure` (confirm against `hive://toolsets` if
routing fails).

- Required: chain/network.
- Optional: block number/hash, transaction hash, contract address, log
  filter, account address, time window.

Ask for the exact chain plus transaction/block identifiers before status
checks — a transaction hash is not globally unique without chain context.

## Procedure

1. Confirm the chain/network id.
2. Call `search_tools` for gas, block, transaction, receipt, log,
   supported-network, or RPC-diagnostic capabilities.
3. Call `get_api_endpoint_schema` for each endpoint before calling it.
4. Use current-state tools for gas and block questions; use
   transaction/receipt/log tools only with exact identifiers.
5. Report the block/slot/fetched time so the answer is auditable.

## Bounded calls

- Bound log queries by block range and topics.
- Do not run broad chain scans for a single transaction question.
- Retry transient RPC failures once when appropriate.

## Worked example

User: "Did my transaction go through? Hash is 0x… — I think it was on Base."

1. Confirm the chain (Base) and the exact hash.
2. `search_tools` → `{"query": "transaction receipt status base", "limit": 5}`
3. `get_api_endpoint_schema` for the receipt/status endpoint returned, then
   `invoke_api_endpoint` with schema-valid arguments.
4. Report status, block number, gas used, and the fetched time. If the
   receipt is missing, say whether that means pending, dropped, or wrong
   chain — do not guess success.

## Report template

```markdown
## Summary
[Network or transaction state in one or two sentences.]

## Calls made
- Toolset: network_infrastructure
- Endpoint(s): [exact endpoint names]
- Identifiers: [chain, hash/block/address, filters]

## Evidence
- State: [block/slot, gas/fee, receipt/log/status]
- Provenance: [provider, fetched_at, runtime status per call]

## Caveats
[Provider mismatch, stale block, rate limit, incomplete logs.]

## Next action
[Narrower log filter, alternate provider, or retry — only if needed.]
```

## Gotchas

- A transaction hash is not globally unique without chain context.
- Logs require bounded block ranges.
- HTTP health does not prove provider data freshness.

## Runtime status handling

Use `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, and
`failing`. For transient RPC failures, retry once or fall back to another
network read and state the limitation.

## Hand-offs

- Wallet balances/holdings rather than chain state → `hive-wallet-investigation`.
- Solana slots, priority fees, parsed transactions → `hive-solana-analysis`.
- Gas context for a risky transaction → pair with `hive-security-risk`.
