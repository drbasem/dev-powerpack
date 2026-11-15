# Red-team playbooks — claude-red (SnailSploit/claude-red, MIT)

**Authorization first — same rule as Strix.** These are offensive-security playbooks. Use them ONLY for work the user is clearly authorized to do: an authorized penetration test or red-team engagement, a bug-bounty program whose scope allows it, a CTF, security research on the user's own systems, or training. If scope or ownership is unclear, ask before proceeding. Never target third-party systems, and never use these to evade detection or cause damage on systems the user doesn't own.

## What it is

`claude-red` is a library of ~78 `SKILL.md` playbooks (23 categories) that each prime Claude as a specialist for one attack surface — the *methodology, tooling, edge cases and escalation paths*, loaded on demand by conversational triggers. It complements Strix: Strix **runs** an autonomous scan and proves findings with PoCs; claude-red gives you the **expert method** to investigate a specific surface by hand, understand a finding, or go deeper than an automated scan.

Categories: web app (OWASP + advanced classes), auth/identity (JWT, OAuth), Active Directory, wireless, cloud (AWS/Azure/GCP), mobile, IoT/embedded, infrastructure & red team (initial access, EDR evasion, Windows internals), exploit dev, fuzzing & vuln research, recon/OSINT, API, container & Kubernetes, CI/CD, crypto/TLS, privilege escalation, post-exploitation, forensics & C2, supply chain, social engineering, network (L2/L3, MITM), AI security (prompt injection, RAG poisoning), and utility (triage checklist, reporting).

## When to use (within an authorized engagement)

- A Strix finding needs manual confirmation or deeper exploitation → load the matching surface skill (e.g. `offensive-sqli`, `offensive-ssrf`, `offensive-jwt`).
- Testing a surface Strix doesn't cover well (Active Directory, wireless, cloud IAM paths, mobile, exploit dev, fuzzing).
- Structuring recon, choosing tooling, or writing the final report (`utility` skills).
- Defensive work: understanding an attack to harden against it — pair with `security-strix.md`'s checklist.

## Install (optional)

It auto-loads its own skills by trigger once cloned into the skills folder:

```bash
git clone https://github.com/SnailSploit/claude-red ~/.claude/skills/claude-red
```
Single category (sparse):
```bash
git clone --filter=blob:none --sparse https://github.com/SnailSploit/claude-red
cd claude-red && git sparse-checkout set Skills/web Skills/active-directory
```
On Windows, run these in Git Bash. Keep it as its own `claude-red` skill folder — don't copy its files into dev-powerpack.

## How dev-powerpack uses it

1. Confirm authorization and scope in writing before any offensive step.
2. Prefer a Strix scan first for breadth (`security-strix.md`); then load the specific claude-red surface skill for depth on what you found.
3. Reproduce only against the authorized target; capture evidence/PoC.
4. Feed results back into the fix loop (fix root cause → re-test) and the report.

If `claude-red` isn't installed, fall back to the hardening checklist in `security-strix.md` and the general five-axis security review.
