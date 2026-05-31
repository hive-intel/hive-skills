---
name: hive-stateful-monitoring
description: Use this skill when a user asks Hive to remember, monitor, schedule, alert, or report on crypto intelligence across sessions. Creates durable monitor intent through Hive's stateful monitoring tools.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "stateful-monitoring"
  requires_network: "true"
version: 1.0.0
---

# hive-stateful-monitoring - Stateful Monitoring

Use this skill when the user wants Hive to remember a wallet, token, protocol,
market, prediction market, watchlist digest, token discovery risk workflow, or
risk watch workflow and keep reporting on it later.

## Task toolset

Use `stateful_monitoring`.

Required identifiers: monitor kind and target object.

For B2B adapter mode, Hive state is isolated by server-injected subject context:
`partner API key -> Hive owner user_id -> tenant_id -> end_user_id -> subject_id`.
The adapter must sign `METHOD + "\n" + PATH + "\n" + TENANT_ID + "\n" +
END_USER_ID + "\n" + TIMESTAMP` with HMAC-SHA256 and send the
`X-Hive-Tenant-Id`, `X-Hive-End-User-Id`, `X-Hive-Subject-Timestamp`, and
`X-Hive-Subject-Signature` headers. Never ask the model or end user to provide
`__hive_user_id` or `__hive_subject_id`; those are hidden server-injected args.
In TypeScript backends, use
`hive-mcp-client` (`npm install hive-mcp-client`) with `subjectSigningSecret` and per-call
`subject`/`withSubject(...)` context instead of hand-building these headers.

## Procedure

1. Convert the user's durable intent into a monitor `kind`, `target`, `rules`,
   and `cadence`.
2. Use `hive_create_monitor` for new watch requests.
3. Use `hive_list_monitors` before creating a duplicate monitor.
4. Use `hive_update_monitor` to change cadence, rules, target, metadata, or
   status.
5. Use `hive_archive_monitor` when the user asks Hive to stop watching.
6. Use `hive_get_monitor_runs`, `hive_list_observations`,
   `hive_list_alerts`, `hive_get_latest_snapshot`, or
   `hive_generate_monitor_report` when the user asks what changed, what is
   alerting, or what Hive remembers from previous runs.
7. Use `hive_update_alert_status` when the user acknowledges, reopens, or
   resolves an alert.
8. Use `hive_remember_fact`, `hive_list_memory_facts`, and
   `hive_forget_memory_fact` for durable user-scoped facts that should appear
   in future monitor reports.
9. Use `hive_list_subjects`, `hive_get_subject`, `hive_archive_subject`, and
   `hive_list_subject_audit_events` only when operating or auditing a B2B
   adapter's downstream state boundaries.
10. Explain that monitor execution is handled by Hive workers, not by the
   current chat session. Worker-supported monitor kinds are wallet, token,
   protocol, market, prediction_market, watchlist_digest,
   token_discovery_risk, and risk_watch.

## Example

For "Watch this Ethereum wallet and tell me when it moves more than $100k",
create a wallet monitor with an Ethereum address target, a large-transfer rule,
and the requested cadence or default cadence.

For "Send every customer a daily crypto watchlist brief", create a
`watchlist_digest` monitor per B2B subject with saved wallets, tokens,
protocols, markets, and optional prediction markets in the target.

## Runtime status handling

If stateful tools return `missing_key` or an auth-required error, explain that
hosted MCP authentication or the Hive persistence backend must be configured
before durable monitoring can be used. Do not pretend the monitor was saved.
