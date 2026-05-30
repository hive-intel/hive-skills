# Hive Discovery Surfaces

Use this reference when the exact Hive endpoint is unknown. Discovery is a
first-class workflow, not a fallback after a failed tool call.

## Surfaces

| Surface | Use when | Output |
| --- | --- | --- |
| `hive://toolsets` | The user described a task or workflow | Canonical task toolsets, required inputs, recommended tools |
| `hive://tools` | The user asks what Hive can call | Full catalog summary without flooding root `tools/list` |
| `hive://providers` | The user asks about provider coverage or provenance | Provider names, categories, auth/runtime notes |
| `hive://status` | The user asks if Hive or a provider is healthy | Classified provider/runtime state |
| `hive://skills` | The request matches a recurring workflow | Agent skills and when to apply them |
| `search_tools` | Intent is known but exact endpoint is not | Ranked endpoint/toolset candidates |
| Category listing tools | The user is inside a known category | Scoped tool lists for market, wallet, DeFi, NFT, security, and other domains |
| `get_api_endpoint_schema` | Before any exact invocation | Parameters, required fields, validation shape |
| `invoke_api_endpoint` | After schema validation | Bounded provider call with normalized output and provenance |

## Decision rules

- For broad crypto questions, start with `hive://toolsets`.
- For "what tools do you have?" questions, read `hive://tools`.
- For "which provider/source?" questions, read `hive://providers`.
- For health, quota, key, or degradation questions, read `hive://status`.
- For exact execution, never skip schema lookup unless the schema was already
  loaded in the current turn.

## Search query shape

Good `search_tools` queries combine workflow, domain, chain, and identifier
type:

```json
{
  "query": "wallet investigation ethereum token balances transfers",
  "limit": 5
}
```

```json
{
  "query": "token security ethereum contract honeypot approvals",
  "limit": 5
}
```

```json
{
  "query": "prediction markets event outcomes traders polymarket",
  "limit": 5
}
```

## Common failure states

- `missing_key`: the endpoint exists, but the configured key is absent.
- `plan_required`: the endpoint exists, but the account plan does not allow it.
- `rate_limited`: retry later or reduce the call volume.
- `degraded`: provider returned partial, fallback, cached, or stale data.
- `failing`: provider or Hive path failed. Return the classified failure and
  the next diagnostic step.
