---
name: dev-powerpack
description: Hybrid software-engineering workflow for any coding project. Combines a senior-engineer lifecycle (spec, plan, incremental build, TDD, five-axis review, ship — from addyosmani/agent-skills) with power tools — Context7 for up-to-date library docs, Graphify for a knowledge graph of the codebase, 21st.dev MCP for ready-made UI components, Strix for AI penetration testing, claude-red offensive-security playbooks for authorized red-team/CTF work, and app-store-compliance for catching App Store / Google Play rejections before submitting a mobile app. Use whenever building a feature, starting or planning a project, fixing a bug, writing code against any library or framework, exploring an unfamiliar codebase, building UI/front-end components, reviewing code, hardening security, pentesting or red-teaming your own app, submitting or releasing an iOS/Android app, or preparing to ship. Also trigger on "/spec", "/plan", "/build", "/test", "/review", "/ship", "/pentest", "/redteam", "/appstore", "/compliance", "use context7", "graphify", "21st", "strix", "pentest", "red team", "security scan", "App Store rejection", "Google Play rejection", "mobile app submission".
---

# Dev Powerpack

One skill that runs a disciplined engineering lifecycle and calls the right tool at the right moment.

```
 DEFINE ──▶ PLAN ──▶ BUILD ──▶ VERIFY ──▶ REVIEW ──▶ SECURE ──▶ SHIP
  spec      tasks    slices     tests     5 axes    Strix      checklist
     ▲         ▲        ▲                    ▲
  Graphify  Graphify  Context7 + 21st     Graphify (impact)
```

## Step 0 — Orient (every session)

1. Read `HANDOFF.md` (if the project has one) and the project's CLAUDE.md / README.
2. Detect the stack and exact versions from the dependency files (`package.json`, `pubspec.yaml`, `pyproject.toml`, `build.gradle`, `go.mod`, `Cargo.toml` …). State them.
3. Check which tools are available (each one is optional — skip silently if missing, never block on it):
   - Context7: MCP tools `resolve-library-id` / `query-docs`, or `ctx7` CLI
   - Graphify: `graphify` CLI, or `graphify-out/graph.json` already in the repo
   - 21st: MCP tools `search` / `get_component` / `generate`
   - Strix: `strix` CLI + Docker running
   - claude-red: `claude-red` skill folder present (offensive-security playbooks, loads by trigger)
   - app-store-compliance (mobile apps only): guard script at `~/repositories/app-store-compliance`, or the `app-store-compliance` skill
   Setup for each: [references/setup.md](references/setup.md).
4. If the codebase is unfamiliar or large (> ~30 files) and Graphify is available, build or update the graph first — see [references/graphify.md](references/graphify.md).

## Router — what kind of work is this?

| Situation | Do this | Reference |
|---|---|---|
| Vague idea, unclear requirements | Interview one question at a time, then write a spec | [lifecycle.md § Define](references/lifecycle.md#1-define) |
| New feature / multi-file change | Spec → plan → incremental build | [lifecycle.md](references/lifecycle.md) |
| "How does X work / where is Y?" in an existing repo | Query the graph before grepping | [graphify.md](references/graphify.md) |
| Writing code that uses any library/framework/SDK | Fetch current docs first, cite them | [context7.md](references/context7.md) |
| Building UI (component, page, landing, dashboard) | Search 21st for a component, adapt it to the project | [ui-21st.md](references/ui-21st.md) |
| Bug / failing test | Reproduce with a test → root cause → fix → regression test | [lifecycle.md § Debug](references/lifecycle.md#debug) |
| Review a diff / PR | Five-axis review + graph impact check | [lifecycle.md § Review](references/lifecycle.md#6-review-five-axes) |
| Auth, input handling, payments, APIs, before release | Security checklist, then Strix scan | [security-strix.md](references/security-strix.md) |
| Authorized pentest / red-team / CTF — deep manual work on one attack surface | Confirm scope, then load the matching claude-red playbook | [redteam-claude-red.md](references/redteam-claude-red.md) |
| Submitting a mobile app to the App Store / Google Play (or after a rejection) | Run the pre-submission guard, clear every critical before release | [mobile-compliance.md](references/mobile-compliance.md) |
| Ready to release | Ship checklist (mobile: + compliance guard) | [lifecycle.md § Ship](references/lifecycle.md#8-ship) |

Slash-style shortcuts the user may type: `/spec`, `/plan`, `/build`, `/test`, `/review`, `/secure`, `/ship`, `/docs <lib>`, `/graph`, `/ui <what>`, `/pentest` or `/redteam <surface>`, `/appstore` or `/compliance` (mobile) — map each to the matching row above.

## Non-negotiable behaviors (apply in every phase)

1. **Surface assumptions** before non-trivial work: list them, invite correction, then proceed.
2. **Stop on confusion.** If the spec, code and user disagree, name the conflict and ask — don't guess silently.
3. **No API from memory.** Any framework-specific call, config or version-sensitive pattern → check Context7 (or the official docs) first. If neither is reachable, say the code is unverified.
4. **Thin slices.** Never write more than ~100 lines without building/testing. Each slice leaves the app working.
5. **Tests are proof.** "Seems right" is not done. Bug fix = failing test first.
6. **Simplest thing that works.** No abstractions before the third use case; no speculative features.
7. **Touch only what the task needs.** No drive-by refactors; mention them instead.
8. **Report honestly.** Say what was verified, what wasn't, and what failed — with the output.
9. **Offensive work only on targets the user owns or is authorized to test.** Never point Strix or any claude-red playbook at third-party systems; confirm scope first.
10. **Secrets stay out of code and chat.** API keys (Context7, 21st, LLM keys for Strix) live in environment variables.

## Definition of done (any change)

- [ ] Matches the spec / task acceptance criteria
- [ ] Builds; focused tests and the full suite pass (project's own commands, not assumed ones)
- [ ] Library usage verified against current docs (Context7 cite or official link)
- [ ] Self-reviewed on the five axes; no dead code, no debug prints, no secrets
- [ ] Security-relevant change? Checklist done; Strix run if it touches auth/input/APIs
- [ ] Graph updated (`graphify update .`) if the project uses Graphify
- [ ] `HANDOFF.md` updated with what changed and what's next

## Sources and credits

Condensed and combined from: addyosmani/agent-skills (MIT), upstash/context7 (MIT), Graphify-Labs/graphify (Apache-2.0), 21st-dev/magic-mcp (ISC), usestrix/strix (Apache-2.0), SnailSploit/claude-red (MIT), mjmirza/app-store-compliance (OpenRoots ORA 2.3). See [references/NOTICE.md](references/NOTICE.md).
