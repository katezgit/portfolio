# {Startup Name} Redesign

> Source: {source_url} · Live: {live_url}

## 1. Diagnosis

What's the real product? What's the user's primary job? Where does the current UI undersell that?

**Issues**
- Weak hierarchy
- Unclear primary action
- Inconsistent spacing / type
- Low-trust visual system
- Missing operational states (empty, loading, error, success)

## 2. Before

Screenshots live in `before/`. Add one-line labels per screen — let the screenshots do the work.

- `before/dashboard.png` — core action is buried
- `before/empty-state.png` — no guidance, dead page

## 3. After

Screenshots live in `after/`. Each shares a filename with its `before/` counterpart for trivial pairing.

- `after/dashboard.png` — primary action lifted to top-right, scannable in 3s
- `after/empty-state.png` — actionable empty state with one clear next step

## 4. Design decisions

| Decision | Reason |
| --- | --- |
| Reorganized around primary job | Old layout did not make the next action obvious |
| Reduced secondary visual noise | Users should know where to look within 3 seconds |
| Standardized spacing / type / components | Product needed to feel mature and trustworthy |
| Added empty / loading / error states | Real products need operational clarity |

## 5. Engineering execution

Prototype source: `prototypes/{slug}/`. Live: `{live_url}`.

Stack: React / Next.js, Tailwind, design tokens, responsive layout, interaction states.

## 6. Takeaway

One paragraph: what this case study proves about the loop — diagnose → redesign → ship.
