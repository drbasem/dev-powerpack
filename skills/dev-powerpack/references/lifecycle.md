# Engineering lifecycle (condensed from addyosmani/agent-skills)

Gate each phase: the human reviews the output before the next phase starts — except for small, unambiguous changes (one-line fixes, typos), which skip straight to Build.

## 1. Define

**Interview (when the idea is vague).** Ask one question at a time, most decision-relevant first: who is the user, what problem, what does success look like, what's out of scope, constraints (platform, budget, deadline, offline?). Stop when you can write acceptance criteria.

**Scope check.** If one request bundles several independently testable capabilities (e.g. auth + billing + notifications), first propose a capability map — a small table of module ids, responsibilities and dependencies (no cycles) plus a build order — and get it approved.

**Spec** (save as `docs/spec.md` or `SPEC.md` in the project):

```markdown
# Spec: <feature>
## Objective        — why, for whom
## Scope            — in / out
## Requirements     — numbered, testable
## Acceptance criteria — Given/When/Then or checklist
## Constraints      — stack, versions, performance, security, platforms
## Open questions
```

Before writing it on an existing codebase: `graphify query "<feature area>"` to see what already exists.

## 2. Constraints (quality bar, decide once)

Write down the project's non-negotiables once (in CLAUDE.md or `docs/constraints.md`): lint/format rules, test coverage expectation, supported platforms/versions, performance budgets, accessibility level, security rules. Enforce them in every review instead of re-deciding.

## 3. Plan

Break the spec into small, atomic tasks (each ≈ one commit, ≤ ~1 hour, independently verifiable):

```markdown
- [ ] T1 <task> — files: … — done when: … — verify: <command>
```

- Order by dependency, then risk-first (prove the riskiest unknown early).
- Prefer **vertical slices** (DB + API + UI for one capability) over horizontal layers.
- Contract-first when front-end and back-end proceed in parallel.
- Use the graph to list files each task touches: `graphify explain "<Symbol>"`.

## 4. Build (incremental)

Loop per task: **implement smallest piece → test → verify → commit → next.**

- Before coding framework-specific parts: Context7 lookup (see context7.md).
- Before writing UI from scratch: 21st search (see ui-21st.md).
- Simplicity first; follow existing patterns in the codebase.
- Keep the app runnable after every slice. Risky changes go behind a feature flag.
- Atomic commits with descriptive messages; never mix refactor + behavior change in one commit.

## 5. Test (TDD)

1. Discover the project's own test commands (README, CI config, Makefile, `./gradlew`, `flutter test`, `pytest` …). Never assume `npm test`.
2. **RED** — write a test that fails for the right reason.
3. **GREEN** — minimum code to pass.
4. **REFACTOR** — clean up with tests green.
5. Run the focused test during the loop, the full suite before calling it done.

Test pyramid: many unit, some integration, few end-to-end. Test behavior, not implementation details. For web UI, also verify in a real browser (console errors, network failures, screenshot).

### Debug

Reproduce → write a failing test that captures the bug → form hypotheses → bisect / add instrumentation → find the root cause (not the symptom) → fix → keep the test as a regression guard. Use `graphify path "<A>" "<B>"` to see how the failing piece connects to the rest. Never "fix" by deleting or weakening a test.

## 6. Review (five axes)

Approve when the change clearly improves code health, even if imperfect.

1. **Correctness** — matches spec; edge cases (null/empty/boundaries); error paths; race conditions; tests actually test the claim.
2. **Readability & simplicity** — clear names; flat control flow; could it be fewer lines; abstractions earn their keep; no dead code or leftover debug.
3. **Architecture** — follows existing patterns; clean boundaries; no circular deps; reduces complexity rather than relocating it. Check blast radius with `graphify query`/`explain` on changed symbols.
4. **Security** — input validated at boundaries; authz on every protected path; no secrets in code/logs; parameterized queries; output encoding. See security-strix.md.
5. **Performance** — no N+1 queries, unbounded loops/lists, unnecessary re-renders, blocking I/O on the UI thread; measure before optimizing.

Label findings: **Must fix** / **Should fix** / **Nit**. Also run a simplification pass: remove what isn't needed.

## 7. Document

- ADR (`docs/adr/NNNN-title.md`) for any decision that's expensive to reverse: context, decision, alternatives, consequences.
- Comments explain *why*, not *what*. Update README run/build instructions when they change.
- Update `HANDOFF.md` at the end of each piece of work.

## 8. Ship

Pre-launch checklist:

- [ ] Full test suite + lint + type-check green; release build succeeds
- [ ] Security: checklist done, Strix scan clean (or findings triaged), dependencies audited (`npm audit`, `pip-audit`, `flutter pub outdated` …)
- [ ] Config/secrets via environment, not committed; debug flags off
- [ ] Observability: errors logged with context; crash reporting / health check in place where relevant
- [ ] Version number and changelog updated
- [ ] Rollback plan known (previous build kept, feature flag, or revert commit)
- [ ] Staged rollout when possible; watch errors after release

## Also covered (apply when relevant)

- **API & interface design:** contract first, consistent naming, versioning, explicit errors, pagination, idempotency for writes.
- **Frontend:** component composition, accessible (labels, contrast, keyboard), responsive, loading/empty/error states for every data view.
- **Git:** short-lived branches, atomic commits, never force-push shared branches.
- **CI/CD:** lint + test + build on every PR; Strix CI scan for security-sensitive repos.
- **Deprecation/migration:** add new path → migrate callers → remove old path; never break callers silently.
- **Context engineering:** load only the files a task needs; prefer graph queries and summaries over dumping whole directories.
