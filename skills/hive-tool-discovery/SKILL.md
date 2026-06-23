---
name: hive-tool-discovery
description: Use this skill when the exact Hive MCP tool, task toolset, provider, endpoint name, schema, or argument shape is unknown and hive-query routing was not enough — including "what can Hive do", "which provider covers X", "is Hive healthy", or any failed tool-name guess. Discover first with Hive resources and search_tools, then inspect the schema before any invoke_api_endpoint call.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "discovery"
  requires_network: "true"
version: 1.1.0
---

# hive-tool-discovery — Find The Right Hive Tool

Resolve an ambiguous request or unknown endpoint into an exact, schema-valid
Hive call. Discovery is a first-class workflow, not a fallback after a failed
guess — guessing tool names wastes calls and produces misleading "tool does
not exist" conclusions.

## Procedure

1. Read `hive://toolsets` for task-level routing.
2. Call `search_tools` with the user's intent and optional provider/category.
3. Pick the most specific tool or task toolset.
4. Call `get_api_endpoint_schema` before execution.
5. Call `invoke_api_endpoint` with only schema-valid arguments.

Read `references/discovery-surfaces.md` for the full map of discovery
surfaces — which `hive://` resource answers which question, good
`search_tools` query shapes, and failure-state semantics.

## Worked example

User: "Can Hive tell me which wallets dumped a token right before a rug?"

The exact endpoint is unknown, so search intent first:

```json
{
  "query": "wallet transfers token sells time window ethereum",
  "limit": 5
}
```

Pick the most specific candidate, call `get_api_endpoint_schema` for it, and
only then `invoke_api_endpoint`. If nothing matches, say what was searched
and which nearest capabilities exist — not "Hive cannot do this".

## Runtime status handling

Hive uses `ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`,
and `failing`. Missing keys, plan gates, quota exhaustion, and rate limits
are runtime states. Do not say a tool does not exist unless discovery fails
to find it.

## Guardrails

- Do not hardcode stale endpoint names.
- Use `hive://skills` when the question matches a recurring workflow.
- Use `hive://status` when the user asks whether Hive or a provider is
  healthy.

## Hand-offs

- Routing was already clear → `hive-query` or the matching domain skill.
- The user wants to install or debug the MCP connection itself → `hive-mcp`.
