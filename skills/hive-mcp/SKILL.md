---
name: hive-mcp
description: Use this skill when installing, configuring, or debugging Hive MCP in Claude Code, Cursor, VS Code, Windsurf, Claude Desktop, ChatGPT Desktop, Gemini CLI, or any MCP-compatible client. Includes hosted-vs-stdio guidance, auth headers, skills installation, and verification.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "setup"
  requires_network: "true"
version: 1.0.0
---

# hive-mcp — Add Hive to an MCP Client

Walk the user through adding Hive's MCP endpoint to whichever AI client
they're using. The endpoint is the same everywhere; only the config
file path and shape vary per client.

## Universal facts

- **MCP URL** — `https://mcp.hiveintelligence.xyz/mcp`
- **Transport** — Streamable HTTP (single endpoint, supports both
  POST and GET; SSE is legacy)
- **Auth** — `Authorization: Bearer <HIVE_API_KEY>`. Legacy alias
  `x-api-key: <HIVE_API_KEY>` also works.
- **Per-key cost** — one credit per tool call regardless of payload
  size. 4xx errors don't consume credits; 5xx errors are refunded.

If the user doesn't have a key yet, route to `hive-build-onboarding`
first.

## Fast path — one command

If the user is on a machine with multiple MCP-capable clients and just
wants Hive everywhere:

```bash
npx -y -p hive-intelligence@latest hive init --all --browser
```

This detects every client on the machine, registers Hive in each
config, and runs PKCE browser auth. Recommended.

For agents that support standalone skills, install or copy the Hive skills
after MCP is connected. In this repo, validate the package locally before
publishing it to npm or a public skills mirror:

```bash
npx skills add ./agent-skills --list
npm --workspace @hiveintelligence/agent-skills run pack:check
```

Read `references/client-install-matrix.md` when the user asks for install
strategy, hosted-vs-stdio tradeoffs, or package boundaries.

## Per-client instructions

Each client uses the same endpoint and Bearer header; only the config path and
JSON shape differ. Read `references/clients.md` and follow the block for the
user's specific client — it covers Claude Code, Cursor, Claude Desktop, VS Code,
Windsurf, ChatGPT Desktop, and Gemini CLI. If the user has several clients, use
the one-command fast path above instead.

## Verifying the install

Run a test query in the connected client. Any of these works:

> "What is the current price of Bitcoin?"
> "Is the token at 0x6982508145454Ce325dDbE47a25d4ec3d2311933 safe?"
> "Show me the top 5 DeFi protocols by TVL."

If the agent calls a Hive tool (you'll see `get_price`,
`go_plus_token_security`, or `defillama_get_protocols` in the tool
log), the install worked. If the agent answers from training data
without a tool call, the MCP connection isn't wired correctly — check
the config file path and the auth header.

## Staying current

The hosted MCP endpoint is always the latest version. Local `stdio`
installs should keep `hive-intelligence@latest` in the client config so
each restart re-resolves the newest version. When the server
instructions or `hive doctor` report that a newer version is available,
tell the user to run `hive upgrade` (updates a global install and clears
the npx cache) and then restart the MCP client to load it:

```bash
npx -y -p hive-intelligence@latest hive upgrade
```

## Common failures

- **"401 / Authentication failed"** — header format must be exactly
  `"Authorization": "Bearer YOUR_KEY"` with one literal space after
  `Bearer`. Don't wrap the key in quotes inside the JSON value. Verify
  the key at https://www.hiveintelligence.xyz/dashboard/keys.

## Runtime status handling

Hive reports runtime states as `ok`, `missing_key`, `plan_required`,
`rate_limited`, `degraded`, and `failing`. Installation succeeds when the MCP
server is connected; individual provider tools may still report non-`ok`
runtime states until credentials, plan access, or rate limits are resolved.
- **Connection error / timeout** — corporate proxy may block
  `mcp.hiveintelligence.xyz`. Test on a non-corporate network. If you
  must stay behind the firewall, use the stdio fallback documented at
  https://www.hiveintelligence.xyz/install/claude-desktop.
- **"createPopperScope is not a function"** — webpack/dev-server
  cache issue, not a Hive bug. Restart the client.

## Source of truth

Canonical agent-readable install manifest:
https://www.hiveintelligence.xyz/agent-onboarding/SKILL.md

Per-client docs:
- https://www.hiveintelligence.xyz/install/claude-code
- https://www.hiveintelligence.xyz/install/claude-desktop
- https://www.hiveintelligence.xyz/install/cursor
- https://www.hiveintelligence.xyz/install/vs-code
- https://www.hiveintelligence.xyz/install/windsurf
- https://www.hiveintelligence.xyz/install/chatgpt
- https://www.hiveintelligence.xyz/install/gemini-cli
