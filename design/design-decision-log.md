# Design Decision Log

The running record of every **major product-design and UX decision** for Stacks — the counterpart to `decisions/decision-log.md`, which covers product/engineering/circulation. This log exists because most consequential design calls get made in conversation and would otherwise vanish. **If a design decision was made and isn't written here, it didn't happen.** Append to it as you work.

Newest entries at the top of each section. Status key: **Accepted** (settled), **Recommended** (leading option, not yet confirmed), **Exploring** (actively open), **Deferred** (deliberately postponed).

Related: house heuristics in [`design-principles.md`](design-principles.md) · review checklist in [`ux-heuristics.md`](ux-heuristics.md) · visual system in [`../DESIGN.md`](../DESIGN.md) · strategy in [`../PRODUCT.md`](../PRODUCT.md).

---

## How to add an entry

Copy the template below into the right section (Foundations / Cataloging / Circulation / Connections & Browse / Visual system / Cross-cutting). Keep entries short and reasoned — the *why* and the *what we rejected* are the whole point.

```
### <Short decision title>
**Status:** Accepted | Recommended | Exploring | Deferred · **Date:** YYYY-MM-DD · **Surface:** <screen/flow/component>
**Decision.** One or two sentences: what we're doing.
**Why.** The reasoning. Tie back to a principle in design-principles.md or a product decision where relevant.
**Rejected / considered.** The alternatives and why they lost.
**Affects.** Screens, components, or docs this touches. Note any follow-up.
```

When a decision reverses or supersedes an earlier one, don't delete the old entry — set its status to superseded and link to the new one, so the reasoning trail survives.

---

## Foundations & information architecture

### Full-app IA scaffold: three tabs, global tools, notification center as the action surface
**Status:** Accepted · **Date:** 2026-08-08 · **Surface:** global navigation, whole-app structure · **Supersedes:** "Full-app IA scaffold: four tabs, global Add, Requests as the action center" (below)
**Decision.** Primary nav is a **three-tab bar — Library · Browse · Profile** — governed by one rule: *the bottom bar holds destinations, not actions.* **Library is home** (default landing; My Shelf of owned copies + a Borrowed segment for copies held from neighbors). Add/Scan, Search, and Notifications are **global tools available everywhere, not tabs.** The former Requests tab is dissolved: its *events* (incoming borrow request, return to confirm, connection request, approval, overdue) move to a **notification center behind a header bell** that deep-links into the action screen; its *standing lists* move into Library — "On loan" is a filter on My Shelf (lent copies are still yours), "Borrowed" is a Library segment. Search is **contextual** (my shelf in Library, neighbors in Browse) with a later unified global search returning results grouped by scope. Screen-ID scheme is O/L/B/C/P/N/S.
**Why.** Destinations-not-actions keeps the thumb bar small and stable and stops it filling with tools (Add, Search, Notifications are things you *do*, not places). Library-as-home delivers the catalog payoff with no extra hop and no empty-at-cold-start dashboard (Principle 1; avoids the social-feed / institutional-dashboard anti-references). The old Requests tab conflated *events that need a tap* with *standing lists you return to*; splitting them — events to the bell, lists to Library — gives convergence *and* omnipresence for events, and a durable home for loan/borrow state. "Books on loan" gains an obvious home (My Shelf filter) and "Borrowed" stops being homeless.
**Rejected / considered.** A four-tab bar with Requests as the action center (superseded entry) — rejected: Requests did two unlike jobs and cost a permanent slot. A dedicated Home/dashboard tab — rejected: empty at cold-start, delays catalog payoff, drifts to anti-reference feel. A Search tab — rejected: search is an action, and a search tab overlaps Browse's discovery job. A separate top-level "Borrowed" or "Loans" tab — rejected: folds cleanly into Library as segment + filter.
**Scope note.** The POC notification center is **in-app and pull-based only** (no push, no overdue automation), consistent with the settled "in-app badges only for the POC" rule. Push infra, overdue nudges, and messaging remain Post-POC (feature area F).
**Affects.** Every screen; `product/information-architecture.md` (rewritten, authoritative); `product/journeys-and-screens.md` tab-bar line updated. Screen IDs (L4, C3, N1, …) are the reference vocabulary going forward.

### Full-app IA scaffold: four tabs, global Add, Requests as the action center
**Status:** Superseded (2026-08-08) → see "three tabs, global tools, notification center" above · **Date:** 2026-08-07 · **Surface:** global navigation, whole-app structure
**Decision.** Adopt a single master IA (`product/information-architecture.md`) covering all feature areas, not just the POC. Primary nav is the four-tab bar (Library · Browse · Requests · Profile); **Add/Scan is a global affordance, not a tab**; **Requests is the cross-role action center** where every pending decision for both roles converges. Notifications/Activity is deliberately *not* a fifth tab — badges carry it in the POC, and a header bell surfaces it post-POC. Screens are numbered by area (O/L/B/R/P/S) for stable reference in future design and PRD work.
**Why.** One primary job per screen and a thumb-reachable bar (design-principles: cognitive load, hierarchy). Making "add a book" global reflects it being the highest-frequency action (Principle 2) without spending a tab slot. Converging both roles' pending actions in Requests means a user never hunts across tabs to answer "what needs me now" (Principle 4, and the loop is where value is felt). Reserving structural slots for post-POC areas (AI shelf scan L12, pods/map B6, notifications center S2) means they attach at defined hooks with no re-architecture.
**Why superseded.** The Requests tab conflated events with standing lists, and cost a permanent slot for a job the bell + Library filters do better. See the superseding entry for the full argument.
**Affects.** Superseded by the three-tab model above.

### App shell: one experience with modes, not two apps
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** global navigation
**Decision.** A single app shell serves both Steward and Borrower roles; primary nav is Library · Browse · Requests · Profile. Most people are both roles, so we do not split into two experiences.
**Why.** Principle 1 and the personas in PRODUCT.md — most users lend *and* borrow. Two apps would double the surface and force a false identity choice.
**Rejected / considered.** Separate steward/borrower modes or apps — rejected as artificial and heavier.
**Affects.** Every screen. Tab bar defined in `product/journeys-and-screens.md`.

*(Seeded from the existing product docs. Add new foundations decisions above this line.)*

---

## Cataloging (the core magic)

### Custom inset scanner with a live result panel, not a full-bleed camera
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** L4 barcode scanner
**Decision.** The scanner is not full-bleed camera; the camera view is inset, paired with a live result panel showing the last-scanned item plus a running count, and Continue / Add / Review actions.
**Why.** Gives the batch-scan flow a visible sense of progress and control beyond the running counter alone (Principle 2), and creates a natural checkpoint (Review) feeding directly into the existing "never add silently" scan-review step.
**Rejected / considered.** Full-bleed camera with overlay chrome — rejected in favor of the inset+panel layout, which surfaces the last scan without a modal interruption.
**Affects.** Component spec §H (scanner); scan review & confirm (L5) should be reachable directly from the panel's Review action, not only after a batch ends.

### Haptics: soft success on scan, distinct double-buzz on error
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** L4 scanner; pattern available to other confirm/error moments
**Decision.** A soft, light haptic fires on each successful scan hit. A harsher double-buzz fires on a failed/ambiguous scan.
**Why.** Reinforces "instant feedback" (Principle 2) through a native, near-free channel; a distinct error pattern lets a user register a miss without looking at the screen.
**Affects.** Scanner surface. Reusing the same success/error haptic vocabulary for other confirm actions (approve, confirm return) is a candidate for a later pass, not decided yet.

### Prototype the cataloging flow first
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** scanner, scan review, My Library
**Decision.** Design and prototype cataloging (scanner → batch review → My Library) before the circulation loop or connections.
**Why.** Highest-risk, highest-magic interaction; batch-scan feel decides whether onboarding completes (Principle 2). If it isn't fast and satisfying, nothing downstream matters.
**Rejected / considered.** Starting with the lending loop (the mission) — rejected because catalog value is the wedge and the retention hook.
**Affects.** Prototype sequence in `product/journeys-and-screens.md §5`.

### Never add scanned books silently
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** scan review and confirm
**Decision.** Batch scans always feed an editable review-and-confirm list; nothing enters the library without confirmation. Same rule applies to AI shelf scan later.
**Why.** Scan/vision results are imperfect (worn barcodes, misread spines). Silent adds erode trust in the catalog — the one thing that must feel reliable.
**Affects.** Scan review screen; future AI-shelf-scan flow.

---

## Circulation (the lending loop)

<!-- The lending mechanics are decided in decisions/decision-log.md and circulation/copy-state-model.md.
     This section is for their DESIGN expression: how states look and how the actions feel. -->

### Two-step handoff: approve reserves, "handed over" starts the loan
**Status:** Accepted · **Date:** 2026-08-09 · **Surface:** owner book detail (requested/ready states), approve flow, borrower Borrowed view
**Decision.** Approving a request surfaces a *Ready for pickup* state on the book card, with a single "Mark as handed over" action that opens the due-date sheet and starts the loan. The due-date choice ("Due in 2 weeks / Custom date… / No due date") lives on the **handover** step, not approval, since the clock starts at handoff. The requester sees "Approved · arrange pickup" in Library › Borrowed; a "Requests & pickups" group holds both pending and ready items.
**Why.** Principle 3 (legible state) and Principle 4 (protect the relationship) — collapsing approve and handoff made due dates dishonest and left the borrower unsure they were approved. Two states model the two real moments without building any in-app scheduling/chat (out of POC scope).
**Rejected / considered.** One-step approve = checked out (simpler, but dishonest due dates and no approval signal). Setting the due date at approval (clock would start too early).
**Affects.** `circulation/copy-state-model.md` (new Ready state, grid row, transitions); owner detail CTA; borrower Borrowed rows; the approval sheet moved to handover. Supersedes the one-step behavior in `decisions/decision-log.md` (Circulation).

### Direct lend + off-app borrower: a "Lend" action and a free-text borrower
**Status:** Accepted · **Date:** 2026-08-09 · **Surface:** owner book detail (available state), Lend sheet
**Decision.** Available copies get a primary "Lend to someone" action opening a borrower picker: connections listed, plus a "Someone not on Stacks" free-text name field. Choosing a borrower flows into the due-date sheet, then checked out. Off-app loans render a "not on Stacks" tag and only the owner-driven "Mark returned directly" (no borrower-side return; no sim-return affordance).
**Why.** The Steward's real lending is often proactive and to people not yet on the app; the catalog's tracking value (Principle 1, the wedge) must not be gated behind the borrower having an account. One safe default over configuration (Principle 5): borrower picker defaults to connections, off-app is one tap away.
**Rejected / considered.** Restricting checkout to request-driven, on-app borrowers only — rejected as too narrow for cold-start and everyday lending.
**Affects.** Owner detail available-state CTA; new Lend sheet; `copy-state-model.md` (direct-lend on-ramp, off-app rules); `product/information-architecture.md` (C-series actions).

### Borrow history shows date and borrower only, no on-time/late nuance
**Status:** Accepted · **Date:** 2026-08-07 · **Surface:** owner book detail, borrow history section
**Decision.** The borrow history list on the owner's book detail screen shows each past loan as borrower + borrowed/returned dates only. No on-time/late signal or overdue flag on history rows.
**Why.** Keeps the section a quiet look-back rather than a judgment surface. Matches Principle 4, social friction removed — flagging past lateness reads as shaming a neighbor after the fact, which the state model already avoids for the live overdue state (shown, not nudged).
**Rejected / considered.** Checkmark for on-time vs. flag for late return — rejected as unnecessary nuance that risks a punitive tone on a list neighbors' names appear in.
**Affects.** Owner book detail screen; borrow history component.

### Copy state drives every book card
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** every book card
**Decision.** Every book card renders one of six copy states (available, requested, checked out, return pending, overdue, unavailable). The state-by-role grid in `circulation/copy-state-model.md` is the design source of truth, including role-specific labels ("Request pending" vs "On hold").
**Why.** Principle 3 — lending state must be legible at a glance. Centralizing on the state model keeps cards consistent and prevents ad-hoc states.
**Affects.** Book card component (see `DESIGN.md` Components); owner/borrower/neighbor detail screens.

### State must never be signaled by color alone
**Status:** Accepted · **Date:** 2026-08-03 · **Surface:** state badges on all cards
**Decision.** Each lending state pairs its color (amber/blue/red/neutral) with a text label and/or icon.
**Why.** WCAG AA and the state model's heavy reliance on colored badges; color-blind and low-vision users must distinguish states. Recorded in PRODUCT.md accessibility and DESIGN.md.
**Affects.** All state badges; the semantic color roles in `DESIGN.md`.

### Decline screen forks into a next-state choice
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** owner decline action
**Decision.** Declining a request is one action that then asks the owner to send the copy to *available* or *unavailable*. No reason is captured; the borrower only sees "declined."
**Why.** Mirrors the settled circulation rule (`copy-state-model.md`) and Principle 4 — declining stays low-friction and non-punitive, no awkward reason field.
**Affects.** Decline flow; requires the next-state choice built into the screen, not a separate step.

---

## Connections & browse

*(No design-specific decisions logged yet. The connective tissue is designed after cataloging and circulation per the prototype sequence.)*

---

## Visual system

### Motion: unique page transitions are a priority, not just functional feedback
**Status:** Exploring · **Date:** 2026-08-10 · **Surface:** whole app, motion system; especially book card → detail
**Decision.** Beyond the already-settled reduced-motion requirement for scanner feedback, invest in distinctive page transitions — specifically, opening a book card into its detail screen should feel considered, not a generic push.
**Why.** Cheap way to make browsing/cataloging feel "a little magical" (Principle 2) without visual noise; card-to-detail is the single most-repeated navigation in the app, so it's high-leverage.
**Affects.** Needs a follow-up motion-design pass (likely a shared-element/hero transition on the cover) once the book card's visual treatment is locked. Concrete duration/easing still open.

### Iconography: SF Symbols
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, icon system
**Decision.** Use SF Symbols throughout rather than a custom icon set, for now.
**Why.** Free Dynamic Type scaling and weight-matching with SF Pro; fastest path for a POC; avoids committing to a custom icon language before the rest of the visual system is proven.
**Rejected / considered.** Custom icon set matching the paper/neighborly personality — deferred, not rejected; revisit post-launch.
**Affects.** Every state-badge icon, tab bar glyphs, header tool icons.

### 8pt spacing grid, 4px minimum; even-numbered type scale
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, spacing & type tokens
**Decision.** All spacing values are multiples of 8px, with 4px as the smallest allowed increment (hairline gaps only). Type sizes follow the same even-numbered discipline — no odd point sizes anywhere in the scale.
**Why.** A consistent grid is an affordance in product UI — predictable rhythm speeds recognition; even numbers keep spacing and type coherent together.
**Affects.** `DESIGN.md` spacing tokens (4/8/16/24/32/40/48 scale). Concrete type-scale step values still open.

### Elevation: flat/tonal content, lifted book covers
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, elevation system
**Decision.** Most surfaces (card chrome, sheets, list rows) stay flat or tonal. Book covers specifically get a light shadow/lift, so they read as physical objects rather than flat images.
**Why.** Reinforces the book card as the app's central object without adding visual weight everywhere, which would fight the calm/effortless brand personality.
**Affects.** Book-card cover treatment only — not badges, buttons, or sheets.

### Typography: SF Pro for UI, New York (serif) for display/headings
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, type system
**Decision.** SF Pro carries the vast majority of the UI — labels, body, buttons, data. New York (serif) is reserved for headings/display type, an editorial, book-ish accent without sacrificing native legibility elsewhere.
**Why.** System fonts are legitimate and get Dynamic Type support for free; isolating the serif accent to display moments keeps risk low while still nodding to the book/paper personality.
**Rejected / considered.** A single family throughout — too plain for the brand personality. A fully custom typeface — unnecessary cost/risk for a POC.
**Affects.** `DESIGN.md` typography tokens; onboarding's brand-register moment is the natural home for New York at largest scale.

### Light and dark mode, system-driven
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, theme
**Decision.** Support both light and dark mode, following the system setting by default with a user override available in settings.
**Why.** Matches the real usage scene in PRODUCT.md's accessibility section — standing at a bookshelf in a dim room or a sunny window. A single theme fails one of those conditions.
**Affects.** Every color token needs a light and dark value; contrast must be verified in both.

### Warm, paper-toned color direction (not sepia); extends to semantic state colors
**Status:** Recommended · **Date:** 2026-08-10 · **Surface:** whole app, color tokens
**Decision.** Base palette leans warm — warm grays/near-blacks and a paper-adjacent neutral background — evoking books and paper without going sepia/nostalgic. The same warm undertone carries through to the six semantic lending-state colors, not just neutrals.
**Why.** Reinforces brand personality at the token level, not just in copy; avoids the dated "vintage library" cliché of true sepia.
**Rejected / considered.** True sepia/vintage palette — too nostalgic/dated. Cool neutral grays — reads corporate/cold, against the anti-references.
**Affects.** `DESIGN.md` colors section — exact OKLCH values still deferred to a follow-up color session. Every semantic state badge inherits this direction.

---

## Cross-cutting

### Voice and tone ruleset, with a hard ban on em dashes and trailing qualifier clauses
**Status:** Exploring (draft) · **Date:** 2026-08-13 · **Surface:** every string in the product
**Decision.** `design/voice-and-tone.md` is the owning doc for UX writing. It sets the voice ("text a neighbor you like but don't know well") and a set of mechanical sentence rules, the sharpest being: no em dashes anywhere; no trailing qualifier clause after a comma (the "Devi approves or not, no obligation." shape); no "not X, but Y" antithesis; no adjective triplets; no exclamation marks. It also bans a vocabulary list (seamless, effortless, simply, unlock, curate, delight, magical) and a set of moves (flattering the user, explaining the user's feelings to them, cutesy error voice, vague privacy reassurance). Copy is graded against a read-aloud smell test rather than a word count.
**Why.** PRODUCT.md already asks for plain, human, low-pressure copy, and `design-principles.md` covers UX writing in one paragraph, which is not enough to hold a line against generated-sounding text. The specific failure mode is syntactic: comma-spliced fragments and dash-shaped asides read as machine-written even when the vocabulary is fine. Banning the sentence shapes catches what a vocabulary list misses. Ties to Principle 4 (remove social friction) because most of the offending copy appears in the request, decline, nudge, and overdue moments, where over-explaining a feeling is what makes the interaction awkward.
**Rejected / considered.** Leaving tone as a paragraph inside `design-principles.md` — rejected, too thin to enforce and easy to skim past. A tone-of-voice spectrum matrix (formal/casual sliders) — rejected as unactionable at string level. Allowing em dashes in long-form marketing copy only — deferred, and the ban currently holds everywhere for simplicity.
**Affects.** All UI copy; `design/ux-heuristics.md` §5 (now a summary of this doc); `design/component-spec.md` §3.C badge and role-label microcopy pass; `CLAUDE.md` knowledge map and design workflow. Open calls parked in §9 of the new doc: system "we", onboarding brand-register latitude, push notification copy, off-app loan phrasing.

### Sign in with Apple and native share sheet confirmed as system components
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** O2 auth, P2 invite
**Decision.** Both use their standard system-provided appearance; not reskinned to match the app's visual system.
**Why.** Apple HIG requires the system button style for Sign in with Apple; the native share sheet is the expected, trusted surface for sharing an invite link.
**Affects.** No custom design work needed for either; flagged so they aren't accidentally redesigned later.

### Pull-to-refresh everywhere it's pull-based
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** Library, Browse, Notification center
**Decision.** Pull-to-refresh is adopted as the refresh pattern on all three pull-based surfaces.
**Why.** Consistent with the settled "in-app, pull-based, no push" POC scope — pull-to-refresh is the expected native complement to a pull-only data model.
**Affects.** L1/L11 (Library), B1/B2 (Browse), N1 (Notification center).

### Long-press context menu replaces swipe actions
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** My Shelf, Borrowed, and any list/grid row with quick actions
**Decision.** Row/card-level actions (mark unavailable, lend, etc.) are reached via a press-and-hold context menu, not swipe gestures.
**Why.** My Shelf supports both list and grid/cover layouts, and swipe doesn't work uniformly across both — a context menu works identically in either.
**Rejected / considered.** Swipe-to-act — would need a different, inconsistent affordance in grid view.
**Affects.** Book-card component (§B) gains a long-press state; removes "swipe actions" as an open question.

### Dynamic Type: full standard range everywhere, accessibility range capped on dense components
**Status:** Recommended · **Date:** 2026-08-10 · **Surface:** whole app, type system
**Decision.** Support the full standard Dynamic Type range (xSmall–xxxLarge) on every screen. For the accessibility range (AX1–AX5), cap dense components — tab bar labels, state badges, book-card grid metadata, filter chips — at AX1 (Accessibility Medium); text-heavy single-column screens (book detail, forms, settings, borrow history) support the full range through AX5.
**Why.** Matches the common pattern for dense grid/list apps: full accessibility support everywhere sounds right in principle but breaks the book-card grid (cover + title + author + badge) well before AX5. Capping only the dense surfaces keeps the app inclusive where it matters most without forcing a from-scratch reflow of the grid.
**Rejected / considered.** Full AX1–AX5 support everywhere — impractical for the grid view without a redesign. Capping the whole app at the standard range only — insufficiently accessible for text-heavy screens.
**Affects.** Book card, state badge, tab bar, chips need a defined "capped" fallback layout at AX1 (e.g., icon-only badge, or list-view fallback beyond that point).

### No iPad support for the POC
**Status:** Accepted · **Date:** 2026-08-10 · **Surface:** whole app, device scope
**Decision.** iPhone only; no iPad-specific layout for the POC.
**Why.** Matches the already-accepted iOS-first, POC-scope discipline — extending to iPad adds a second layout system before the core loop is proven.
**Affects.** Components can assume a single (compact-width) size class; no adaptive/regular-width variants needed yet.

### Ship one safe sharing default for the POC
**Status:** Accepted · **Date:** 2026-08-02 · **Surface:** sharing & visibility
**Decision.** The POC ships a single visibility default — "visible to connections" — and stubs the granular sharing UI.
**Why.** Principle 5 — one safe default over a wall of choices. Complex permission UI scares people off and isn't needed to prove the loop.
**Rejected / considered.** Per-library / per-copy visibility controls at launch — deferred (see `decisions/open-questions.md`, "Sharing granularity").
**Affects.** Sharing settings screen (fast-follow); onboarding.
