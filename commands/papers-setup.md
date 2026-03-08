---
description: Set up the claude-papers MCP server for the current project. Installs npm dependencies and registers the server in .mcp.json in the current working directory.
argument-hint: "[PAPERS_HOST] [PAPERS_TOKEN]  (e.g. https://papers.wiolett.net tok_...)"
---

# Papers Setup

You are setting up the `claude-papers` MCP server for the **current project** (scoped to the current working directory).

## Steps

### 1. Find plugin install path

```bash
echo "$CLAUDE_PLUGIN_ROOT"
```

If empty, find it:

```bash
find ~/.claude/plugins/cache -name "plugin.json" -path "*/claude-papers*" 2>/dev/null | head -1 | xargs dirname | xargs dirname
```

Store this as `PLUGIN_PATH`.

### 2. Get PAPERS_HOST and PAPERS_TOKEN

Parse `$ARGUMENTS` — it may contain one or both values in the form `https://... tok_...`.

- If `$ARGUMENTS` contains a URL (starts with `http`), use it as `PAPERS_HOST`.
- If `$ARGUMENTS` contains a token (starts with `tok_`), use it as `PAPERS_TOKEN`.

For any missing values, check the environment:
```bash
echo "${PAPERS_HOST:-NOT_SET}"
echo "${PAPERS_TOKEN:-NOT_SET}"
```

If still missing, ask the user:
> Please provide your Papers instance URL (e.g. https://papers.wiolett.net):
> Please provide your Papers API token (tok_...):

### 3. Install npm dependencies

```bash
cd "$PLUGIN_PATH" && npm install --prefer-offline 2>&1 | tail -5
```

### 4. Get current working directory

```bash
pwd
```

Store this as `CWD`.

### 5. Register MCP server in .mcp.json

Read the existing `$CWD/.mcp.json` (may not exist). Merge in the papers entry and write it back:

```json
{
  "mcpServers": {
    "papers": {
      "command": "<PLUGIN_PATH>/node_modules/.bin/tsx",
      "args": ["<PLUGIN_PATH>/src/index.ts"],
      "env": {
        "PAPERS_HOST": "<PAPERS_HOST>",
        "PAPERS_TOKEN": "<PAPERS_TOKEN>"
      }
    }
  }
}
```

Replace `<PLUGIN_PATH>`, `<PAPERS_HOST>`, and `<PAPERS_TOKEN>` with the actual values.

### 6. Report result

Tell the user:
- Plugin path: `PLUGIN_PATH`
- MCP server registered in `CWD/.mcp.json`
- **Restart Claude Code** to activate the `papers` MCP tools
- Run `/claude-papers:papers-setup` again in any other project where you want Papers access

---

## Troubleshooting

**npm install fails** — ensure Node.js 18+ is installed (`node --version`)

**Token rejected** — generate a new token in Papers: profile dropdown → Settings → API Tokens

**MCP server not appearing** — fully quit and relaunch Claude Code after editing `.mcp.json`
