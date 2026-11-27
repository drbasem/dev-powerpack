# One-time setup of the tools (Windows-friendly)

All are optional; the skill works without any of them (it falls back to official docs, Grep, hand-written UI, and the manual security checklist). Keys go in **user environment variables**, never in code or chat.

## Context7 (docs) — free key at context7.com/dashboard

Option A — guided (Node ≥ 22.13):
```bash
npx ctx7 setup --claude
```
Option B — manual MCP for Claude Code:
```bash
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "Authorization: Bearer $CONTEXT7_API_KEY" --scope user
```

## Graphify (codebase graph) — Python 3.10+

```bash
uv tool install graphifyy      # or: pipx install graphifyy   (note the double y)
graphify install               # registers its own /graphify command with Claude Code
```
Then in any project: `graphify .`

## 21st MCP (UI components) — key at 21st.dev/mcp

```bash
npx @21st-dev/cli@latest init --client claude
```
or manual HTTP MCP:
```bash
claude mcp add --transport http 21st https://21st.dev/api/mcp --header "x-api-key: $API_KEY_21ST" --scope user
```
(Old Magic MCP keys were reset — generate a new one.)

## Strix (pentesting) — Docker Desktop + an LLM key

```bash
pipx install strix-agent       # or: uv tool install strix-agent ; on WSL/Linux: curl -sSL https://strix.ai/install | bash
```
Set `STRIX_LLM` (e.g. `anthropic/claude-sonnet-5-5`) and `LLM_API_KEY` as user env vars. First run pulls the sandbox Docker image.

## claude-red (offensive-security playbooks) — no key, authorized use only

```bash
git clone https://github.com/SnailSploit/claude-red ~/.claude/skills/claude-red
```
Installs as its own `claude-red` skill folder; its ~78 playbooks load on demand by trigger. Windows: run in Git Bash. Only use on targets you own or are authorized to test (see references/redteam-claude-red.md).

## app-store-compliance (mobile release guard) — no key, mobile apps only

```bash
git clone https://github.com/mjmirza/app-store-compliance ~/repositories/app-store-compliance
```
Run the guard before submitting: `bash ~/repositories/app-store-compliance/agent-os/hooks/app-store-compliance-guard.sh "/path/to/app"`. Windows: Git Bash. It installs nothing and changes nothing in your app. `git pull` the repo occasionally so its rule pack stays fresh. See references/mobile-compliance.md.

## Setting env vars on Windows (PowerShell, persists for the user)

```powershell
[Environment]::SetEnvironmentVariable("CONTEXT7_API_KEY", "<paste>", "User")
[Environment]::SetEnvironmentVariable("API_KEY_21ST", "<paste>", "User")
[Environment]::SetEnvironmentVariable("LLM_API_KEY", "<paste>", "User")
[Environment]::SetEnvironmentVariable("STRIX_LLM", "anthropic/claude-sonnet-5-5", "User")
```
Restart Claude Code afterwards.

## Check what's installed

```bash
claude mcp list
graphify --version
strix --version
docker info
```
