# dev-powerpack

**One skill that makes your AI coding agent work like a senior engineer — and gives it a set of power tools.**

[العربية](README.ar.md)

Five excellent open-source projects each solve one piece of the AI-coding puzzle. Installed separately, nobody tells the agent *when* to use which. `dev-powerpack` combines them into a single [Agent Skill](https://agentskills.io) with one lifecycle and a router that calls the right tool at the right moment.

```
 DEFINE ──▶ PLAN ──▶ BUILD ──▶ VERIFY ──▶ REVIEW ──▶ SECURE ──▶ SHIP
  spec      tasks    slices     tests     5 axes    Strix      checklist
     ▲         ▲        ▲                    ▲
  Graphify  Graphify  Context7 + 21st     Graphify (impact)
```

## The seven ingredients

| Project | Problem it solves | Role in dev-powerpack |
|---|---|---|
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | Agents skip specs, write huge untested diffs, review shallowly | The backbone: spec → plan → thin slices → TDD → five-axis review → ship, plus non-negotiable behaviors (surface assumptions, stop on confusion, tests are proof) |
| [upstash/context7](https://github.com/upstash/context7) | Models write code from stale training data and hallucinate APIs | **Before** any library/framework code, fetch current, version-specific docs and cite them |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Agents burn context grepping file after file | Map the repo into a local knowledge graph; query paths, dependencies and blast radius instead of reading everything |
| [21st-dev/magic-mcp](https://github.com/21st-dev/magic-mcp) (21st MCP) | AI-built UIs look generic | Start UI work from a curated component, then adapt it to the project's design tokens |
| [usestrix/strix](https://github.com/usestrix/strix) | Static scanners flood you with false positives | Autonomous pentest of *your own* app with working proofs-of-concept; fix → re-scan loop; CI gate |
| [SnailSploit/claude-red](https://github.com/SnailSploit/claude-red) | Automated scans miss depth on specific attack surfaces | ~78 offensive-security playbooks (web, AD, cloud, wireless, exploit dev…) for *authorized* red-team/CTF work — expert method for manual, deep testing |
| [mjmirza/app-store-compliance](https://github.com/mjmirza/app-store-compliance) | Mobile apps get rejected on the same preventable causes, costing a week each | Pre-submission guard for iOS/Android: 125+ rejection patterns mapped to the exact guideline + fix; catches them before App Store / Play review does |

Every tool is **optional**. If one is missing, the skill falls back (official docs, Grep, hand-written UI, manual security checklist) and never blocks.

## What the agent does differently

- States the detected stack and versions, and its assumptions, before coding.
- Writes a short spec for non-trivial work, then small verifiable tasks.
- Looks up current docs before using any API — no "from memory" code.
- Queries the code graph before planning or debugging in a large repo.
- Builds UI from a real component, not a blank file.
- Writes the failing test first; never weakens a test to make it pass.
- Reviews on five axes: correctness, readability, architecture, security, performance.
- Runs a security checklist and (on owned targets only) a Strix scan before shipping.
- For a mobile release, runs the App Store / Play pre-submission guard and clears every critical first.
- Ends with a definition-of-done checklist and an updated `HANDOFF.md`.

Shortcuts you can type: `/spec` `/plan` `/build` `/test` `/review` `/secure` `/ship` `/docs <lib>` `/graph` `/ui <what>` `/pentest` `/redteam <surface>` `/appstore`.

## Install the skill

**Claude Code (plugin):**
```
/plugin marketplace add drbasem/dev-powerpack
/plugin install dev-powerpack@dev-powerpack
```

**Any SKILL.md-compatible agent** (Claude Code, Cursor, Codex, Gemini CLI, Copilot …):
```bash
npx skills add drbasem/dev-powerpack
```

**Manual:** copy `skills/dev-powerpack/` into `~/.claude/skills/` (or your agent's skills folder).

## Install the tools (optional, recommended)

Full steps, including Windows notes: [`skills/dev-powerpack/references/setup.md`](skills/dev-powerpack/references/setup.md).

| Tool | Quick install | Key |
|---|---|---|
| Context7 | `npx ctx7 setup --claude` or HTTP MCP `https://mcp.context7.com/mcp` | optional (higher rate limits) — context7.com/dashboard |
| Graphify | `uv tool install graphifyy` then `graphify .` in a project | none (code parsing is fully local) |
| 21st MCP | `npx @21st-dev/cli@latest init --client claude` | required — 21st.dev/mcp |
| Strix | `uv tool install strix-agent` (+ Docker running) | LLM API key |
| claude-red | `git clone https://github.com/SnailSploit/claude-red ~/.claude/skills/claude-red` | none |
| app-store-compliance | `git clone https://github.com/mjmirza/app-store-compliance ~/repositories/app-store-compliance` | none |

Keep all keys in environment variables — never in code or chat.

## Repository layout

```
.claude-plugin/          plugin + marketplace manifests
skills/dev-powerpack/
  SKILL.md               router, core behaviors, definition of done
  references/
    lifecycle.md         spec → plan → build → test → review → document → ship
    context7.md          docs lookup workflow
    graphify.md          code-graph workflow
    ui-21st.md           UI component workflow
    security-strix.md    hardening checklist + Strix pentest/fix loop
    redteam-claude-red.md  authorized red-team playbooks (claude-red)
    mobile-compliance.md   App Store / Play pre-submission guard
    setup.md             installing the tools
    NOTICE.md            sources and licenses
```

## Responsible use

Strix and the claude-red playbooks perform real attacks. Only use them on code, apps and systems you own or are explicitly authorized to test — within an authorized engagement, bug-bounty scope, CTF, or your own research. Confirm scope before any offensive step.

## Credits

This project is a rewritten synthesis, not a fork. All credit for the underlying ideas and tools goes to their authors: Addy Osmani and contributors (agent-skills, MIT), Upstash (Context7, MIT), Graphify Labs (Apache-2.0), 21st.dev (ISC), the Strix team (Apache-2.0), SnailSploit (claude-red, MIT), and mjmirza (app-store-compliance, OpenRoots ORA 2.3). Each tool is installed from its own source and governed by its own license and terms.

## Author

**Dr. Basem M. ElHalawany** — [github.com/drbasem](https://github.com/drbasem). Issues and pull requests welcome.

License: [MIT](LICENSE)
