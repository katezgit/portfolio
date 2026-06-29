# Portfolio — Kate Zhang

## Goal

Portfolio for a **design + product + frontend engineer**. The pitch (`strategy/1line-pitch.md`):

> I redesign useful but rough startup products into polished, trustworthy, customer-ready interfaces — then implement them as working frontend prototypes.

Pick YC-tier startups at PMF with rough UI. Redesign the core workflow, ship a live prototype, present as **before vs after**. Decks built by hand per project with the [`zarazhangrui/frontend-slides`](https://github.com/zarazhangrui/frontend-slides) plugin.

## Hard rules

* **Scope.** All work inside `/Users/kate/phoenix/projects/portfolio/`. Never read, write, or cite siblings.
* **Communication.** Direct, concise, logical. No compliments, apologies, or acknowledgments. Open with finding, action, or answer. Reports: logic → data → conclusion, ≤3 points. Recommendations: one option with its reason. Narration: one sentence stating intent before tool calls, silence during, skip the end-summary if the diff is obvious.
* **First principles.** Reason from the underlying problem and its constraints. Memories, docs, prior decisions are evidence, not truth.
* **Product context is the anchor.** Every redesign call grounds in the target startup's user and job-to-be-done. Prior art (Linear, Vercel, Notion, shadcn) is input to the option space, never the anchor.
* **No action is a valid action.** When the operator names a literal target and zero things match, report empty in one line. Do not substitute the nearest match.
* **Operator outranks stale sources.** When operator input conflicts with code or memory, weight the operator higher and verify the stale source. Push back with data when judgment says something is off — the right answer is the goal, not agreement.
* **Authorization.** Weak verbs — "go", "ship it", "yes", "k", "👍" — execute the most recent proposed plan end-to-end, including commit/push if proposed.
* **Persistent scope.** Once the operator grants a scope ("commit without asking", "no recap"), apply until revoked.

## Design discipline

* **DS primitives carry no business logic** — see global `~/.claude/CLAUDE.md`. Each prototype is its own app meant to feel like the target startup's product; do not extract shared abstractions across case studies.
* **No over-abstract small inline.** Single-caller JSX under ~80 lines stays inline.

## Project structure

```
portfolio/
├── CLAUDE.md
├── strategy/                    # positioning, pitch, narrative templates
├── docs/                        # canonical notes (operator-approved only)
├── .intermediate/               # scratch / drafts / audits — gitignored
├── manuals/                     # human-only — Claude does NOT read
├── case-studies/
│   ├── _template/               # copy to start a new case study
│   └── {slug}/
│       ├── meta.yml             # name, tagline, live URL, source URL, dates
│       ├── case-study.md        # diagnosis → decisions → takeaway
│       ├── before/*.{png,jpg}
│       └── after/*.{png,jpg}
├── prototypes/{slug}/           # prototype source
└── decks/{slug}/                # slide deck (frontend-slides)
```

**Slug.** Lowercase kebab-case matching the startup's product name (`linear`, `cubic`, `humanloop`). Same slug across `case-studies/`, `prototypes/`, `decks/`.

**`meta.yml`** is the single source of truth for project metadata. Decks read it; don't duplicate.

**`docs/` vs `.intermediate/`.** Canonical record vs gitignored workspace. Draft in `.intermediate/`, promote when the answer is locked. Audit screenshots → `.intermediate/audits/{slug}/`. Candidate targets → `.intermediate/targets.md`.

**Worktrees.** All git worktrees live under `.intermediate/worktrees/{slug}/` so they stay out of the main tree's git history. Branch name matches the slug.

## Workflow for a new case study

1. `cp -r case-studies/_template case-studies/{slug}` and fill in `meta.yml` + `case-study.md`.
2. Drop originals into `before/`.
3. Build the prototype under `prototypes/{slug}/`, deploy, paste the URL into `meta.yml: live_url`.
4. Capture redesign screenshots into `after/` with filenames matching `before/`.
5. Build the deck under `decks/{slug}/` with the frontend-slides plugin, sourcing copy from `case-study.md` and assets from `meta.yml`.
