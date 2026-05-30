---
name: hive-cli
description: Use this skill when the user wants to run Hive from a terminal, script, cron job, jq pipeline, shell briefing, or local diagnostic command. Covers CLI auth, status, tool discovery, JSON output, and safe automation.
license: MIT
metadata:
  package: "@hiveintelligence/agent-skills"
  category: "setup"
  requires_network: "true"
version: 1.0.0
---

# hive-cli — Hive From the Terminal

Use this skill when the user says any of:

- "Run a CLI command for…"
- "Show me from the terminal…"
- "Pipe this into jq" / "give me a shell script"
- "Set up a daily briefing"

The Hive CLI lives in the `hive-intelligence` npm package; the
executable name is `hive`. Package and binary names differ on purpose
— the npm package also ships the stdio MCP server and bundled skills
installer. One install, multiple agent entrypoints.

## Bootstrap

If the user doesn't have the CLI installed:

```bash
# One-time install + auth
npx -y -p hive-intelligence@latest hive init --browser

# Or, install globally for repeated use
npm install -g hive-intelligence
hive auth login
```

`init --browser` runs the PKCE flow described in `hive-build-onboarding`.

## Common commands

```bash
hive doctor                              # connectivity + auth check
hive status                              # plan, recent usage, key info
hive tools list                          # full live tool catalog
hive tools search <keyword>              # filter the catalog
hive tools info <tool-name>              # input schema for one tool
hive market price --ids bitcoin --vs usd # price query (domain subcommand)
hive defi tvl --protocol aave            # DeFi TVL query
hive watch '<command>' --interval 30     # tail a query on an interval
hive uninstall --all                     # remove from every client
```

Domain subcommands map directly to Hive's category namespace. Common
ones: `hive market`, `hive defi`, `hive wallet`, `hive security`,
`hive nft`, `hive prediction`. Run `hive --help` for the full list.

## Pipe into jq

The CLI returns JSON by default — perfect for `jq`:

```bash
# Price as a single number
hive market price --ids bitcoin --vs usd | jq '.bitcoin.usd'

# Top DeFi protocol name
hive defi protocols | jq '.[0].name'

# Count tools matching "wallet"
hive tools search wallet --format json | jq 'length'
```

## Daily briefing pattern

```bash
#!/bin/bash
set -euo pipefail

echo "=== Daily Crypto Briefing ==="
echo
echo "--- Watchlist ---"
hive market price --ids bitcoin,ethereum,solana --vs usd \
  | jq -r 'to_entries[] | "\(.key): $\(.value.usd)"'
echo
echo "--- Top Gainers (24h) ---"
hive market gainers-losers --vs usd --duration 24h | jq '.top_gainers[:5]'
echo
echo "--- DeFi TVL Leaders ---"
hive defi protocols \
  | jq '.[:5] | .[] | "\(.name): $\(.tvl / 1000000 | floor)M"'
```

Schedule with cron:

```cron
0 8 * * * /path/to/briefing.sh >> /var/log/crypto-briefing.log 2>&1
```

## Authentication options

1. **`hive auth login`** — interactive PKCE browser flow. Stores key
   in `~/.hive/credentials.json`. Recommended for personal machines.
2. **`HIVE_API_KEY=hive_live_…`** — env var. Recommended for CI,
   Docker, scripts. The CLI reads the env var per-command.
3. **`hive --api-key <key> ...`** — one-shot override. Useful for
   testing a different account without rotating the saved cred.

`hive status` confirms which path the CLI is using.

## Output formats

- `--format json` (default) — clean JSON, ready for jq
- `--format table` — human-friendly columns
- `--format yaml` — yaml output
- `--quiet` — suppress headers, useful in shell pipes

## When to use the CLI vs the MCP

- **CLI** — terminal workflows, cron jobs, shell scripts, one-off
  exploration during a chat session.
- **MCP** — let Claude Code / Cursor / Claude Desktop call tools as
  part of its reasoning. The user doesn't see commands; the agent
  calls tools transparently.

If the user is in Claude Code and asks "what's BTC?", call the MCP
tool. If they ask "give me a shell script that fetches BTC daily",
write a `hive market price …` invocation in the script.

## Runtime status handling

CLI and MCP responses should be interpreted with Hive's runtime states:
`ok`, `missing_key`, `plan_required`, `rate_limited`, `degraded`, and
`failing`. In scripts, retry `rate_limited` and `degraded` with backoff, but
surface `missing_key` or `plan_required` to the operator.

## Reference

- CLI command reference: https://www.hiveintelligence.xyz/cli
- CLI tutorial: https://www.hiveintelligence.xyz/tutorials/cli
- One-command install: https://www.hiveintelligence.xyz/install/claude-skill
