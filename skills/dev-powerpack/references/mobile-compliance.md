# Mobile release compliance — app-store-compliance (mjmirza/app-store-compliance)

Purpose: catch the App Store / Google Play rejection causes **before** you submit, so a release isn't lost to a week-long reject-fix-resubmit loop. A reference (125+ documented rejection patterns mapped to the exact guideline and fix) plus a pre-submission guard script that scans an iOS/Android project. It does **not** guarantee approval — a reviewer still checks what a scan can't see.

License: OpenRoots Agent License 2.3 (`LicenseRef-OpenRoots-ORA-2.3`) — written to be used by AI coding agents. Referenced here, not copied in.

## When (part of the SHIP phase, mobile apps only)

- Before any upload to App Store Connect or Google Play Console.
- After a rejection — to build the exact fix + appeal plan.
- As a release-blocker gate in the mobile ship checklist.

Applies to iOS and Android, including Flutter / React Native / Expo (managed Expo reads `app.json`/`app.config`; run `npx expo prebuild` to also cover native folders).

## Install (optional, once per machine)

```bash
git clone https://github.com/mjmirza/app-store-compliance ~/repositories/app-store-compliance
```
On Windows run in Git Bash (the guard is a bash script; needs `git` + `bash`). Also installable as its own `app-store-compliance` skill in `~/.claude/skills/` (then it triggers on "App Store rejection", "Data Safety form", "privacy nutrition labels", etc.).

## Run the guard

```bash
bash ~/repositories/app-store-compliance/agent-os/hooks/app-store-compliance-guard.sh "/path/to/your/app"
```
Point it at the folder holding the Xcode project or Android app (keep the quotes if the path has spaces). It installs nothing and changes nothing in the app. Read the verdict line:

| Verdict | Meaning | Next |
|---|---|---|
| `BLOCKED.` | A critical rejection risk was found | Fix each `[CRITICAL]` using its `fix.` line, re-run |
| `CLEAR.` | No critical risk in code (not an approval) | Do the account + listing checks in the checklist |
| `Rule pack STALE.` | Rules > 60 days old | `git pull` in the repo, re-scan |
| `NOT CHECKED.` | No iOS/Android project found there | Point it at the right folder (or `expo prebuild`) |

## The two causes to fix first (most modern rejections)

1. **A working demo account with a live backend** reviewers can actually log into.
2. **A privacy declaration that matches runtime behavior** — Apple privacy manifest / nutrition labels and Google Data safety form must cover every SDK's real data collection.

Beyond those: account-deletion requirement, IAP rules (don't steer to external payment where disallowed), permission purpose strings that match real use, broken links / placeholder content, and the 2026 regulatory layer (EU AI Act, DSA trader status, COPPA / US age-assurance laws) when the app reaches those users — see the repo's `docs/EU-REGULATORY-2026.md` and `docs/GLOBAL-REGULATORY-2026.md`.

## How dev-powerpack uses it

1. During build, treat the two top causes as acceptance criteria for any store-bound mobile feature (login/demo access, privacy-accurate SDK use).
2. In the ship checklist, run the guard and clear every `[CRITICAL]` before building the release artifact.
3. After a rejection, map the rejection email to the pattern in `docs/MISTAKE-PATTERNS.md` and follow its fix + appeal.
4. The guard's scan is a signal, not a guarantee — still do the manual account/listing checks and record readiness.

If the repo isn't installed, fall back to the "two causes" above plus the store guidelines directly (developer.apple.com/app-store/review/guidelines, play.google/developer-content-policy).
