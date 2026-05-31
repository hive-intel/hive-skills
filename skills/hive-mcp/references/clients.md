# Per-client install instructions

The MCP endpoint is the same everywhere — `https://mcp.hiveintelligence.xyz/mcp`
with `Authorization: Bearer <HIVE_API_KEY>`. Only the config file path and JSON
shape vary. Read the block for the user's specific client.

## Claude Code

```bash
claude mcp add --transport http hive https://mcp.hiveintelligence.xyz/mcp \
  --header "Authorization: Bearer $HIVE_API_KEY"
```

Add `--scope user` to install globally, or `--scope project` to commit to the
repo's `.mcp.json`. Default scope is local. Restart isn't required — tools
appear in the next chat session.

## Cursor

Edit `~/.cursor/mcp.json`. Add the `hive` server block:

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

Reload the MCP server list from Cursor's command palette (`Cmd+Shift+P` → "MCP:
Reload Servers"). Or use the deeplink:

```
cursor://anysphere.cursor-deeplink/mcp/install?name=hive&config=<base64-encoded-config>
```

## Claude Desktop

Settings → Developer → Edit Config. The file opens in your default editor. Same
shape as Cursor:

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

Quit Claude Desktop fully (⌘Q on macOS — the menu bar icon must disappear) and
reopen. Tools become available on next chat.

## VS Code (GitHub Copilot Chat)

Requires VS Code 1.101 or newer. Create `.vscode/mcp.json` in the project root
(or use User Settings for a global install):

```json
{
  "servers": {
    "hive": {
      "type": "http",
      "url": "https://mcp.hiveintelligence.xyz/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_HIVE_API_KEY"
      }
    }
  }
}
```

Reload the window. Note the `type: "http"` field — this is required in VS Code's
schema and differs from Cursor / Claude Desktop.

## Windsurf

Edit `~/.codeium/windsurf/mcp_config.json`:

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

Reload Windsurf.

## OpenAI Responses API

Use Hive as a server-side remote MCP tool from your application code. Keep the
Hive API key on your server and pass it in the remote MCP tool headers.

Direct ChatGPT app connector setup is beta and auth-dependent; do not promise a
verified bearer-token ChatGPT app connector unless Hive has shipped the auth
shape that workspace requires.

## Codex CLI

Edit `~/.codex/config.toml`:

```toml
[mcp_servers.hive]
url = "https://mcp.hiveintelligence.xyz/mcp"
bearer_token_env_var = "HIVE_API_KEY"
```

Set `HIVE_API_KEY` in the shell that launches Codex, then verify with `codex
/mcp` after restart.

## Gemini CLI

Edit `~/.gemini/settings.json`:

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
