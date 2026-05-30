---
name: hive-defi-research
description: Use this skill for DeFi protocol and chain research covering TVL, fees, revenue, yields, stablecoins, bridges, protocol slugs, and chain-level DeFi metrics. Use it for current protocol comparisons and freshness-sensitive DeFi analysis.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "defi"
  requires_network: "true"
version: 1.0.0
---

# hive-defi-research — DeFi Protocol Analysis

Use this skill for protocol, TVL, fees, yield, stablecoin, bridge, or chain
DeFi questions.

## Task toolset

Use `defi_protocol_analysis`.

Required identifiers: protocol name or chain.

## Procedure

Read `references/workflow.md` when the request needs protocol comparison,
metric normalization, timestamp caveats, or a structured DeFi report.

1. Resolve the protocol slug or chain name.
2. Pull TVL, fee/revenue, yield, bridge, stablecoin, or chain metrics depending
   on the user question.
3. Compare protocols only with like-for-like metrics and timestamps.
4. State if a metric is unavailable or stale.

## Example

For "compare Aave and Compound", run protocol-level TVL and fee/yield checks for
each, then summarize differences in a table.

## Runtime status handling

Treat unreliable or temporarily unavailable DeFi endpoints as `degraded`.
Do not silently omit a metric; mark it unavailable with the runtime status.
