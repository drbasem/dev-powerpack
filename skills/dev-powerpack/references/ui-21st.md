# 21st MCP — UI components (21st-dev/magic-mcp, now "21st MCP")

Purpose: start UI work from proven, polished components (React/Tailwind/shadcn-style) instead of from a blank file, then adapt them to the project.

## When

- Building a component, section or page: hero, pricing table, navbar, sidebar, dashboard card, form, modal, data table, testimonial, footer, auth screen…
- Need a brand logo (SVG) for a known company/product.
- Need design inspiration / variants before choosing a direction.

Not useful for non-web stacks (Flutter, native Android/iOS) except as visual inspiration — then translate the layout by hand.

## Tools (current names; legacy Magic names still accepted)

| Tool | Use |
|---|---|
| `search` | Search the 21st catalog (components, themes, templates) |
| `get_component` | Get the code of a chosen component |
| `get_inspiration` | Browse examples for a UI idea (was `21st_magic_component_inspiration`) |
| `search_logo` | One brand logo per call (was `logo_search`) |
| `generate` / `iterate_generation` | Hosted AI UI generation — only listed when AI access is enabled on the account; check `get_usage.aiGenerationEnabled`. If a call returns `ai_subscription_required`, don't retry — use `search` + `get_component` and adapt the code yourself. |

Run `tools/list` (or check the available MCP tools) to see what this account has.

## Workflow

1. Read the project's UI stack first: framework, styling (Tailwind version, CSS modules…), component library, design tokens, dark mode approach.
2. `search` with a concrete description ("pricing table with monthly/yearly toggle, 3 tiers"). Pick 1–3 candidates; show the user if the choice is a matter of taste.
3. `get_component` → adapt: match the project's tokens, fonts, spacing, icon set, file structure and naming; strip demo data; wire real props/state.
4. Verify dependencies it needs against current docs (Context7) before installing them.
5. Add loading / empty / error states, accessibility (labels, roles, focus, contrast), responsive behavior.
6. Check it renders in a real browser (no console errors, works at phone width).

## Rules

- Never paste a component in unadapted; it must look like it belongs to the project.
- Don't pull in a new UI library just for one component if the project already has one.
- Respect component licenses; paid components require the user's account access.
