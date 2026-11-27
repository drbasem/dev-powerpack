# UI/UX design intelligence — ui-ux-pro-max + sumi

Purpose: make interfaces actually look good and feel consistent, not just functional. These two packs supply **design decisions** — type scale, palette, spacing rhythm, motion, accessibility — where [ui-21st.md](ui-21st.md) supplies ready-made **components**. Use them together: 21st to start from a real component, these to tune it so it doesn't read as a generic AI default.

Both are their own multi-skill plugins (referenced, not copied into dev-powerpack). Either is optional.

## When

- Designing or refactoring any screen, component, page, or design system.
- Choosing color, typography, spacing, layout, icons, charts, or motion.
- Reviewing UI for quality: accessibility, consistency, "does this look generic/untuned?"
- Branding (logo usage, palette, voice) — ui-ux-pro-max has a `brand` skill.

Skip for pure backend/API/infra work, unless it changes how something looks, feels, moves, or is interacted with.

## The two packs

### ui-ux-pro-max (nextlevelbuilder/ui-ux-pro-max-skill, MIT)

Searchable **local** design data queried with its `scripts/search.py`: ~80 styles, ~194 product palettes with reasoning, 74 font pairings, 141 UX guidelines, 105 icons, GSAP motion presets, chart types, and 23 tech stacks. Ships sub-skills: `ui-ux-pro-max` (the intelligence), `design`, `design-system`, `ui-styling`, `brand`, `banner-design`, `slides`.

Its rule priority (follow 1→10): **Accessibility → Touch & Interaction → Performance → Style Selection → Layout/Responsive → Typography/Color → Animation → Forms/Feedback → Navigation → Charts**. Hard rules worth remembering: contrast ≥ 4.5:1, touch targets ≥ 44×44px with ≥ 8px spacing, base font 16px / line-height 1.5, semantic color tokens (no raw hex in components), one motion curve, SVG icons not emoji, mobile-first, respect reduced-motion.

### sumi (phazurlabs/sumi, Apache-2.0)

A "design crew" plugin (43 skills, 37 commands, an anti-slop engine, and a citation trail). Its premise: **a default is a decision nobody made** — it hunts the "tells" of an untuned AI build and fixes them: five font weights → two with a job each; flat type → a real display-to-body ratio; pure #000 on #FFF → a sampled paper + ink; four easing curves → one house curve; three competing CTAs → one action. Start with `/sumi:start`; it keeps an auditable trail of why each decision was made.

## Install (optional)

```bash
# ui-ux-pro-max (plugin)
/plugin marketplace add nextlevelbuilder/ui-ux-pro-max-skill
# then install the plugin it lists

# sumi (plugin)
/plugin marketplace add phazurlabs/sumi
/plugin install sumi@sumi-marketplace
# then: /sumi:start
```
Either can also be used by copying its `.claude/skills/<name>/` folders into `~/.claude/skills/`. On Windows, run git/bash parts in Git Bash.

## How dev-powerpack uses them

1. In the **Build** phase, after pulling a component from 21st (or before writing UI by hand), apply the design intelligence: pick a style/palette/type pairing deliberately and state the choice, don't accept defaults.
2. Enforce the hard rules as acceptance criteria: contrast, touch targets, 16px base, semantic tokens, one motion curve, reduced-motion, mobile-first, loading/empty/error states.
3. In **Review**, run a UI quality pass against the "tells" above plus the accessibility checklist before approving.
4. Keep it coherent: two font weights, one accent, one curve — consistency over cleverness (matches the skill's core "simplest thing that works" rule).

If neither is installed, fall back to the baseline rules listed above and the frontend notes in [lifecycle.md](lifecycle.md) ("Also covered").
