# run-workflow-weaver

**Turn API docs into workflows and diagrams.** This Clawhub skill drives [Workflow Weaver](https://weaver.vibingfun.com) via its [npm CLI](https://www.npmjs.com/package/workflow-weaver) and MCP server, giving AI agents the ability to generate process documentation, visual diagrams, and step-by-step guides from API specifications, OpenAPI files, and code snippets.

## What Can It Do?

- **Generate workflows** from your API endpoints → get step-by-step process guides
- **Create diagrams** → SVG, PNG, or PDF visual workflow maps
- **Export documentation** → Markdown guides with full context
- **Manage projects** → Create, version, share via 23 MCP tools or CLI
- **BYOK support** → Use your own AI provider keys, bypass quotas

## Why This Skill?

If you have:
- An OpenAPI spec and need a visual diagram of the request flow
- API endpoints and want a step-by-step integration guide
- Code snippets that need to become process documentation
- A need to version and share workflow documentation

This skill gives your agent the tools to do it headlessly — no browser, no UI, just subprocess calls or MCP tool invocations.

## What is Workflow Weaver?

[Workflow Weaver](https://weaver.vibingfun.com) is a web app that generates workflows and diagrams from API specifications. This skill lets your agent drive it programmatically via:
- **CLI** (`workflow-weaver`) — subprocess calls with `--json` output
- **MCP server** (`@workflow-weaver/mcp`) — 23 tools for Claude, Cursor, Kiro, etc.

## Installation

### Via ClawHub

```bash
clawhub install run-workflow-weaver
```

### Manual

Copy `.claude/skills/run-workflow-weaver/` into your agent's skills directory.

### Prerequisites

- Node.js ≥ 20
- jq (for JSON parsing in shell examples)
- Global CLI + MCP server:
  ```bash
  npm install -g workflow-weaver @workflow-weaver/mcp
  ```

## Quick Start

```bash
# 1. Authenticate (interactive, one-time)
workflow-weaver auth login --email you@example.com

# 2. Export your token for agent use
export WORKFLOW_WEAVER_REFRESH_TOKEN=$(node -e "process.stdout.write(require(require('os').homedir()+'/.workflow-weaver/config.json').refreshToken)")

# 3. Create a project
PROJECT=$(workflow-weaver projects create \
  --title "Stripe Integration Guide" \
  --use-case "Payment workflow documentation" \
  --json)
PROJECT_ID=$(echo "$PROJECT" | jq -r '.id')

# 4. Add your API spec as a source
workflow-weaver sources add "$PROJECT_ID" \
  --type openapi_file \
  --file ./stripe-openapi.yaml \
  --json

# 5. Generate the workflow (streams progress as NDJSON)
GENERATE_OUTPUT=$(workflow-weaver generate "$PROJECT_ID" --json)
VERSION_ID=$(echo "$GENERATE_OUTPUT" | tail -1 | jq -r '.versionId // .id')

# 6. Export as Markdown
workflow-weaver export "$PROJECT_ID" "$VERSION_ID" --format md
```

## Files

| File | Purpose |
|------|---------|
| `.claude/skills/run-workflow-weaver/SKILL.md` | Agent-facing instructions, 23 MCP tools reference, security rules |
| `.claude/skills/run-workflow-weaver/smoke.sh` | 9-test validation script (offline + live) |
| `README.md` | This file — installation and quick start |

## MCP Server (23 Tools)

Works with Claude Desktop, Cursor, Kiro, or any MCP host:

```json
{
  "mcpServers": {
    "workflow-weaver": {
      "command": "npx",
      "args": ["@workflow-weaver/mcp@latest"],
      "env": {
        "WORKFLOW_WEAVER_REFRESH_TOKEN": "***",
        "WORKFLOW_WEAVER_SUPABASE_URL": "***",
        "WORKFLOW_WEAVER_SUPABASE_KEY": "***"
      }
    }
  }
}
```

Tools include: `create_project`, `add_source`, `generate_workflow`, `export_version`, `create_share_link`, `get_billing_status`, and 17 more.

## Smoke Test

Verify installation without credentials:

```bash
bash .claude/skills/run-workflow-weaver/smoke.sh
```

Verify with live credentials:

```bash
WORKFLOW_WEAVER_REFRESH_TOKEN=*** bash .claude/skills/run-workflow-weaver/smoke.sh
```

## Security

- No hardcoded credentials anywhere
- Refresh token via `WORKFLOW_WEAVER_REFRESH_TOKEN` env var only
- Supabase credentials (`SUPABASE_URL`, `SUPABASE_KEY`) via env var only
- Config file (`~/.workflow-weaver/config.json`) documented but never read directly in automation scripts
- Stripe/billing URLs presented to the user — never opened programmatically

## Tags

`workflow`, `diagram`, `documentation`, `api`, `openapi`, `process`, `mcp`, `generator`, `export`, `markdown`, `svg`, `png`, `pdf`

## License

MIT
