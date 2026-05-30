---
name: hive-tool-discovery
description: Use this skill when the exact Hive MCP tool, task toolset, provider, endpoint name, schema, or argument shape is unknown. Discover first with Hive resources and search_tools, then inspect schema before any invoke_api_endpoint call.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "discovery"
  requires_network: "true"
version: 1.0.0
---

# hive-tool-discovery — Find The Right Hive Tool

Use this skill before calling Hive when the user's request is ambiguous or the
exact endpoint name is unknown.

## Procedure

Read `references/discovery-surfaces.md` when the user asks what discovery
surfaces exist, how agents know available endpoints, or when to use resources
instead of tool calls.

1. Read `hive://toolsets` for task-level routing.
2. Call `search_tools` with the user's intent and optional provider/category.
3. Pick the most specific tool or task toolset.
4. Call `get_api_endpoint_schema` before execution.
5. Call `invoke_api_endpoint` with only schema-valid arguments.

## Example

```json
{
  "query": "wallet investigation ethereum balances transfers",
  "limit": 5
}
```

## Runtime status handling

Hive uses `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, and
`failing`. Missing keys, plan gates, quota exhaustion, and rate limits are
runtime states. Do not say a tool does not exist unless discovery fails to find
it.

## Guardrails

- Do not hardcode stale endpoint names.
- Use `hive://skills` when the question matches a recurring workflow.
- Use `hive://status` when the user asks whether Hive or a provider is healthy.
