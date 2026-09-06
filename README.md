# Hive Skills — Agent Skills for Crypto Intelligence

> Live crypto, wallet, token, DeFi, DEX, NFT, Solana, security, network, and market workflows for Claude Code, Cursor, Codex, and any agent — powered by the [Hive Intelligence](https://www.hiveintelligence.xyz) MCP server.

[![Install with skills.sh](https://skills.sh/b/hive-intel/hive-skills)](https://skills.sh/hive-intel/hive-skills)
[![Agent Skills Spec](https://img.shields.io/badge/Agent%20Skills-Specification-blue)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

## Install

```bash
npx skills add hive-intel/hive-skills        # all skills
```

Install a single skill, non-interactively, or for a specific agent:

```bash
npx skills add hive-intel/hive-skills --skill hive-token-diligence --yes
npx skills add hive-intel/hive-skills --skill hive-query --agent claude-code
npx skills add hive-intel/hive-skills --list   # browse without installing
```

Skills are also bundled in the `hive-intelligence` npm package (`npx -y -p hive-intelligence@latest hive init --all --browser`) and installable as a Claude plugin via the bundled `.claude-plugin/plugin.json`.

## Prerequisites

- **Hosted Hive MCP (default):** `https://mcp.hiveintelligence.xyz/mcp` — works with a single Hive API key, no provider setup. Start with the `hive-mcp` skill to wire it up, or `hive-build-onboarding` to get a key.
- **Local stdio / self-host:** optional, for your own provider keys — the `hive-mcp` skill covers both paths.

## Skill catalog

### Setup and Build

Skills for installing Hive MCP, getting API keys, using the Hive CLI, and integrating Hive into application code.

| Skill | Use when |
| --- | --- |
| `hive-mcp` | Add Hive's MCP server to Claude Code, Cursor, VS Code, Windsurf, Claude Desktop, ChatGPT, or Gemini CLI. Per-client instructions. |
| `hive-build-onboarding` | Walk the user through browser-based sign-in. Use when the user has no API key or cannot find theirs. |
| `hive-cli` | Use the `hive` CLI inline from a chat — query prices, scan wallets, check token security, automate briefings. |
| `hive-build` | Integrate Hive into app code via REST or MCP SDK. Python, TypeScript, Go, Java, Rust patterns with retry / async / typed responses. |

### Discovery and Routing

Skills that teach agents how to discover task toolsets, inspect schemas, and execute bounded Hive MCP calls.

| Skill | Use when |
| --- | --- |
| `hive-query` | Route crypto questions through canonical task toolsets, schema lookup, and exact tool invocation. |
| `hive-tool-discovery` | Find the right Hive tool, task toolset, schema, or endpoint before execution. |

### Crypto Research Workflows

Focused workflows for market, token, wallet, security, DEX, DeFi, NFT, Solana, network, and stateful monitoring analysis.

| Skill | Use when |
| --- | --- |
| `hive-market-research` | Research live prices, liquidity, exchanges, OHLC, order books, funding rates, and derivatives. |
| `hive-token-diligence` | Investigate token metadata, market context, liquidity, holders, enrichment, and risk. |
| `hive-wallet-investigation` | Inspect wallet balances, transfers, NFTs, PnL, DeFi positions, and notable activity. |
| `hive-security-risk` | Check token, approval, address, dApp, phishing, contract, and simulation risk. |
| `hive-dex-pool-analysis` | Analyze pools, pairs, liquidity, trades, OHLCV, and DEX flow. |
| `hive-defi-research` | Analyze DeFi protocols, TVL, fees, revenue, stablecoins, bridges, and yields. |
| `hive-nft-research` | Research NFT collections, ownership, metadata, floors, sales, rarity, and spam checks. |
| `hive-solana-analysis` | Analyze Solana wallets, SPL accounts, DAS assets, parsed transactions, and priority fees. |
| `hive-network-infrastructure` | Read chain state, gas, blocks, receipts, logs, transaction status, and RPC diagnostics. |
| `hive-stateful-monitoring` | Create, list, update, and archive durable crypto monitors, alerts, scheduled reports, and agent memory. |

## Try it

Once installed, skills activate when the agent detects a relevant task. Each example shows the phrasing that routes to a given skill:

| Ask your agent | Routes to |
| --- | --- |
| "What is the price and 24h change for ETH right now?" | `hive-query` |
| "Is the token at 0x6982… safe to ape? Run diligence." | `hive-token-diligence` |
| "Trace where this wallet's funds came from on Ethereum." | `hive-wallet-investigation` |
| "What's the BTC perp funding-rate picture across exchanges?" | `hive-market-research` |
| "Show the top DeFi protocols by TVL and 7d fees." | `hive-defi-research` |
| "Add Hive to Cursor and verify it works." | `hive-mcp` |

Answers should report provider provenance, data freshness, and runtime status.

## How it works

Each skill follows the [Agent Skills spec](https://agentskills.io): a `SKILL.md` with YAML `name` + `description` and progressive disclosure — metadata loads at startup, full instructions load on activation, and where a skill carries genuinely deep material (per-client install matrices, root-MCP architecture, B2B subject signing) it lives in `references/` and loads only when needed. Every skill also ships `evals/` with execution test cases and trigger evals. Every Hive skill preserves the same loop: **discovery → schema lookup → bounded execution → diagnostics → provenance-aware answer.**

## Trust & provenance

Data trust is the product. Skills surface the source provider, freshness, and degraded/cached/rate-limited runtime states in grounded answers, and avoid silently mixing provider data. Missing provider keys return a classified `missing_key` state rather than a wrong answer.

## Supported agents

Claude Desktop, Claude Code, Cursor, Windsurf, VS Code (Copilot Chat), OpenAI Responses API, Codex CLI, Gemini CLI, and agents installable via `npx skills` or a `.claude-plugin` marketplace.

## Contributing

This repository is an auto-generated mirror of the Hive monorepo. Report issues or request changes via [GitHub Issues](https://github.com/hive-intel/hive-skills/issues) or support@hiveintelligence.xyz — upstream fixes sync back here.

## Resources

- Hive MCP server: `https://mcp.hiveintelligence.xyz/mcp`
- Docs & API keys: https://www.hiveintelligence.xyz
- skills.sh listing: https://skills.sh/hive-intel/hive-skills
- Agent Skills spec: https://agentskills.io

## License

MIT — see [LICENSE](./LICENSE). Skills are MIT licensed; the Hive MCP server itself is a hosted product.
