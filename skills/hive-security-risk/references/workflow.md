# Security Risk Workflow

Use this reference for token, address, approval, dApp, phishing, contract, and
transaction simulation risk.

## Required identifiers

- Chain/network.
- One or more of: token contract, wallet address, spender address, dApp URL,
  contract address, or transaction payload.

Ask for missing chain and actor details before approval or simulation checks.
Never invent a spender, owner, calldata, or URL.

## Tool selection order

1. Identify the risk object: token, address, approval, dApp, contract, or
   transaction.
2. Use `search_tools` for the matching security/risk endpoints.
3. Inspect schemas before invocation.
4. Run the narrowest risk check first.
5. Add simulation or approval analysis only when the required payload/owner
   details exist.

## Bounded calls

- Keep risk checks scoped to the provided contract, address, or payload.
- Avoid broad scans unless the user explicitly asks for an audit.
- Use severity and evidence rather than unsupported certainty.

## Report template

- Summary: risk level and safest recommendation.
- Calls made: endpoints, chain, risk object, identifiers.
- Evidence: flags, simulation results, approval exposure, source/freshness.
- Caveats: missing payload, incomplete provider coverage, degraded/rate-limited
  provider.
- Next action: revoke approval, avoid signing, retry simulation, or provide
  missing payload.

## Gotchas

- A token can pass metadata checks and still be risky.
- `plan_required` means the tool exists but cannot run under current upstream
  credentials.
- Do not provide signing instructions when a critical risk is unresolved.
