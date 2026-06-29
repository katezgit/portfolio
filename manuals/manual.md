# Manual — how I use this repo

Human-only notes. Claude does not read this file (per `manuals/` convention).

## The loop

1. **Pick a target.** Find a YC (or comparable) startup that just hit product-market fit but ships rough UI. Log candidates as I scout them — see "Picking targets" below.
2. **Diagnose.** Write what's wrong and why. Capture original screenshots into `case-studies/{slug}/before/`.
3. **Redesign.** Sketch / wireframe / explore in `.intermediate/{slug}/`. When the direction is locked, write it up in `case-studies/{slug}/case-study.md`.
4. **Build.** Code the prototype under `prototypes/{slug}/`. Deploy live (Vercel default). Paste the URL into `meta.yml: live_url`.
5. **Capture after.** Screenshots from the live prototype into `case-studies/{slug}/after/`. Use the **same filenames** as `before/` so the deck pairs them automatically.
6. **Ship the deck.** Run the frontend-slides plugin under `decks/{slug}/`, sourcing copy from `case-study.md` and asset paths from `meta.yml`.
7. **Share.** Drop the deck URL + live prototype URL into outreach.

## Where things live

| Folder | What | Committed? |
| --- | --- | --- |
| `strategy/` | Pitch, positioning, narrative templates | yes |
| `docs/` | Canonical, finalized notes I want preserved | yes |
| `.intermediate/` | Scratch — drafts, candidate lists, exploration screenshots, audit captures | **no** |
| `case-studies/{slug}/` | Per-startup: `meta.yml`, `case-study.md`, before/, after/ | yes |
| `prototypes/{slug}/` | Per-startup prototype source (Next.js / Vite / static) | yes (build output ignored) |
| `decks/{slug}/` | Slide deck output per case study | yes |
| `manuals/` | Human-only notes (this file) | yes |

**Rule of thumb.** If it's a draft, a candidate list I'm still triaging, an exploration I might throw away, or a debug screenshot — `.intermediate/`. Once it's the answer, promote to `docs/` or directly into a case study.

## Starting a new case study

```bash
cp -r case-studies/_template case-studies/{slug}
```

Fill in `meta.yml` first (slug, name, source_url, status: `drafting`). Drop original screenshots into `before/`. Outline `case-study.md` against the template sections — keep section 1 (Diagnosis) tight, that's the thesis.

## Slug convention

Lowercase, kebab-case, matches the target startup's product name. Same slug across `case-studies/`, `prototypes/`, `decks/`, and `.intermediate/`. Examples: `linear`, `cubic`, `humanloop`, `granola`.

## Picking targets

Filter:
- YC W24 / S24 / W25 batch (or comparable seed/Series A)
- Has paying customers (PMF signal: public testimonials, $X/mo pricing, hiring postings)
- B2B SaaS, dashboard, internal tool, or workflow product (not consumer social)
- UI is functional but visibly rough — weak hierarchy, default Tailwind look, missing states

Keep the running candidate list in `.intermediate/targets.md` (it will churn — that's fine, it's gitignored).

## Deploy targets

Default to **Vercel** for Next.js prototypes (one-command deploy, custom domain free). Cloudflare Pages or Netlify also fine. Whatever it is, put the public URL into `meta.yml: live_url` — that's what the deck and outreach point to.

## Don't

- Don't build a shared "portfolio component library" across prototypes. Each prototype should look like the target startup's product, not a unified Kate brand.
- Don't commit `.intermediate/`.
- Don't promote half-finished thinking to `docs/` — leave it in `.intermediate/` until it's the final answer.
- Don't write README/marketing files unless I explicitly want one for outreach.

## Working with Claude on this repo

- The communication style is in `CLAUDE.md` (mirrors baxel.ai discipline — terse, no acknowledgments, no padding).
- Weak verbs ("go", "ship it", "yes") authorize the most recent proposed plan end-to-end.
- For per-startup work, give Claude the slug and the phase: "diagnose `granola`", "build `granola` prototype", "draft `granola` deck".
