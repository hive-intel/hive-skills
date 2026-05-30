# Hive Intelligence — Agent Skills

Installable [agent skills](https://github.com/vercel-labs/skills) that teach Claude Code, Cursor, Codex, and other agents how to use [Hive Intelligence](https://www.hiveintelligence.xyz) — the agent-facing crypto intelligence MCP server. Covers MCP setup, tool discovery, and live crypto research workflows across market data, wallets, DeFi, DEX, NFTs, security, Solana, network infrastructure, and prediction markets.

## Install

Install every skill:

```bash
npx skills add hive-intel/hive-skills
```

Install a single skill:

```bash
npx skills add hive-intel/hive-skills@hive-mcp
```

Skills are also bundled in the `hive-intelligence` npm package (`npx -y -p hive-intelligence@latest hive init --all --browser`) and installable as a Claude plugin via `.claude-plugin/plugin.json`.

## Skills

### Setup and Build

Skills for installing Hive MCP, getting API keys, using the Hive CLI, and integrating Hive into application code.

- **`hive-mcp`** — Add Hive's MCP server to Claude Code, Cursor, VS Code, Windsurf, Claude Desktop, ChatGPT, or Gemini CLI. Per-client instructions.
- **`hive-build-onboarding`** — Walk the user through PKCE-based browser auth. Use when the user has no API key or cannot find theirs.
- **`hive-cli`** — Use the `hive` CLI inline from a chat — query prices, scan wallets, check token security, automate briefings.
- **`hive-build`** — Integrate Hive into app code via REST or MCP SDK. Python, TypeScript, Go, Java, Rust patterns with retry / async / typed responses.

### Discovery and Routing

Skills that teach agents how to discover task toolsets, inspect schemas, and execute bounded Hive MCP calls.

- **`hive-query`** — Route crypto questions through canonical task toolsets, schema lookup, and exact tool invocation.
- **`hive-tool-discovery`** — Find the right Hive tool, task toolset, schema, or endpoint before execution.

### Crypto Research Workflows

Focused workflows for market, token, wallet, security, DEX, DeFi, NFT, Solana, network, prediction-market, and stateful monitoring analysis.

- **`hive-market-research`** — Research live prices, liquidity, exchanges, OHLC, order books, funding rates, and derivatives.
- **`hive-token-diligence`** — Investigate token metadata, market context, liquidity, holders, enrichment, and risk.
- **`hive-wallet-investigation`** — Inspect wallet balances, transfers, NFTs, PnL, DeFi positions, and notable activity.
- **`hive-security-risk`** — Check token, approval, address, dApp, phishing, contract, and simulation risk.
- **`hive-dex-pool-analysis`** — Analyze pools, pairs, liquidity, trades, OHLCV, and DEX flow.
- **`hive-defi-research`** — Analyze DeFi protocols, TVL, fees, revenue, stablecoins, bridges, and yields.
- **`hive-nft-research`** — Research NFT collections, ownership, metadata, floors, sales, rarity, and spam checks.
- **`hive-solana-analysis`** — Analyze Solana wallets, SPL accounts, DAS assets, parsed transactions, and priority fees.
- **`hive-network-infrastructure`** — Read chain state, gas, blocks, receipts, logs, transaction status, and RPC diagnostics.
- **`hive-prediction-markets`** — Research prediction markets, events, outcomes, market stats, traders, holders, and trades.
- **`hive-stateful-monitoring`** — Create, list, update, and archive durable crypto monitors, alerts, scheduled reports, and agent memory.

## Source

This repository is generated from the private Hive monorepo and kept in sync automatically — do not hand-edit. The skills are MIT licensed; the Hive MCP server itself is a hosted product at `https://mcp.hiveintelligence.xyz/mcp`.

## License

MIT — see [LICENSE](./LICENSE).
