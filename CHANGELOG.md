# Changelog

## 0.1.5 — 2026-11-21

### Fixed
- smoke.sh test 6 passes its invalid token only to the MCP process, so a real `WORKFLOW_WEAVER_REFRESH_TOKEN` is no longer overwritten and unset, and the live auth and billing checks (tests 7-8) run instead of skipping
- Removed the empty `autoApprove` key from the MCP config example

## 0.1.4 — 2026-11-21

### Security
- Description and docs now state that smoke.sh, with a token set, also reads the account's billing status (read-only)

## 0.1.3 — 2026-11-21

### Security
- Description now says what the skill does: drives the hosted Workflow Weaver service, with smoke.sh as an install/auth check only; trigger narrowed to explicit Workflow Weaver requests
- Removed the command that printed the refresh token to stdout
- Pinned `workflow-weaver@0.1.0` and `@workflow-weaver/mcp@0.1.0`; MCP examples use the pinned binary instead of `npx ...@latest`
- MCP examples use user-level config files, Cursor uses `${env:...}` references, and plaintext-config risks are documented
- Agents must confirm destructive, quota-using and share-link actions with the user; share links are documented as public

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
