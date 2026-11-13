# Security — hardening checklist + Strix AI pentesting (usestrix/strix)

**Authorization first:** only scan code, apps, domains or IPs the user owns or has written permission to test. If ownership is unclear, ask. Never scan third-party sites.

## A. Hardening checklist (during build and review)

- Input validated and normalized at every trust boundary (forms, APIs, deep links, file uploads, intents).
- AuthN on every protected route; **AuthZ checked per object** (no IDOR/BOLA: user A can't read B's record by changing an ID).
- Parameterized queries / ORM only — no string-built SQL, shell or NoSQL queries.
- Output encoding / no `dangerouslySetInnerHTML` / no WebView JS bridges to untrusted content (XSS).
- CSRF protection for cookie-based sessions; secure, HttpOnly, SameSite cookies.
- Secrets in environment / secret store, never in repo, APK, client bundle, logs or error messages.
- Passwords hashed with bcrypt/argon2; tokens expire; rate-limit login and expensive endpoints.
- HTTPS only; certificate validation never disabled; CORS restricted.
- Dependencies audited (`npm audit`, `pip-audit`, `osv-scanner`); remove unused ones.
- Mobile: no sensitive data in plain SharedPreferences/logs; `android:exported` reviewed; no debug builds released.
- Errors fail closed; no stack traces to users.

## B. Strix — automated pentest that proves findings with PoCs

Needs Docker running + an LLM key. Scans take minutes (`quick`) to hours (`deep`) → run in the background.

```bash
# install (Linux/macOS/WSL); or: pipx install strix-agent  /  uv tool install strix-agent
curl -sSL https://strix.ai/install | bash

export STRIX_LLM="anthropic/claude-sonnet-5-5"   # any LiteLLM model id
export LLM_API_KEY="<key from env, never typed into chat>"

strix -n -t ./ --scan-mode quick --max-budget 10           # white-box: local repo
strix -n -t http://localhost:3000 --scan-mode quick        # black-box: your running dev app
strix -n -t https://staging.myapp.com --scan-mode deep     # your own staging site
```

Always use `-n` (headless/non-interactive) from an agent. Set `--max-budget` (USD of LLM spend) — confirm it with the user.

**Exit codes:** `0` clean · `1` fatal error · `2` vulnerabilities found. A `0` only covers what was analyzed — open `run.json` and check `status` and `llm_usage.cost` vs the budget before calling it clean (a run that hit the budget is incomplete).

**Results** in `strix_runs/<run-name>/`: `penetration_test_report.md`, `vulnerabilities/*.md`, `vulnerabilities.json`, `findings.sarif`, `run.json`.

### Fix loop

1. Read `vulnerabilities.json`; sort by severity (critical → low).
2. For each: reproduce from the PoC (against local/dev only), write a failing test where possible, fix the root cause, keep the test.
3. Re-run Strix on the same target to confirm the finding is gone.
4. Report: fixed / accepted-risk (with reason) / false positive (with evidence).

### CI

Add a PR job that runs `strix -n -t ./ --scan-mode quick` and fails on exit code 2; upload `findings.sarif` to GitHub code scanning.

### Managed cloud (no Docker / no LLM key)

`strix cloud login …`, `strix cloud scans start --source . --dry-run --show-files --json`, then approve the reviewed `source.archive_sha256`. Cloud scans consume paid credits and upload source code — get the user's explicit OK before any cloud scan, top-up or source upload.

## Windows note

Strix's installer is a bash script and needs Docker Desktop. On Windows use WSL2 or `pipx install strix-agent` with Docker Desktop running.
