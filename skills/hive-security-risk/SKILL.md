---
name: hive-security-risk
description: Use this skill before advising on swaps, approvals, signatures, dApps, contracts, URLs, or transaction payloads where token, address, approval, phishing, or simulation risk matters. Report severity, evidence, and remediation instead of guessing.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "security"
  requires_network: "true"
version: 1.0.0
---

# hive-security-risk — Security Risk

Use this skill before explaining swaps, approvals, signatures, dApps, or
unknown contracts.

## Task toolset

Use `security_risk`.

Required identifiers: chain and token contract, wallet address, spender,
dApp URL, or transaction payload depending on the request.

## Procedure

Read `references/workflow.md` when the request involves signing, approvals,
dApps, simulations, or any risk report that needs severity and remediation.

1. Identify the asset, address, approval, dApp, or transaction payload.
2. Run token/address/approval/phishing/simulation checks as appropriate.
3. Report severity, evidence, and remediation steps.
4. Do not provide signing guidance when critical risk is unresolved.

## Example

For an ERC-20 approval concern, collect chain, token contract, owner wallet, and
spender before selecting the approval or allowance tool.

## Runtime status handling

`rate_limited` and `degraded` can be retried with backoff. `plan_required` means
the tool exists but the upstream account cannot execute it right now.
