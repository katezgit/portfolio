# Blaxel — comments

One section per screenshot in [problems.md](problems.md). Anchors match the screenshot filename (without extension).

---

## nav-active-state-confusing

[Back to index](problems.md) · [Open screenshot](before/nav-active-state-confusing.png)

Collapsed sidebar uses a thin orange outline on the active icon. That treatment reads as hover/focus, not "you are here." With no accent bar, no fill, and no label visible, the user loses orientation the moment the sidebar collapses.

Industry pattern for collapsed sidebars (Linear, Vercel, Notion) is a clear *fill* on the active icon plus an accent color on the glyph itself — sometimes with a left vertical bar. Outline-only is the weakest possible active treatment and conflicts with hover.

---

## nav-incoming-feature-badges

[Back to index](problems.md) · [Open screenshot](before/nav-incoming-feature-badges.png)

**Verdict: poor experience.**

Three issues:

1. **Casing inconsistency.** `Preview` is sentence-case, `SOON` is all-caps. Pick one. All-caps reads louder, which is backwards here — Preview is more available than something that doesn't exist, but the louder badge sits on the absent feature.
2. **`SOON` carries no information.** No timeline, no waitlist, no signup. Industry pattern for dev infra (GCP, AWS, Vercel, Linear) is `Beta` / `Preview` / `Waitlist` — each implies a concrete next action. "Soon" is marketing filler.
3. **Affordance is missing.** A `SOON` row shouldn't look the same as a live route. Without dimmed text + `cursor: not-allowed`, users will click and bounce off a dead screen.

**Recommendation.** Three states, consistent casing, each with a concrete affordance:
- `Beta` — fully usable, in active iteration. Row is interactive.
- `Preview` — limited access, may break. Row is interactive, banner on the page.
- `Waitlist` — not shipped. Row is dimmed, click opens a join-waitlist sheet (captures intent instead of wasting it).

Single reason: every badge should map to an action the user can take *right now*. "Soon" doesn't.

---

## post-signup-no-focus

[Back to index](problems.md) · [Open screenshot](before/post-signup-no-focus.png)

**Verdict: confirmed — your read is correct, and the root cause is deeper than color.**

Visible orange-primary elements on this single canvas (counted): logo, active nav icon, inline doc links inside step 1, step 2, step 3, step 4, "Visit Docs", "Visit SDK Reference", "Visit API Reference" ×2, "Full changelog", the credits counter `10`. ~12 primary-color hits.

But the color is the symptom. The real problem: **this page is trying to serve two users at once.**

- The first-time user has exactly one job: get to "my first sandbox runs." That job is the numbered checklist on the left.
- The returning user wants reference material. That job is the right rail (Resources) and the inline doc links.

Both users get a 50/50 canvas with primary color on both sides → no hierarchy → user reads it as a wall.

**Recommendation.** Demote everything that isn't the linear install flow on the first-run view:

1. Hide the right-rail Resources panel until the user has created at least one sandbox. Replace with empty space or a single low-emphasis "Docs ↗" link in the top-right.
2. Inline doc links inside steps (`this documentation`, `log in to your workspace`) drop from orange to muted underline. They're escape hatches, not the path.
3. Step 4's "Create a new sandbox" CTA becomes the single orange button — the only orange element on the canvas above the fold.

Single reason: on first-run, the page should answer one question — "what do I do next" — with one answer. Reference material is for users who already know what they're doing.

---

## privacy-link-and-cookie-banner

[Back to index](problems.md) · [Open screenshot](before/privacy-link-and-cookie-banner.png)

Three stacked failures on one interaction.

**1. Wrong glyph.** The `↗` icon on the Privacy row is the universal "opens in new tab / external link" affordance. Clicking it triggers an in-page cookie banner in the lower-left corner — not an external page, not even a modal. The icon is lying about what's about to happen.

**2. Disconnected reveal.** Even if the glyph were correct, the result appears in a completely different region of the screen (bottom-left, while the trigger is mid-right). The user's eye has no path from action to consequence — they'll think nothing happened and click again.

**3. Misleading banner.** The banner says "You can opt out of non-essential cookies" but exposes only two controls: `OK` (which reads as accept-all) and `Do Not Sell or Share My Personal Information` (a CCPA-specific link, not a general opt-out). The literal opt-out the copy promises is nowhere on the surface.

**Recommendation.** Open a real Cookie Preferences modal, anchored to the click. Inside: a clear toggle list (`Essential` locked on, `Analytics`, `Marketing`) with Save / Cancel. Replace the `↗` on the row with `›` (or no glyph). Reason: every part of this interaction — icon, location, copy — should agree on one thing. Right now all three contradict each other.

---

## settings-name-page-mixed-ia

[Back to index](problems.md) · [Open screenshot](before/settings-name-page-mixed-ia.png)

**Verdict: both reads are correct, and they share one root cause — the nav label is far narrower than the page contents.**

The left-nav item says `Name`. The breadcrumb says `SETTINGS > NAME`. But the page actually carries: display name, name, workspace ID, account ID, account owner, sandbox settings, and workspace deletion. Of those seven groups, exactly one is about a name.

Three specific failures:

1. **Nav label is too narrow for the page.** "Name" predicts a name field. It does not predict workspace deletion. Either the label is wrong or the page is over-stuffed. Industry convention for this kind of page is `General` (GitHub, Vercel, Linear, Stripe).
2. **Sandbox settings sits on a workspace-identity page.** Sandbox-level config doesn't belong with workspace-identity fields. It either deserves its own nav item under Workspace (`Sandbox defaults`), or — better — moves out of Settings entirely and lives in the Sandboxes section of the app, since that's where its users mentally are.
3. **`Delete workspace` is orphaned.** Destructive actions need a `Danger zone` container with header + description + the action button (GitHub's pattern, copied by Vercel, Linear, Supabase). A bare red button floating below an unrelated toggle is the worst possible treatment — high consequence, zero scaffolding.

**Recommendation.** Rename the nav item from `Name` to `General`. Move `Sandbox settings` out of this page (own nav item or out of Settings entirely). Wrap `Delete workspace` in a `Danger zone` card with explicit header and a one-line description of what will be deleted.

Single reason: a settings page's label should predict its contents, and destructive actions should never appear without a header that says "this is destructive."

---

## settings-name-form-edit-vs-readonly

[Back to index](problems.md) · [Open screenshot](before/settings-name-form-edit-vs-readonly.png)

**Verdict on copy buttons: confirmed.** Dark filled tiles at full input height, equal weight to the value they sit beside. Copy is a secondary utility — should be a ghost icon at `text-muted`, only resolving to full color on hover/focus.

> **The product already has the right pattern elsewhere.** The top-right billing-email chip uses a thin outlined glyph at text-muted (see [validated example](before/copy-button-good-pattern.png)) — operator confirmed this is the desired treatment. The fix here is to make the workspace-name form copy buttons match the billing-chip style, not to invent a new one. This is a consistency problem, not a design problem.

**But there's a bigger problem on this page than the copy button styling.**

Five fields render with identical input chrome (rounded rect + border):
- `Workspace display name` — editable
- `Name` — read-only (same value as display name)
- `Workspace ID` — read-only
- `Account ID` — read-only
- `Account owner` — read-only

The user can't distinguish editable from read-only without clicking. Compounding it:
- The CTA reads `Update workspace`, implying multi-field updates. Only one field updates.
- `Workspace display name` and `Name` both show `katezbuilds` — presumably display vs slug, but the labels don't carry the distinction.

**Recommendation.** Two visually distinct sections:

1. **Editable** — `Workspace display name` in input chrome, button `Save changes`. Disabled until dirty.
2. **Identity (read-only)** — `Name (slug)`, `Workspace ID`, `Account ID`, `Account owner` rendered as plain label + monospace value + ghost copy icon. No input border. Group heading "Identity" or "Read-only details".

Single reason: editable fields and read-only data should never look the same — otherwise users learn what's editable through failed clicks.

---

## toggle-no-loading-state

[Back to index](problems.md) · [Open screenshot](before/toggle-no-loading-state.png)

**Verdict: confirmed, and there are two failures stacked, not one.**

1. **Off-state is too muted.** The grey track + grey thumb reads as `disabled`, not `off`. Convention (Apple HIG, Linear, Vercel, shadcn) keeps the off-state thumb white-on-grey with strong contrast, so off and disabled stay visually distinct. Right now the user has to test the click to know whether the control is interactive at all.
2. **No loading state during the API round-trip.** After click, the toggle does nothing visible until the server responds. With a slow API this reads as "the click failed."

These compound: a muted off-state + a silent loading window means the user clicks, sees nothing change, assumes the control is dead, and either gives up or rage-clicks.

**Recommendation: optimistic update + spinner in the thumb.**

- On click: toggle visually moves to the new position immediately. Replace the thumb circle with a small spinner (same diameter).
- On API success: spinner resolves to a solid thumb. Done.
- On API failure: toggle snaps back to the original position + inline error or toast.

Also bump the off-state contrast so it doesn't read as disabled at rest.

Single reason: every interactive control should respond within 100ms — anything slower needs an explicit progress signal, or users assume failure.

---

## integrations-page-layout

[Back to index](problems.md) · [Open screenshot](before/integrations-page-layout.png)

**Verdict: layout optimized for browsing, but the actual job here is find-by-name.**

The page has **three nested navs before content**: global icon rail → Settings sub-nav (Profile / Account / Workspace) → Categories filter column (All / Enabled / Model / Mcp server, four items). Each layer eats horizontal real estate; only the third one is integration-specific, and it carries four options that would fit in chip tabs above the grid.

Three concrete failures:

1. **Categories column is wasteful.** Four filters in a full-height column at the same width as the Settings nav. Replace with chip tabs at the top of the content area — same job, ~80% less chrome.
2. **Search is undersized for its importance.** This is a marketplace with dozens of integrations. The user's primary job is *"find Anthropic / OpenAI / Stripe"*. Search should be the page's largest input, full-width above the grid, autofocused on mount — not a small box buried in the filter column.
3. **Card grid optimizes for discovery, not lookup.** Each card carries a 40px icon, 2-line description, and a badge — beautiful for first-visit browsing but slow for "scan to a specific name." Names sit inside boxes that compete with each neighbor.

**Recommendation: dual-mode layout, default to list.**

- Move Categories to chip tabs across the top: `All · Enabled · Model · MCP server`.
- Make search the dominant control: full-width input above the grid, autofocus, ⌘K keybinding.
- Add a `Grid / List` toggle. **Default to List** — dense rows: `icon · name · category badge · enabled state`. Grid stays as the discovery view.
- This frees the left sidebar to collapse the Settings sub-nav once Integrations becomes its own surface (it's more marketplace than settings; consider lifting it out).

Single reason: this surface should be tuned to the primary job here — *find one specific named integration* — not generic browsing. List view + prominent search optimizes for that.

---

## breadcrumb-inconsistency

[Back to index](problems.md) · Screenshots: [clickable](before/breadcrumb-clickable.png) · [not clickable](before/breadcrumb-not-clickable.png)

**Verdict: confirmed — same component, three different conventions on the same product.**

Side-by-side:

| | Integrations page | Preferences page |
|---|---|---|
| Casing | `Integrations > Anthropic` (title case) | `SETTINGS > PREFERENCES` (all caps) |
| Parent interactivity | Clickable, returns to parent | Not clickable, decorative |
| Type weight | Normal, white text | Smaller, dimmed |

User learns on the Integrations page that the parent segment is navigable, then hits Preferences and finds the same-shape thing is dead text. The pattern they just trained on collapses one click later.

Worse: the breadcrumb on Preferences is purely cosmetic — it tells the user nothing they don't already know from the left-nav (`Profile > Preferences` is highlighted). At that point it's chrome that confuses without informing.

**Recommendation.** One breadcrumb component, applied everywhere, with these rules:

1. **Always title case** (`Settings > Preferences`). All-caps reads like a section header, not a path.
2. **Parent segments are always interactive links** with hover state. Current segment is plain text.
3. **Render only when the breadcrumb adds information** the left-nav doesn't already convey — e.g. `Integrations > Anthropic` (drilldown into a specific integration). On top-level settings pages where the left-nav already shows where you are, omit the breadcrumb entirely.

Single reason: a breadcrumb teaches a navigation pattern through repetition. Inconsistent shape, casing, or interactivity untrains the pattern faster than no breadcrumb at all.

---

## addons-coming-soon-looks-disabled

[Back to index](problems.md) · [Open screenshot](before/addons-coming-soon-looks-disabled.png)

**Verdict: confirmed. Rounded + bordered + muted + button-sized = the universal "disabled button" pattern.** Users will try to click and read the no-op as "I'm not eligible" or "broken."

This is the same root problem as [nav-incoming-feature-badges](#nav-incoming-feature-badges), seen on a different surface — and the two surfaces don't even agree with each other:

| Surface | Label | Treatment |
|---|---|---|
| Sidebar (Agents) | `SOON` | Tiny all-caps pill |
| Sidebar (Agent Drives) | `Preview` | Sentence-case pill |
| Add-ons (3 cards) | `Coming Soon` | Button-shaped, muted, looks disabled |

Three labels, three treatments, one concept.

**Recommendation: replace the dead pill with a real call to action — `Notify me`.**

- Button is interactive, not muted. Secondary style (not orange — that stays reserved for the actual purchase CTA once the feature ships).
- Click captures email (or uses the signed-in account) → adds to a waitlist on the backend.
- Card itself stays at slightly reduced opacity so it's clear the feature isn't live, but the button is clearly clickable.

Pair this with the recommendation in [nav-incoming-feature-badges](#nav-incoming-feature-badges) — both surfaces should converge on the same three-state vocabulary (`Beta` / `Preview` / `Waitlist`) and the same interaction pattern.

Single reason: if the feature isn't shipped, the user's contribution is *interest* — give them a button that captures it, not a label that looks broken.

---

## settings-subnav-instability

[Back to index](problems.md) · Screenshots: [open](before/settings-subnav-open-annoying.png) · [collapsed](before/settings-subnav-collapsed-disoriented.png)

**Verdict: confirmed — the Settings sub-nav is a hover-flyout pretending to be a navigation rail. Three stacked failures.**

1. **The panel is unstable.** It opens on hover, vanishes on hover-off. To keep it visible, the user has to either keep the mouse parked over the trigger or find the `>` pin button to lock it open. Pin-to-keep-open is a fine pattern for *occasional* utility panels — it is a terrible pattern for the primary nav of the section the user is currently inside.
2. **The pin button is undiscoverable.** A `>` glyph alongside a "SETTINGS" label reads as a section-collapse chevron, not a pin/lock affordance. You confirmed: you didn't realize the button was there. That's not a one-user fluke — it's the predictable failure mode of using a chevron to mean "stick this open."
3. **Collapsed state destroys orientation.** When the sub-nav is closed (image 16), the only on-screen anchor that says "you are in Settings" is the small breadcrumb text `SETTINGS > TOP-UP`. The global left rail shows app-wide icons (Sandboxes, Volumes, etc.) — outer context. Inside Settings, the user has no visible inner context. They feel dropped into a page from nowhere.

**Recommendation: drop the hover-flyout and the pin entirely. Make the Settings sub-nav a persistent, always-visible column inside the Settings section.**

- When the user enters `/settings/*`, the sub-nav renders as a fixed left column alongside the page content. No hamburger, no pin, no hover.
- The global icon rail can stay (giving cross-app navigation), but the Settings sub-nav is *always there* whenever the URL is under `/settings`.
- This also closes the gap with [integrations-page-layout](#integrations-page-layout) — three nested navs were partly the problem; here the right fix is "the inner nav is stable and discoverable, the global rail collapses to icons."

Single reason: the navigation for the section you're inside should be the most stable thing on the page, not the least. Hover-shown navigation makes users feel they're navigating against the product instead of through it.

---

## sidebar-expand-animation-slow

[Back to index](problems.md) · [Open screenshot](before/sidebar-expand-animation-slow.png)

**Verdict: confirmed. The animation is pure tax with no informational payoff.**

The width transition from icon-only to full-labels carries no information the user doesn't already know — the labels appear, the layout shifts, the user reads. A slow ease just delays the moment when the user can act. Industry timing for nav reveals:

- Linear: instant (no animation)
- Figma: instant
- Vercel: ~120ms
- Notion: ~150ms ease-out

Anything past 200ms enters "I'm waiting for the UI." Past 300ms it reads as sluggishness. The current animation is well into the "impatient" zone.

This compounds with [settings-subnav-instability](#settings-subnav-instability) — the user already has to fight the inner Settings nav, and now the outer rail makes them wait too. Every nav-related interaction is taxed.

**Recommendation.** Cut the animation to 120ms ease-out, or remove it entirely.

Single reason: navigation chrome should respond at human-reaction speed (≤150ms). Anything slower trains users to dread the next click.

---

## invoices-empty-state-banned-icon

[Back to index](problems.md) · [Open screenshot](before/invoices-empty-state-banned-icon.png)

**Verdict: confirmed. The glyph is "prohibited," not "empty."**

The ⊘ symbol (circle with diagonal slash) is the universal "no entry / forbidden / banned" icon — used on traffic signs, OS-level permission denials, and platform restrictions. On a billing page especially, the user's first read is "I'm not allowed to see invoices" or "my account is restricted," not "no invoices yet."

Empty-state iconography should signal **absence of the entity**, not **prohibition of access**. The two readings have opposite emotional payloads — one is a calm "nothing here yet," the other is an anxious "you're blocked."

**Recommendation.** Replace the glyph with a faded document/receipt icon (the same icon used in the side-nav for `Invoices`, at ~30% opacity). Pair with two-line copy:

> No invoices yet
> Invoices will appear here after your first billing cycle.

This converts the empty state from "you can't have this" to "nothing here yet, and here's when something will appear" — useful information instead of a dead end.

Single reason: an empty state should look like a faded version of what would be there. Prohibition glyphs only belong on real prohibitions.

---

## topup-modal-size-jump

[Back to index](problems.md) · Screenshots: [one-time](before/topup-modal-one-time.png) · [monthly](before/topup-modal-monthly.png)

**Verdict: confirmed — you're not wrong. The modal roughly doubles in area when the user switches from One-time to Monthly.**

Eyeballing the two states:
- One-time: single column, ~600×700.
- Monthly: two columns with a detail rail, ~1100×1100.

Same modal, same trigger, same tabs — but the container itself rescales around the click. That's not a tab swap; that's a different modal.

Why it happens: Monthly needs to expose tier benefits because the user is committing to recurring spend; One-time is just an amount picker. Both reasons are legitimate in isolation. The pair breaks because the tab control is making a promise — *"switching me changes the content, not the container"* — and the implementation breaks that promise.

Three consequences:

1. **Spatial surprise.** The modal grows past where the user's eye was anchored. Buttons (`Cancel`, `Continue`) move to new positions, so the next click target shifts.
2. **Tab semantics violated.** Tabs train users that the container is stable. When this one isn't, users start distrusting tabs across the product.
3. **Asymmetric information density.** One-time gives the user *less* useful context than Monthly — but One-time is the higher-trust action (immediate charge). It should get *more* affirmation, not less.

**Recommendation: lock the modal to the larger (Monthly) size. Use the right rail on One-time to show a `Your purchase summary`.**

- `One-time` right rail: amount selected × credits = total, plus what that buys ("≈ X standby sandbox-hours," "≈ Y MCP calls").
- `Monthly` right rail: stays as-is (`About Tier N`).
- Container stays at the same dimensions; only the rail's content swaps. Tabs now keep their promise.

Bonus: One-time gains a value-affirmation that probably increases completion (the user sees what they're getting before clicking Continue).

Single reason: tabs should change content inside a stable container — never the container itself.

---

## dual-sidebar-ctrl-b-only-outer

[Back to index](problems.md) · [Open screenshot](before/dual-sidebar-ctrl-b-only-outer.png)

**Verdict: confirmed. Two competing left rails + a keyboard shortcut that only controls one of them = friction by design.**

Measuring chrome on this page:
- Global rail (expanded): ~320px
- Settings sub-nav: ~340px
- Total left chrome: ~660px

Result: on this viewport the right-side `Add payment method` button is **clipped off the canvas** before the user can scroll to it. Two rails are eating more than half the screen on a page that has roughly 200px of actual content.

`Ctrl+B` only collapses the outer rail. The user's mental model is *"this shortcut closes the left chrome"* — but it leaves the inner Settings sub-nav fully present. To shrink the inner one the user has to find the `>` glyph (see [settings-subnav-instability](#settings-subnav-instability) — already established as undiscoverable).

This is the same architectural failure as [integrations-page-layout](#integrations-page-layout) and [settings-subnav-instability](#settings-subnav-instability): **two columns are trying to be "the left nav" at the same time.**

**Recommendation: when the URL is under `/settings/*`, the global rail collapses to a thin "exit settings" affordance (logo + back arrow ~48px wide), and the Settings sub-nav becomes the primary left rail. `Ctrl+B` then operates on the single visible rail.**

This is what Stripe, Vercel, and Linear all do — Settings is a mode, not just a page. While the user is inside it, there is one left rail, not two. When they leave settings, the global rail returns.

Single reason: at any moment a user is inside one section. Two rails make the user mentally arbitrate which one is "theirs." Consolidate to one and the question disappears — along with the clipped-content problem and the half-working keyboard shortcut.

---

## whats-new-icon-consumer-vibe

[Back to index](problems.md) · [Open screenshot](before/whats-new-icon-consumer-vibe.png)

**Verdict: confirmed. The sprout glyph is consumer-tier iconography on an enterprise-tier product.**

A growing seedling reads as: organic, friendly, casual, growth. Those are the visual codes of consumer / wellness / "vibe coding" tools (Replit, Lovable, v0, Bolt). They are the wrong codes for the buyer Blaxel is asking to trust an AI-runtime platform with production workloads.

Look at how enterprise dev-infra products signal "release notes / what's new":
- Linear: ✨ sparkles
- Vercel: 📢 megaphone
- Stripe: changelog book / "Changelog" link with no icon
- Cloudflare: lightning glyph or simple `NEW` text badge
- DataDog: bell with badge

Common thread: **restrained geometric primitives, no organic illustrations**. The sprout breaks that system here.

Even within this dropdown the icon family is inconsistent — `Status page` uses a heartbeat-line (also more consumer than convention; enterprise uses status dots or bar charts), and most other rows use neutral outline glyphs. The sprout is the loudest break.

**Recommendation: replace with ✨ (sparkles) or a small filled `NEW` text badge.** Reason: signals "release notes" inside the established visual code of enterprise dev tools. Bonus: same change tightens the icon system across the dropdown (a single audit pass should standardize Status page too).

Single reason: every icon is a tone signal to the buyer. Enterprise developers reading a sprout will subconsciously price-anchor this product against Replit, not against AWS.

---

## sidebar-credit-card-eats-space

[Back to index](problems.md) · Screenshots: [takes space](before/sidebar-credit-card-takes-space.png) · [competes on policies](before/sidebar-credit-card-competes-on-policies.png)

**Verdict: confirmed. The bottom-of-sidebar card is doing three jobs that shouldn't all be persistent, and the cost is paid by navigation.**

The card carries: `Tier 0` header + `credit balance · $10` + `1/6 tasks to earn free credit` + progress bar. Roughly 150px of permanent vertical real estate at the bottom of every page, every navigation. Consequences visible in the screenshots:

1. **Nav items get pushed below the fold.** In the first screenshot, `Custom domains` is the last visible item in the upper nav; the scroll affordance (down-arrow chevron) shows there's more underneath. Reaching Policies takes a sidebar scroll *every time*.
2. **Persistent visual heaviness.** Three rows + a tier banner + a progress bar at the bottom of the chrome — the user's eye keeps being pulled there even while they're trying to navigate. The operator's read ("hard not to touch the credit card") is real: it's sized like a primary CTA.
3. **Duplication on upsell pages.** On the Policies page (second screenshot), the page itself already carries `Ready to ship? Add a payment method → Upgrade tier`. The sidebar's credit card is now competing with the page's own upsell — two CTAs aimed at the same conversion, sized similarly, both on screen.

Looking at how peer products handle persistent account state:
- Linear: small avatar + plan badge in the bottom-left, click for a popover.
- Vercel: usage indicator collapses to a single thin row inside the user menu.
- Stripe: account/billing state lives behind the user avatar, not in the nav.

Pattern: **persistent identity / quota state lives in the user-profile slot, not in a dedicated card.** Onboarding tasks live in a dismissible right-side prompt or a `?` help-checklist drawer — not in permanent left-rail real estate.

**Recommendation: collapse the card into the user-profile row, surface onboarding tasks elsewhere, and let the page own its upsell.**

1. Bottom-left becomes a single row: avatar `katezbuilds · Tier 0 · $10` with a chevron. Click opens a small popover with the same credit info + a `Top up` link. ~36px tall instead of ~150px.
2. The `1/6 tasks to earn free credit` checklist moves to a dismissible toast or a `?` button next to Help — out of the permanent nav.
3. On pages that already carry a tier/upgrade CTA in their content (Policies, future Tier-1+ feature pages), the sidebar's slim credit row stays neutral — the page's CTA carries the upsell.

Single reason: persistent upsell chrome eats sidebar space and competes with whatever upsell the page itself is showing. Sidebar should help users navigate; upsell should be contextual to what they're trying to do.

---

## tier-badge-no-guidance

[Back to index](problems.md) · [Open screenshot](before/tier-badge-no-guidance.png)

**Verdict: confirmed. The badge does the most important job on the page silently — it tells the user "this page is locked" — but it's styled to look like a decorative status tag.**

Three layered failures:

1. **Wrong color semantics.** Pale green = success / "active" / "shipped" in every other context this user has seen (status pages, deployments, CI checks). Using green for a *gating requirement* the user hasn't met inverts the convention — the badge reads as a positive label, not a lock indicator.
2. **No tooltip, no hover explanation.** The badge sits next to the page title with no affordance saying *what `Tier 3` means in this context*. User only finds out by clicking it. That's the worst possible discovery model for a gate.
3. **Redundant with the page's own messaging.** The right rail on this page already says `Ready to ship? Upgrade to Tier 3 to start creating custom domains.` The badge near the title repeats the same concept without the explanation — and pulls attention with an arrow icon, while delivering less information.

Same root family as [nav-incoming-feature-badges](#nav-incoming-feature-badges), [addons-coming-soon-looks-disabled](#addons-coming-soon-looks-disabled), and the tone problem in [whats-new-icon-consumer-vibe](#whats-new-icon-consumer-vibe): **every persistent label across the product should be self-explanatory at a glance and map to one concrete action.** Right now badges are decoration; they should be controls.

**Recommendation.** Replace the pale-green pill with an inline link-styled affordance: `🔒 Requires Tier 3 →` next to the title.

- Color: neutral (text-muted) with a lock glyph. Not green. Not a pill.
- Hover state shows a tooltip: `Custom Domains is available on Tier 3 and above. You're on Tier 0.` + a `Compare tiers` link.
- Click goes directly to the Top-up / Quotas tiers page with Tier 3 pre-highlighted.
- The page's right-rail card stays — they now reinforce each other instead of duplicating.

Single reason: a badge that gates content should look locked, explain itself on hover, and link to the unlock path. A green pill that needs a click to decode is friction disguised as decoration.

---

## invitations-empty-state-raw

[Back to index](problems.md) · [Open screenshot](before/invitations-empty-state-raw.png)

**Verdict: confirmed. This isn't an empty state — it's a missing one.**

What's on screen: the literal string `No invitations found`, top-left of the content area, in dark-grey text. Nothing else. No icon, no centered framing, no card, no CTA, no copy explaining what invitations are.

Three failures stacked:

1. **Wrong location vs eye path.** The user's attention naturally lands at the center of an empty canvas. The text sits top-left, hugging the breadcrumb. Visual gravity points to where the message *isn't.*
2. **No next action.** The only reason a user is on this page is to invite teammates or check pending invitations. With zero of either, the obvious next move is *"invite a teammate"* — and there's no button anywhere on the page.
3. **Inconsistent with other empty states.** [invoices-empty-state-banned-icon](#invoices-empty-state-banned-icon) at least has an icon (even if it's the wrong one — a prohibition glyph). This page has no icon at all. Two empty-state treatments across the product = no system.

**Recommendation: centered empty-state pattern with three elements.**

```
        [envelope icon, faded]

      No invitations yet
   Invite teammates to collaborate
       in this workspace

       [ + Invite a teammate ]   ← primary button
```

- Icon centered horizontally, ~80px, at ~30% opacity.
- Two-line copy (state + explanation).
- Primary CTA — the same one that exists on the populated state of this page.

Pair this with [invoices-empty-state-banned-icon](#invoices-empty-state-banned-icon) — both should land on the same template, just with different content. The product needs **one** empty-state component used everywhere: `EmptyState(icon, title, description, primaryAction?)`.

Single reason: empty states should be calm, centered, designed surfaces with a clear next action. A line of orphan text at top-left looks half-built and tells the user nothing about what to do.

---

## billing-email-chip-misplaced

[Back to index](problems.md) · [Open screenshot](before/billing-email-chip-misplaced.png)

**Verdict: confirmed. Three failures explain why you missed it for multiple visits.**

1. **Wrong corner.** Industry convention puts workspace / org / account identity in the **top-left** — Linear, Vercel, Notion, GitHub, Stripe, Supabase, Cloudflare all place it there. The user's first orientation glance is top-left, then top-right (notifications, profile). Putting the billing-account chip top-right pushes a primary orientation signal into a secondary attention zone.
2. **Wrong primary label.** Showing an email reads as "you are signed in as this user" — a personal-identity statement. Billing/account context should read as a *thing the user belongs to*: workspace name, org name, plan badge. The credit-card glyph next to the email tries to disambiguate, but it's smaller than the email itself, so it loses.
3. **Fragmented account state.** Account identity is currently scattered across three corners:
   - Top-right: billing email chip
   - Bottom-left: `katezbuilds · katezbuilds` workspace name + credit balance card (see [sidebar-credit-card-eats-space](#sidebar-credit-card-eats-space))
   - Sidebar nav: workspace settings under `Settings > Name` (see [settings-name-page-mixed-ia](#settings-name-page-mixed-ia))

   No single place answers *"who am I, what workspace, what tier, what balance, what's billing to whom?"*. The user has to assemble the picture from three locations.

**Recommendation: consolidate account identity into one top-left surface, with the chip as the trigger and a popover carrying the detail.**

Top-left of the sidebar (or top-left of the page header):
```
[avatar] katezbuilds · Tier 0           ⌄
```

Click opens a popover with:
- Workspace name
- Billing account: `katexuzy@gmail.com` (copy icon)
- Tier · Credit balance
- `Switch workspace` link
- `Billing settings` link

This single surface replaces:
- The top-right email chip (delete)
- The bottom-left credit card (collapse, see [sidebar-credit-card-eats-space](#sidebar-credit-card-eats-space))
- Whatever workspace-switcher entry point exists today

Single reason: account identity is one concept. Surfacing it across three corners with three different treatments forces every user to do the work the design should have done.

---

## invite-dialog-naughty-animation

[Back to index](problems.md) · [Open screenshot](before/invite-dialog-naughty-animation.png)

**Verdict: confirmed. Spring/bounce entrance is the wrong tone for a workspace trust action.**

`Invite user` is a permission-granting action — the user is about to give someone access to their workspace and (transitively) their billing. The dialog should *feel* like a deliberate, considered moment. A bouncy entrance reads playful, almost joke-y — it gives the form a register closer to a game-UI achievement popup than to "grant production access."

How peer products animate the same kind of dialog:

- Linear: fade + small scale (95 → 100%), ~150ms ease-out. No overshoot.
- Vercel: fade + small Y-translate, ~120ms.
- Stripe: fade + scale to 100%, ~150ms.
- Notion: simple fade, ~100ms.

None use spring physics. Spring is reserved for delight surfaces (consumer onboarding, gamification, hero illustrations) — not for trust actions.

This is the third instance of the same root issue in this audit: motion + iconography are skewing consumer when they should skew enterprise.
- [whats-new-icon-consumer-vibe](#whats-new-icon-consumer-vibe) — sprout glyph
- [sidebar-expand-animation-slow](#sidebar-expand-animation-slow) — slow nav animation
- This row — bouncy dialog entrance

**Recommendation.** Replace the spring with `fade + scale(0.96 → 1.0)` at ~150ms ease-out. No overshoot, no bounce. Apply the same easing curve to every modal/dialog/popover in the product.

Single reason: animation tone signals product category. Spring/bounce = consumer/playful. Ease-out fade = enterprise/calm. Inviting a teammate is a trust action; its dialog should feel calm.

---

## bulk-action-bar-too-far

[Back to index](problems.md) · [Open screenshot](before/bulk-action-bar-too-far.png)

**Verdict: confirmed. Bottom-fixed bulk-action bar is a Gmail-era pattern that breaks on modern wide viewports.**

The user just clicked a checkbox in a table near the top of the page. The acknowledgement (`1 item selected · delete · close`) appears 600-800px away at the bottom of the viewport. On a 1440p screen the user's eye has no reason to travel that far — the source of the action and the result of the action are in different zones.

Why this pattern existed historically: small viewports (1024×768), short pages, the bottom bar was always near the table. On wide monitors with sparse tables, the bar is in a different attention region from the selection.

How peer products solve it today:

- **Linear** keeps actions in the toolbar above the table; the selected count appears in the same row as filters.
- **Notion**: action bar appears at the top of the database, not the bottom.
- **Airtable**: bulk-action bar slides in at the top of the data grid, replacing the filter row.
- **GitHub**: actions appear inline at the top of the issue list when items are selected.

Common pattern: **bulk actions live in the same horizontal band as the table's filter/header toolbar.** The user's eyes never leave the selection zone.

**Recommendation.** When one or more rows are selected, the row above the table header (`Role | Source | Status | … | Columns`) transforms in place: `1 item selected · [Delete] [Clear]`. Same vertical region as filters, no new floating chrome.

Single reason: action bars should appear in the eye-path of the selection. A page-bottom dock disconnects source and result on any viewport over ~900px tall.

---

## checkbox-unchecked-looks-active

[Back to index](problems.md) · [Open screenshot](before/checkbox-unchecked-looks-active.png)

**Verdict: confirmed. The unchecked control wears the active color, destroying the state distinction.**

A checkbox has three visual states that must be distinguishable at a glance:

| State | Convention | What this product does |
|---|---|---|
| Unchecked | Neutral border (text-muted / grey) | **Orange border** |
| Checked | Primary fill (orange) | Orange fill ✓ |
| Indeterminate | Primary fill + dash | Orange fill + dash |

When unchecked already uses the primary color, the user can't tell at rest whether the control is interactive-but-empty or already engaged with the row. The orange leaks state weight into a neutral position.

This is a sibling of [toggle-no-loading-state](#toggle-no-loading-state) (off-state too muted, looks disabled) and [nav-active-state-confusing](#nav-active-state-confusing) (active state too weak). The product has no consistent visual vocabulary for control states — sometimes "off" looks disabled, sometimes "off" looks active, sometimes "active" looks like hover.

**Recommendation.** Lock the checkbox states to convention:
- Unchecked → `border: text-muted` (neutral grey), transparent fill.
- Hover → `border: text-default` (slightly brighter), still transparent.
- Checked → `bg: primary-orange`, white ✓.
- Indeterminate → `bg: primary-orange`, white dash.

Audit the product for any other controls where the "off" state borrows the active color — this is likely a token-level theming choice, not a one-off bug.

Single reason: a control's rest state should look at rest. Primary colors signal active engagement; using them on neutral states is what creates the "active for no reason" confusion.

---

## settings-name-buttons-and-spacing

[Back to index](problems.md) · [Open screenshot](before/settings-name-buttons-and-spacing.png)

Two distinct visual problems on the same page, both compounding [settings-name-page-mixed-ia](#settings-name-page-mixed-ia).

**1. Update vs Delete look equivalent.** Both buttons are fully saturated, full-pad fills — orange for Update, red for Delete. Equal visual weight tells the user *"these are two actions of the same kind."* They aren't. One is recoverable; the other ends the workspace.

Industry convention for destructive actions in enterprise products:

- **GitHub:** outlined red button inside a labeled "Danger zone" card.
- **Linear:** ghost button with red text label, no fill at rest.
- **Stripe:** muted red text inside a separated section.
- **Vercel:** ghost button with red text + confirmation modal.

Common thread: **destructive actions are visually subordinate to constructive ones, not equal.** A full red fill says *"click me"* with the same energy as the primary save — and that's exactly wrong when the consequence is irreversible.

**2. Form content is glued to the sub-nav edge.** The Name field, ID fields, and buttons all start with no left padding off the Settings sub-nav. Two columns of UI butt directly against each other — looks unfinished and makes the form feel like a continuation of the nav rather than its own region.

**Recommendation.**

1. **Destructive treatment.** Drop `Delete workspace` from a saturated red fill to a ghost button with red text (`Delete workspace` in red, transparent background). Wrap it in the `Danger zone` card from [settings-name-page-mixed-ia](#settings-name-page-mixed-ia) recommendation. Confirmation modal stays as the safety net.
2. **Spacing.** Add `padding-left: 32px` (or whatever the design system gutter is) to the form region, so it visually separates from the sub-nav.

Single reason: visual weight should follow consequence. A destructive action that looks the same as a save action is mis-priced — and a form glued to a nav edge looks half-laid-out.

---

## service-accounts-content-too-wide

[Back to index](problems.md) · [Open screenshot](before/service-accounts-content-too-wide.png)

**Verdict: confirmed. Content area has no max-width, so wide viewports stretch the page into something the user has to scan instead of read.**

Visible failures on a wide monitor:

1. **`Create service account` is ~1500px away from where the eye lands.** The page title is top-left, the only row of table content is left-aligned, but the primary CTA sits at the far right edge of the viewport. The user has to do a deliberate saccade across the entire screen to find the button.
2. **Table columns become sparse.** Four columns (Name, Client ID, Client Secret, Created at) get stretched across the full width with huge inter-column gaps. The single row of actual data looks orphaned.
3. **Search input also feels stranded.** Top-left, ~250px wide, in an ocean of empty space.

Enterprise dev-tool convention is a constrained content column:

- **Linear:** ~1200px max-width, centered.
- **Stripe Dashboard:** ~1280px max-width.
- **Vercel:** ~1200px, centered.
- **GitHub:** ~1280px on most settings surfaces.

The reason: reading and scanning ergonomics don't scale with viewport size. A 27" monitor doesn't mean the user wants to read across 27 inches.

This is the same architectural gap as [integrations-page-layout](#integrations-page-layout) — the product has no consistent content-width strategy. Works on narrow viewports, falls apart on wide ones.

**Recommendation.** Wrap every settings content area in `max-width: 1200px` and center it in the available canvas. The CTA, search, and table all sit in the same column at the same horizontal extent.

Single reason: page width should follow reading ergonomics, not viewport width. Letting content stretch to monitor edges makes the user do work the layout should be doing.

---

## topup-too-many-primary-buttons

[Back to index](problems.md) · [Open screenshot](before/topup-too-many-primary-buttons.png)

**Verdict: confirmed. Four primary CTAs above the fold = zero primary CTAs.**

Counted:
1. `Add to credit balance` — top-right of the Tier 0 card
2. `Add payment method` — inline in the warning banner
3. `Add payment method` — right side of the Auto top-up card (duplicate copy of #2)
4. `Set up` — right side of the Monthly top-up card

Every one of them carries the same orange fill and the same visual weight. When everything is primary, nothing is. The user can't read the page as a sequence — *"this first, then this"* — because the page never tells them what comes first.

Three layered problems:

1. **Workflow inverted.** To top up, the user needs (a) a payment method, then (b) credits. The page shows `Add to credit balance` first (top), then the `Add payment method` warning second. Same-color buttons mean the user can't tell the prerequisite from the consequent.
2. **CTA duplicated.** `Add payment method` appears twice (warning banner + Auto top-up card). If they go to the same flow, one is noise. If they go to different flows, the duplicate label is a lie.
3. **State-blind hierarchy.** Auto top-up and Monthly top-up both have primary `Set up` buttons even though they're useless until a payment method exists. The page treats prerequisites and downstream options as equally available.

Same root family as [post-signup-no-focus](#post-signup-no-focus): the product treats orange as a default style for "anything clickable," not as a hierarchy signal.

**Recommendation: state-aware primary actions. Exactly one primary CTA per page state.**

While no payment method exists:
- `Add payment method` — single primary action, prominent at the top of the page.
- `Add to credit balance`, `Set up` for Auto, `Set up` for Monthly — all rendered as **disabled secondary** with a tooltip: *"Add a payment method first."*

Once a payment method exists:
- Remove the warning banner entirely.
- `Add to credit balance` becomes the primary action (the most common next move).
- `Set up` for Auto / Monthly become secondary ghost buttons (configuration choices, not the default path).

Single reason: a page should communicate "do THIS next" through visual hierarchy. Multiple primary CTAs mean the page is unsure what the user is doing here — and so is the user.

---

## workspaces-row-menu-mixed-actions

[Back to index](problems.md) · [Open screenshot](before/workspaces-row-menu-mixed-actions.png)

**Verdict: agreed. `Go to workspace` should come out of the menu.**

The user's reason for being on `Settings > Workspaces` is to *manage* the billing account's workspaces — create, rename, delete, audit. Navigating into a workspace is a different mental category: it's a context switch, not a workspace-management action. The current row menu mixes the two:

| Menu item | Category |
|---|---|
| Go to workspace | **Navigation** (where am I going?) |
| Rename | Management (what am I doing to this thing?) |
| Delete | Management |

Mixing the two costs in two ways:
1. **Discoverability budget wasted.** The `⋯` menu is the surface for "things I can do TO this row." Burying a navigation action there means users have to open the menu to find a destination — heavier than necessary.
2. **Row-as-link affordance broken.** When a navigation action lives in the menu, the row name itself loses its conventional "click me to go there" reading. Stripe, Vercel, Linear all let users click the entity name in a list to navigate into it; this product hides that move inside a menu.

**Recommendation: lift `Go to workspace` out of the menu. Make the workspace name itself the navigation affordance.**

- Click the `katezbuilds` cell → routes to the workspace.
- Style the workspace name as a link (no underline at rest, underline on hover) so the affordance reads.
- Optionally append a small `↗` glyph after the name (see [external-link-icon-placement-inconsistent](#external-link-icon-placement-inconsistent) for placement rules).
- The `⋯` menu shrinks to `Rename` + `Delete` — pure management.

Single reason: action menus should answer *"what can I do TO this thing"*, not *"where can I go FROM this thing."* Navigation is the row's primary affordance; management is the menu's.

---

## external-link-icon-placement-inconsistent

[Back to index](problems.md) · [Open screenshot](before/external-link-icon-placement-inconsistent.png)

**Verdict: confirmed. Same semantic, two positions, two different reading conventions.**

The product uses the `↗` glyph for "this navigates outside the current context" in at least two places, with opposite placement:

| Location | Pattern |
|---|---|
| Sidebar — `Payment methods ↗` | Trailing (icon after label) |
| Row menu — `↗ Go to workspace` | Leading (icon before label) |

The two placements teach the user different mental models for the same character:

- **Trailing** (after the label) reads as *"this destination is somewhere else."* It's the universal web convention — Stripe, Vercel, Linear, Notion, GitHub, MDN, every well-considered design system places external-link glyphs *trailing*.
- **Leading** (before the label) reads as *"this row's category is 'external thing.'"* It collides with the visual rhythm of every other menu item where the leading glyph is a category icon (pencil = rename, trash = delete). Now the user has to reparse: is this a leading category icon, or a leading destination icon?

The leading placement in the `⋯` menu also fights with the other rows in that same menu (`Rename` has a pencil leading; `Delete` has a trash leading — both *type-of-action* icons). Putting `↗` in the same slot makes it look like *type-of-action = "external"*, which is meaningless.

**Recommendation. Standardize: external-link `↗` is always trailing, never leading.** Single rule across every surface.

- `Payment methods ↗` — stays as-is.
- `Go to workspace ↗` — rewrite. The row menu's leading slot can carry a meaningful action icon (arrow-out box, share glyph, or just none — text alone is fine).

Pair this with the lift in [workspaces-row-menu-mixed-actions](#workspaces-row-menu-mixed-actions) — if `Go to workspace` leaves the menu and becomes a row-name link, this question dissolves on that surface, but the rule still needs to hold everywhere else.

Single reason: an icon's position teaches the user what kind of token it is. Leading icons are categories; trailing icons are modifiers/destinations. Mixing them on the same glyph trains the user to mistrust the icon vocabulary.

---

## sandboxes-toggle-misleading-when-empty

[Back to index](problems.md) · [Open screenshot](before/sandboxes-toggle-misleading-when-empty.png)

**Verdict: confirmed. The toggle creates a false signal that there's hidden content.**

The header reads literally: `0 sandboxes · 10 remaining in Tier 0 · Show terminated`. The user sees zero sandboxes AND a filter toggle that implies more might be revealed if flipped. Two possibilities both land badly:

- **User flips the toggle.** Nothing changes — still zero. The toggle was a dead end. User loses 5 seconds and a small dose of trust in the UI.
- **User doesn't flip it.** They wonder for the rest of the session whether they're missing some terminated sandboxes hidden behind the filter.

A filter that *cannot affect the visible set* is noise — and on an empty state it's specifically *misleading* noise.

This sits in the same family as [invoices-empty-state-banned-icon](#invoices-empty-state-banned-icon) and [invitations-empty-state-raw](#invitations-empty-state-raw): the product doesn't think systematically about empty-state surface composition. Each page is shipped with its full populated chrome regardless of whether that chrome makes sense at zero.

The operator's read on the Resources cards (bottom of the page) is correct — those are fine, they're contextually helpful for a brand-new user.

**Recommendation. Conditionally render the toggle.**

- Render the `Show terminated` toggle only when at least one sandbox in the workspace has been terminated at some point (i.e. `count(terminated) > 0`).
- In the all-zero state, the header line becomes a single piece of useful info: `0 sandboxes · 10 remaining in Tier 0`. Nothing else.

Generalize this into a rule across the product: **filters and toggles render only when the set they operate on is non-empty.** Status filters, role filters, source filters in [bulk-action-bar-too-far](#bulk-action-bar-too-far)'s team page should follow the same logic.

Single reason: a filter promises to change what you see. A filter that can't change anything is a broken promise.

---

## light-mode-primary-button-looks-disabled

[Back to index](problems.md) · [Open screenshot](before/light-mode-primary-button-looks-disabled.png)

**Verdict: confirmed. The button's state model is inverted, and light mode exposes it.**

Convention for a primary button:

| State | Treatment |
|---|---|
| Rest (default) | Solid brand color, full saturation |
| Hover | Slightly darker (~10% darken) |
| Active/pressed | Slightly darker still |
| Disabled | Neutral grey, low contrast text |

What this product does:

| State | Treatment |
|---|---|
| Rest | **Pale/washed orange — reads as disabled** |
| Hover | Solid orange — reads as enabled |

Rest and hover are swapped. The button is showing its hover-target color at rest, then "lighting up" only when the user moves the cursor over it. That makes every primary action look unavailable until you mouse over.

Dark mode hides the bug because dark backgrounds let even a translucent orange read as "loud enough." White light-mode backgrounds expose it — the orange has nothing to push against and the button collapses into the canvas.

This is the same family as:
- [toggle-no-loading-state](#toggle-no-loading-state) — off-state too muted, looks disabled.
- [checkbox-unchecked-looks-active](#checkbox-unchecked-looks-active) — unchecked uses the active color.
- [nav-active-state-confusing](#nav-active-state-confusing) — active state too weak to read.

The product has no consistent visual vocabulary for control states. Sometimes the "active" treatment lands on a passive state; sometimes the "passive" treatment lands on a default. Either way, users can't trust what the button is telling them.

**Recommendation.** Lock the primary button to convention:

- **Rest:** brand orange at 100% (`#E9601A` or whatever the token resolves to), white text. Same value across light and dark mode.
- **Hover:** `darken(brand, 10%)`, white text.
- **Active:** `darken(brand, 15%)`, white text.
- **Disabled:** neutral `bg-disabled` token, `text-muted` label. No orange at all.

Verify in both modes — the button should look fully saturated at rest in light mode (white background), and the same orange should also work against the dark canvas. If the token has to change between modes, that's fine — but the *relationship* between rest, hover, and disabled must stay convention-correct.

Single reason: the resting state is the default user perception of the button. Starting with a muted treatment makes every primary action look unavailable until the cursor proves otherwise.

---

## light-mode-coming-soon-badge-ugly

[Back to index](problems.md) · [Open screenshot](before/light-mode-coming-soon-badge-ugly.png)

**Verdict: confirmed. The badge has no light-mode variant — and the bigger problem is that it's the fourth variant of the same concept.**

This page now adds a new "incoming feature" label to the product's collection:

| Surface | Label | Treatment |
|---|---|---|
| Sidebar (Agents) | `SOON` | Tiny all-caps pill |
| Sidebar (Agent Drives) | `Preview` | Sentence-case pill |
| Add-ons cards | `Coming Soon` | Button-shaped, looks disabled (see [addons-coming-soon-looks-disabled](#addons-coming-soon-looks-disabled)) |
| Agent Runtime page | **`Coming soon!`** | Dark teal pill, sentence-case, **with exclamation mark** |

Four labels, four treatments, one concept. Each ships independently of the others.

Two specific failures on this instance:

1. **No light-mode variant.** A dark teal background sits on a white canvas — the badge wasn't audited across themes. It reads as "stuck in dark mode," a symptom of a token that hardcodes a color value instead of resolving against the theme.
2. **The `!` adds tonal noise.** Exclamation marks read marketing / consumer, not enterprise. Same tone problem as [whats-new-icon-consumer-vibe](#whats-new-icon-consumer-vibe) (sprout glyph) and [invite-dialog-naughty-animation](#invite-dialog-naughty-animation) (bouncy entrance). Status indicators on dev-infra products don't get punctuation; they get vocabulary.

**Recommendation: unify under the three-state vocabulary from [nav-incoming-feature-badges](#nav-incoming-feature-badges) and ship one badge component.**

Single `<StatusBadge variant="waitlist|preview|beta">` used everywhere. Theme-aware tokens:
- Light mode: `bg-amber-100 / text-amber-900` (subtle, integrates with white canvas).
- Dark mode: `bg-amber-950 / text-amber-100`.
- Same component, same vocabulary, same sentence case, no exclamation mark.

On this specific page: replace `Coming soon!` with `Waitlist` — same word as the page's own `Join the waitlist` CTA. The badge and the CTA now agree instead of contradicting each other.

Single reason: a label only carries meaning when it's used consistently. Four variants of "this isn't shipped yet" trains the user to ignore all four.

---

## theme-switcher-misplaced-in-page-menu

[Back to index](problems.md) · [Open screenshot](before/theme-switcher-misplaced-in-page-menu.png)

**Verdict: agreed — Theme belongs out of this menu.**

The `Display` dropdown bundles two different scopes inside one popover:

| Control | Scope | Persists across pages? |
|---|---|---|
| Theme (Light/Dark/System) | **Global app state** | Yes — affects every page |
| Chart settings (Data: RPS, Duration: 24h) | Page-local | This page only |
| Display Models (List: Card) | Page-local | This page only |

The user can't tell which knob will leak. Toggling RPS feels like it should change just the chart — and it does. Toggling Theme feels like it should also change just the chart — but it doesn't; it repaints the whole app. The menu makes a same-shape promise about two different scopes.

Two compounding problems:

1. **Duplication.** Theme already lives at `Settings → Preferences → Appearance` (seen in earlier audits). Now it lives in two places. If they're wired to the same state, one is wasted UI; if they aren't, the user has two competing sources of truth and no way to know which one wins.
2. **Discoverability inversion.** A globally-relevant control (theme) sits inside a page-specific menu (only on Model APIs), so users on other pages can't reach it from here. They have to go *into* Settings to find a switch that affects every other page.

Same scope-mixing family as [settings-name-page-mixed-ia](#settings-name-page-mixed-ia) — controls and content kept landing in containers whose label doesn't predict their contents.

**Recommendation. Move Theme out of the Display menu. Keep it in two places, both correct:**

1. **Settings → Preferences → Appearance** — canonical destination, stays as-is.
2. **User-profile menu in the top corner** — quick switcher (Light / Dark / System), available from every page. This is the Linear / Vercel / Stripe / GitHub pattern.

The Display menu then only carries page-local controls: Chart settings + Display Models. The container's label finally predicts its contents.

Single reason: a control's location should match its scope. Global controls live in global menus (user profile, top-level settings); page controls live in page menus. Mixing scopes makes every knob feel like it might leak.

---

## dashboard-route-shows-onboarding

[Back to index](problems.md) · [Open screenshot](before/dashboard-route-shows-onboarding.png)

**Verdict: confirmed. The route name promises a dashboard; the page delivers a getting-started guide.**

`Dashboard` carries a strong industry convention: a single surface showing the user the state of their system at a glance — KPIs, active resources, recent activity, alerts, quick links to common ops. Stripe, Vercel, Linear, Cloudflare, DataDog all anchor "Dashboard" to that meaning.

This page instead shows:
- 4-step install flow (`Install Blaxel CLI` → `Login locally` → `Install Blaxel SDK` → `Create a sandbox`)
- Resources cards (Documentation, SDK / API References)
- What's new changelog

That's an *onboarding* page. Useful on day 1, useless on day 2 — and yet `Dashboard` is the route users will return to expecting "what's happening in my workspace right now?"

Two compounding problems:

1. **No graduation.** The content doesn't change after the user has installed everything and created their first sandbox. The same install instructions stay forever. The user has no surface showing them their system state at any point in the relationship.
2. **Mental-model mismatch with related complaints.** The visual chaos called out in [post-signup-no-focus](#post-signup-no-focus) is rooted here — that audit treated the page as an onboarding surface (and concluded the right-rail Resources should be demoted). But the route is *also* the only thing called Dashboard. The two roles are fighting for the same canvas, and neither one wins.

**Recommendation. Make the route state-aware: same URL, different content based on `count(sandboxes)`.**

- **`count === 0` (new user):** Show the install flow. Title becomes `Get started`. No right-rail Resources panel; one primary CTA at the bottom of step 4 — `Create your first sandbox`. (This also lands [post-signup-no-focus](#post-signup-no-focus)'s recommendation.)
- **`count >= 1` (returning user):** Replace content with a real dashboard:
  - Active / total sandboxes, with status pills.
  - Credit balance card + tier indicator (deduplicates [sidebar-credit-card-eats-space](#sidebar-credit-card-eats-space) if that one gets collapsed).
  - Recent activity timeline (deploys, terminations, job runs).
  - Last 24h usage chart.
  - Quick actions row: `Create sandbox`, `New job`, `View invoices`.

The route name (`Dashboard`) stays. The page grows up alongside the user.

Single reason: a route's name is a promise about what's behind it. "Dashboard" promises a dashboard. Let the page graduate as the user does, instead of freezing them at day one.

---

## tier-color-overused-as-state

[Back to index](problems.md) · Screenshots: [credits](before/tier-color-credits-page.png) · [top-up](before/tier-color-topup-page.png) · [sidebar](before/tier-color-sidebar.png) · [sandboxes](before/tier-color-sandboxes-link.png) · [model APIs](before/tier-color-modelapis-link.png) · [custom domains](before/tier-color-customdomains-gate.png) · [policies](before/tier-color-policies-gate.png)

**Verdict: you're right. Tier is metadata, not state — and the product treats it like state in three contradictory ways.**

Inventory of where the tier label appears today:

| Surface | Label | Treatment | Semantic intended |
|---|---|---|---|
| Credits header | `Tier 0` | Green text on dark green tint | "you are here" |
| Top-up header | `Tier 0` | Same green | "you are here" |
| Sidebar credit card | `Tier 0` | Same green | "you are here" |
| Sandboxes quota line | `Tier 0` | Orange underlined link | "you are here, click to see tiers" |
| Model APIs quota line | `Tier 0` | Same orange link | Same |
| Custom domains page title | `Tier 3` | Green pale pill badge | **"you must be here"** — gate requirement, not current state |
| Policies page title | `Tier 1` | Green pale pill badge | Same — gate |

Three failures stack:

1. **Green ≠ tier.** Green carries universal UI semantics: *success / shipped / active / safe.* "You are on Tier 0" is the lowest, free tier — neutral at best, mildly negative at worst (you have the fewest features and are the upsell target). Painting it green tells the user *"you're in the good zone"* when the page's job is the opposite: explain what they're missing.
2. **Same color, two opposite meanings.** Green `Tier 0` in the Credits header means *current tier*. Green `Tier 3` on Custom domains means *required tier (you are NOT here)*. The user has no way to tell the two apart from styling — both look like positive status badges.
3. **Three treatments for one concept.** Green-tint header, orange link, green pill badge — the product can't decide what a tier label looks like.

Sibling of [tier-badge-no-guidance](#tier-badge-no-guidance) — that audit was specifically about the gated-page badge. This generalizes the rule.

How peer products handle plan / tier surfacing:

- Stripe: plan name in muted grey (`Free Plan` in `text-muted`), only billing-active state goes green.
- Linear: tier as small grey badge near workspace name.
- Vercel: plan shown as a subtle grey pill.
- GitHub: account plan in neutral text.
- Cloudflare: plan badge grey; only "active" status gets color.

Common rule: **tier is metadata → neutral color. State is state → color signals it.**

**Recommendation. Drop green from tier labels by default; reserve color for actual states.**

- **Current tier** (Credits / Top-up / sidebar / quota lines): `Tier 0` in `text-muted`, no background tint. Optionally a tiny dot indicator if you want to lightly emphasize "this is yours."
- **Required tier** on gated pages (Custom domains, Policies): use a locked / warning treatment — amber or grey pill with a lock glyph: `🔒 Requires Tier 3`. See [tier-badge-no-guidance](#tier-badge-no-guidance) for the hover/tooltip behavior.
- **Tier transition** (only): after a successful upgrade, the tier label can flash green for a few seconds as positive feedback, then settle back to neutral.

Ship ONE `<TierLabel>` component with `variant="current" | "required" | "transition"`, and remove all ad-hoc tier-styling from these seven surfaces.

Single reason: color signals state; metadata should be neutral. Painting tier metadata green either tells the user a lie (Tier 0 = good!) or gives the same word two opposite meanings depending on which page it's on. Neutral metadata + reserved color tokens fixes both.

---

## images-empty-state-neutral-undesigned

[Back to index](problems.md) · [Open screenshot](before/images-empty-state-neutral-undesigned.png)

**Verdict: your instinct is right. This page *should* feel neutral — but it's currently neutral by accident, not by design.**

The reasoning behind "neutral is correct here":

Images aren't created in isolation. An image is *forked from an existing sandbox* — the action starts on a sandbox detail page (or via CLI), not on this page. So this page is fundamentally a **directory + concept explainer**, not an activation surface. Pushing a `Create your first image` CTA here would be a false promise: the button would have nowhere to go without an existing sandbox first.

This is the opposite of [invitations-empty-state-raw](#invitations-empty-state-raw): on Invitations, the entire reason for the page is to *invite a teammate*, so a missing CTA is a design hole. On Images, a missing CTA is correct — but the page still needs to do three jobs that it currently fumbles:

1. **Explain what an Image is.** The current line — *"Fork off sandboxes from an image"* — is opaque to a first-visit user. It doesn't say what an image is, only what you do with one. Compare to "An image is a saved snapshot of a configured sandbox you can spawn new instances from." Same idea, lands without prior knowledge.
2. **Point to where the action lives.** A neutral page should still tell the user *"if you want to create one, here's where."* Right now there's no such pointer. Add a secondary link: `Create an image from any sandbox →` linking to the Sandboxes list.
3. **Offer a learn-more, not just an API reference.** The single `API Reference` Resources card is for builders who already know what an image is. Most users on this empty page don't. Add a `Documentation · What are Images?` card alongside.

Plus three smaller items:

- **Typo: `tospawn`** — missing space (`to spawn`).
- **`Fork off`** — casual / playful idiom that's tonally off for enterprise dev infra. Same family as [whats-new-icon-consumer-vibe](#whats-new-icon-consumer-vibe) and [invite-dialog-naughty-animation](#invite-dialog-naughty-animation). Replace with `Reusable sandbox snapshots.`
- **Generalize the empty-state component.** Pair with [invoices-empty-state-banned-icon](#invoices-empty-state-banned-icon) and [invitations-empty-state-raw](#invitations-empty-state-raw): the product needs one `<EmptyState>` with optional `primaryAction` and optional `secondaryLink`. Images uses `secondaryLink="Create from a sandbox →"` and no primary; Invitations uses `primaryAction="Invite a teammate"`.

**Recommendation: keep it action-free, but design the neutral state.**

```
        [faded cube icon]

      Reusable sandbox snapshots
   Save a configured sandbox as an
  image, then spawn fresh instances
              from it.

  Create one from any sandbox →
        (subtle text link)

  [ Documentation card ]  [ API Reference card ]
```

No primary button. One secondary text link that bridges the user to where the action actually starts. Two Resources cards for both the learner and the builder.

Single reason: neutrality is a design decision, not the absence of one. A page with no CTA should still explain itself, signal where to go next, and be deliberate about its tone — otherwise it reads as half-built rather than calm.

---

## pagination-shown-with-one-item

[Back to index](problems.md) · [Open screenshot](before/pagination-shown-with-one-item.png)

**Verdict: confirmed. The pagination block is dead chrome.**

With one model API, the controls render fully — `Rows per page · 10`, `Page 1 of 1`, ‹‹ ‹ › ›› — and every interactive element is in a disabled state. Nothing is reachable, nothing is changeable, nothing is gained. But the user has to *read* the chrome before realizing it's inert.

Two consequences:

1. **False signal of "more pages exist."** The presence of pagination arrows — even greyed out — primes the user to look for a way to navigate further. The header already says `1 model API`; the pagination block contradicts that with its visual implication.
2. **Wasted attention budget.** A full row of disabled chrome above the Resources section eats vertical space and visual weight for zero information.

Same family as [sandboxes-toggle-misleading-when-empty](#sandboxes-toggle-misleading-when-empty) — the product ships filter/pagination/toggle UI regardless of whether the underlying data justifies it.

**Recommendation: render pagination only when `count > rowsPerPage`.**

- `count <= rowsPerPage` → hide the pagination row entirely. The header count (`1 model API`) already tells the user everything.
- `count > rowsPerPage` → render the full block, including a working page selector.
- Optional: keep `Rows per page` visible if you want users to lower the page size to force pagination — but only above some threshold (e.g. ≥ 5 items).

Apply the same rule to every table in the product (team, service accounts, integrations, invoices history, etc.). The pagination component should self-render-or-hide based on data, not always.

Single reason: chrome that does nothing is noise. Disabled controls are a false signal — they imply the action exists somewhere; it just doesn't.






























