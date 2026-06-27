# run-workflow-weaver

A [Clawhub](https://clawhub.ai) skill for agentically driving the **workflow-weaver** CLI and MCP server.

## What is this?

This skill lets AI agents (OpenClaw, Claude, Cursor, Kiro, etc.) create projects, add sources, generate workflows, export versions, and manage billing for [workflow-weaver](https://www.npmjs.com/package/workflow-weaver) — headlessly, via both CLI subprocess and MCP server interfaces.

## Installation

### As a Clawhub skill

```bash
clawhub install run-workflow-weaver
```

Or manually copy `.claude/skills/run-workflow-weaver/` into your agent's skills directory.

### Prerequisites

- Node.js ≥ 20
- Global CLI + MCP server:
  ```bash
  npm install -g workflow-weaver @workflow-weaver/mcp
  ```

## Files

| File | Purpose |
|------|---------|
| `.claude/skills/run-workflow-weaver/SKILL.md` | Agent-facing instructions, tool reference, and workflow patterns |
| `.claude/skills/run-workflow-weaver/smoke.sh` | Smoke test script for validating CLI + MCP installation |

## Smoke Test

Verify offline (no token required):

```bash
bash .claude/skills/run-workflow-weaver/smoke.sh
```

Verify with a live token:

```bash
WORKFLOW_WEAVER_REFRESH_TOKEN=<token> bash .claude/skills/run-workflow-weaver/smoke.sh
```

## Security

- No hardcoded credentials anywhere
- Refresh token via `WORKFLOW_WEAVER_REFRESH_TOKEN` env var only
- Config file (`~/.workflow-weaver/config.json`) is documented but never read directly in scripts
- Stripe/billing URLs are presented to the user — never opened programmatically
- Smoke test uses `smoke-test-invalid-token` placeholder for protocol validation

## License

MIT
