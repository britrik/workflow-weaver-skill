# Changelog

## 0.1.0 — 2026-06-27

### Added
- Initial skill for workflow-weaver CLI and MCP server
- SKILL.md: agent-facing instructions, 23 MCP tools reference, security rules, 6-step workflow
- smoke.sh: 9-test validation (offline + live credential checks)
- README.md: installation guide with use-case focused content

### Security
- No hardcoded credentials
- Refresh token via `WORKFLOW_WEAVER_REFRESH_TOKEN` env var only
- Supabase credentials via env var only
- Config file documented but never read directly in automation scripts
- Stripe URLs presented to user only

### Testing
- 6 offline tests: version, help, auth error, MCP presence, MCP no-token, MCP handshake
- 2 live tests: auth status, billing status (require credentials)
- Test 0: jq availability check
