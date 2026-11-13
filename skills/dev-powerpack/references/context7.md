# Context7 — up-to-date library docs (upstash/context7)

Purpose: stop writing code from stale training data. Fetch version-specific docs and examples before using any library API.

## When

- Any framework/SDK/library call whose signature, config or recommended pattern could have changed (Next.js, React, Flutter packages, Supabase, Firebase, Tailwind, Cloudflare Workers, Prisma, LangChain …).
- Setup / configuration / migration steps.
- A compile or runtime error that suggests an API mismatch.

Skip for pure language logic (loops, data structures) and trivial edits.

## How — MCP mode

1. `resolve-library-id` with `libraryName` (e.g. "next.js") and `query` (the task) → pick the best ID (e.g. `/vercel/next.js`). Prefer the official repo, highest trust/snippet count, and the version matching the project.
2. `query-docs` with `libraryId`, a specific `query` ("middleware that redirects unauthenticated users", not "next.js"), and optional `language`.
3. If the user already gave an ID like `/supabase/supabase`, skip step 1.
4. Mention the version in the query when the project pins one ("Next.js 16 proxy").

## How — CLI mode (no MCP)

```bash
npx ctx7 library next.js "jwt cookie middleware"     # find the library ID
npx ctx7 docs /vercel/next.js "jwt cookie middleware" # fetch docs
```

## Rules

- Use the docs' pattern over memory when they disagree; tell the user when you changed course because of the docs.
- Cite: "Per Context7 `/vercel/next.js` (v16): …" or link the official page.
- If Context7 is unavailable or has no match: fall back to the official docs site (fetch the specific page, not the homepage). Order of authority: official docs → official changelog/blog → web standards (MDN) → compatibility tables. Stack Overflow / blogs are never primary sources.
- If nothing authoritative is reachable, label the code **unverified**.
- Keep queries narrow; don't pull whole manuals into context.
