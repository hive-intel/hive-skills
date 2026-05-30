# Hive MCP Client Install Matrix

Hive's default product path is the hosted remote MCP endpoint:

```text
https://mcp.hiveintelligence.xyz/mcp
```

Use local `stdio` when the user is developing Hive itself, self-hosting, testing
provider keys, or using a desktop client that only supports local commands.

## Package boundaries

| Package | Role | Who installs it |
| --- | --- | --- |
| `hive-intelligence` | MCP server, CLI, local stdio runtime, bundled skills installer. Exposes `hive`, `hive-intelligence`, and `hive-mcp` binaries. | End users, self-hosters, desktop clients |
| `hive-mcp-client` | Typed adapter for apps and agent frameworks | Developers integrating Hive into code |
| `@hiveintelligence/agent-skills` | Skills-only corpus for agents and skill registries | Agents, skill package managers, docs mirrors |
| `hive-intel` | Packaged user-facing CLI/docs surface retained for compatibility | Existing CLI/docs consumers |

## Recommended setup flow

1. Install hosted MCP first unless the user explicitly needs local `stdio`.
2. Add the API key as `Authorization: Bearer <HIVE_API_KEY>`.
3. Install or copy skills so the agent knows the workflow layer.
4. Verify by asking a live query and checking that the agent calls Hive instead
   of answering from memory.

## Hosted remote MCP

Use for most agents:

```json
{
  "mcpServers": {
    "hive": {
      "url": "https://mcp.hiveintelligence.xyz/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_HIVE_API_KEY"
      }
    }
  }
}
```

## Local stdio MCP

Use when a client requires a command-based server:

```json
{
  "mcpServers": {
    "hive": {
      "command": "npx",
      "args": ["-y", "-p", "hive-intelligence@latest", "hive"],
      "env": {
        "HIVE_API_KEY": "YOUR_HIVE_API_KEY"
      }
    }
  }
}
```

## Skills install

Use the Hive CLI for detected local clients:

```bash
npx -y -p hive-intelligence@latest hive init --all --browser
```

Use the public skills CLI only after the dedicated GitHub skills mirror is
published and verified in CI. Before that, validate the local repo package:

```bash
npx skills add ./agent-skills --list
```

Check the npm package contents before publishing:

```bash
npm --workspace @hiveintelligence/agent-skills run pack:check
```
