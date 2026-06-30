# Blaxel — product feel & personality

Blaxel is infrastructure that runs autonomous agents in production. The product should feel like a system an operator trusts to run unattended: alert without being loud, fast without being twitchy, honest about what it's doing.

**Personality, six words:** Perpetual · Composed · Transparent · Exact · Disciplined · Swift.

---

## Perpetual

Always-on. The product is never "asleep" — sandboxes, agents, runs, and bills are live state, not a report.

**Shows up as**
- Status is current, not "last refreshed at." Counters tick. Logs stream.
- Dashboards open on system truth (running / queued / failed), not onboarding.
- No artificial "you have arrived" moments — the surface is just the latest state of the world.

**Fails when** the dashboard greets a returning operator with install instructions, or a value shown is older than the page itself.

---

## Composed

Calm, deliberate, in control. The product never raises its voice when a normal voice will do.

**Shows up as**
- One primary action per surface. Secondary actions recede.
- Motion is short, linear, informational. No bounce, no spring, no triumph.
- Color is restrained — orange is reserved for the single thing the operator should act on.
- Dialogs and panels swap, they don't pop.

**Fails when** four orange buttons compete on one page, or an invite dialog springs in like a consumer app.

---

## Transparent

The system tells the truth about what it is doing, what it knows, and what it cannot do.

**Shows up as**
- Every async action has a visible state: idle → pending → success / error. Toggles show they registered. Forms show they saved.
- Empty states say what's empty *and why* (no rows yet, no permission, gated by tier).
- Gated features explain the gate inline — no "click to discover what Tier 3 means."
- Errors include the cause and the next step, not a sad face.

**Fails when** a toggle reacts seconds later with no feedback, an empty state uses a prohibition glyph, or a tier badge is decoration without explanation.

---

## Exact

Precision is the brand. Spacing, alignment, copy, numbers — everything is measured.

**Shows up as**
- Tokens, not improvisation: one spacing scale, one type ramp, one radius system.
- Numbers carry units and a fixed precision. Currency, durations, byte sizes are consistent across the product.
- Copy uses the term the operator uses (`sandbox`, `image`, `policy`) and never reaches for cute synonyms (`fork off`, `spawn up`).
- Iconography is geometric, monoline, evenly weighted. No sprout, no sticker energy.

**Fails when** a label is `Fork off sandboxes from an image`, or four variants of "coming soon" coexist on adjacent screens.

---

## Disciplined

The system uses one rule for one thing. No exceptions for taste, no special-cases for novelty.

**Shows up as**
- One label and shape for "incoming" (not `SOON` / `Preview` / `Coming Soon` / `Coming soon!`).
- One breadcrumb convention (casing, click behavior) across all routes.
- One external-link affordance, placed the same way every time.
- Color is semantic. Green means success, not "you have a tier." Red means destructive, not "look here."
- Primitives don't carry business meaning. A `Badge` is a `Badge`; what it says is the consumer's call.

**Fails when** the same component renders three ways across three pages, or "Tier 3" is styled like a success state.

---

## Swift

The interface respects the operator's pace. Latency is a design surface.

**Shows up as**
- Navigation is instant. Sidebar expand / collapse is under 120ms or absent.
- Keyboard-first where the workflow is repeated: `Cmd+K` for navigation, shortcuts for the primary action on a page, `Esc` to dismiss.
- Page transitions don't reset scroll, focus, or selection.
- Heavy data loads progressively — table chrome and filters render first, rows stream in.

**Fails when** the sidebar's expand animation gates every nav interaction, or selecting a row sends the bulk-action bar off-screen at the bottom of the page.

---

## What the product is not

| Not | Because |
|---|---|
| Playful | Operators run real workloads; cuteness reads as inexperience. |
| Marketing-loud | This is the surface *after* the sale, not the pitch. |
| Generic SaaS | The user is a developer running infra, not a knowledge worker filing tickets. |
| Maximalist | Every additional pixel is a tax on scan time. |
| Decorative-colorful | Color is a status channel; spending it on garnish degrades the channel. |

---

## Reference points (input, not anchor)

Adjacent in feel: Vercel (restraint, density), Linear (motion discipline, keyboard), Stripe Dashboard (numeric exactness, status honesty), Fly.io (operator voice). Used to bound the option space — Blaxel's own product context decides every call.
