# Component & Design System Spec — iOS App

Pre-build inventory of every component the POC and near-term roadmap require, plus every design question that has to be answered before or during visual design. This is the input to the design-system phase called for in `CLAUDE.md`: build this once, then design each component against it via **Impeccable**, logging every visual decision in `design-decision-log.md` as it's made.

Synthesized from `PRODUCT.md`, `DESIGN.md`, `design/design-principles.md`, `design/ux-heuristics.md`, `circulation/copy-state-model.md`, `product/information-architecture.md`, `product/journeys-and-screens.md`, `decisions/decision-log.md`, `decisions/open-questions.md`, Impeccable's product-register reference, and the foundations/platform round decided 2026-08-10 (`design/design-decision-log.md`, "Visual system" + "Cross-cutting" + "Cataloging").

**Status of the visual system today:** foundational direction is settled — color strategy, theme, typography, elevation, spacing discipline, icon system, and the platform-level interaction model (Dynamic Type, dark mode, haptics, scanner shape, context menus, pull-to-refresh) are all decided. What's still open is exact numeric/hex values and a handful of component-level and content questions, listed in §5.

---

## 1. Foundations — settled direction, values still open

| Foundation | Decision | Still open |
|---|---|---|
| **Color strategy** | Achromatic chassis — no brand accent. Warm near-black ink on barely-warm near-white (hue 72–78°, chroma 0.003). Color appears in three momentary roles only: four `status/*` hues, per-book `background/cover-tint` (detail header only), and `feedback/scan-hit`. | Exact OKLCH values locked in `DESIGN.md` §Colors, each light + dark, each WCAG AA-verified against its paired label/icon. **Figma library and `tokens/colors.css` still need a resync to these values.** |
| **Theme** | Light and dark mode, system-driven with a user override in settings. | Nothing structural — just needs every color token duplicated for dark. |
| **Typography** | SF Pro for nearly everything (body, labels, buttons, data). New York (serif) reserved for display/heading type only. Type scale is even-numbered, tighter product-UI ratio (~1.125–1.2). | Concrete step values for the type scale; whether New York appears anywhere outside onboarding/headers. |
| **Elevation** | Mostly flat/tonal (card chrome, sheets, rows). Book covers specifically get a light shadow/lift for physical dimension. | Exact shadow values (blur/spread/opacity) once the book card is crafted. |
| **Spacing & shape** | 8pt grid, 4px minimum increment. Full scale: 4/8/16/24/32/40/48. | Corner-radius scale — not yet specified; should probably follow the same even-number discipline but needs an explicit call. |
| **Iconography** | SF Symbols throughout, for now. | Nothing open — revisit a custom icon set post-launch if warranted. |
| **Motion** | Priority, not an afterthought — distinctive page transitions, specifically book card → detail. | Concrete transition spec (likely a shared-element/hero treatment on the cover), duration/easing values, and how far the "unique transitions" idea extends beyond card → detail (e.g., sheet presentation, tab switches). |

---

## 2. iOS-platform decisions

| Question | Decision |
|---|---|
| Device support | iPhone only for the POC — no iPad layout. |
| Dynamic Type | Full standard range (xSmall–xxxLarge) everywhere. Accessibility range (AX1–AX5) capped at **AX1** on dense components — tab bar labels, state badges, book-card grid metadata, filter chips. Text-heavy single-column screens (book detail, forms, settings, borrow history) support the full range through AX5. |
| Dark mode | Yes, system-driven with an override (see §1). |
| Haptics | Soft, light success haptic on each scan hit. Distinct harsher double-buzz on a failed/ambiguous scan. Reusing this vocabulary for other confirm/error moments (approve, confirm return) is a candidate for later, not decided. |
| Scanner camera surface | Not full-bleed. Inset live camera view paired with a live result panel: last-scanned item, running count, Continue / Add / Review actions (see §3.H — this is a significant redesign from the original full-bleed assumption). |
| Row/card quick actions | Long-press context menu, not swipe — chosen because My Shelf supports both list and grid/cover layouts, and swipe doesn't work uniformly across both. |
| Pull-to-refresh | Yes, on all three pull-based surfaces: Library, Browse, Notification center. |
| Sign in with Apple | System-provided button style, per Apple HIG — not reskinned. |
| Native share sheet | Used as-is for Invite — not reskinned. |

---

## 3. Component inventory

Grouped by function. Each entry names the component and, where the copy-state model, IA doc, or the 2026-08-10 decisions already constrain it, what that constraint is. Screen IDs (L#, B#, C#, N#, P#, S#) reference `product/information-architecture.md`.

### A. Shell & navigation
1. **Tab bar** — three destinations (Library · Browse · Profile), Library is home/default-selected. States: default, selected.
2. **Header / nav bar** — per-screen title plus the three global tools (Add, Search, Notification bell). Needs a rule for how title + 3 tool icons fit without crowding on the smallest supported width.
3. **Notification bell + badge counter** — global, every screen. Needs a rule for badge count formatting (1, 2, "9+"?).
4. **Segmented control** — My Shelf / Borrowed toggle at the top of Library.
5. **Filter chip / chip group** — status filter on My Shelf (Available / On loan / Unavailable, possibly All).
6. **Grid/list view toggle** — My Shelf and Browse.
7. **Back navigation** — standard nav-bar back, but confirm whether it's system default or custom.
8. **Pull-to-refresh control** — Library, Browse, notification center (Accepted, §2).

### B. The book card — the primary object
The single most-reused component in the app (already an Accepted decision: "Copy state drives every book card"). Appears in L1, L10, L11, B2, B3, and N1 rows — same vocabulary, different role/actions per screen.

- **Two layouts:** grid cell (My Shelf grid, Browse grid) and list row (list view, Borrowed, notification deep-links).
- **Sub-elements:** cover image (lifted-shadow treatment, per Elevation §1 + no-cover-art fallback for resolver misses), title, author, state badge, a secondary metadata line that changes by state (due date, holder name, "not on Stacks" flag).
- **Inputs the card must take:** copy state (one of 6 + derived overdue) **and** viewer role (owner / requester / third-party neighbor) — the same state renders different labels per role (see §3.C).
- **Interaction:** tap opens detail (L10 or B3). **Long-press opens a context menu** with the card's available quick actions (Accepted, §2) — replaces the earlier open question about swipe actions.
- **Transition:** tap-to-detail is the priority target for the unique page-transition work (§1 Motion) — likely a shared-element treatment on the cover.

### C. State badge
The highest-stakes component to get right — it's the whole legibility promise (Principle 3) and the accessibility-critical one (never color alone). It isn't six variants, it's closer to eleven once role is factored in. Full enumeration, pulled directly from the state-by-role grid in `circulation/copy-state-model.md`:

| State | Owner-facing label | Other-role label | Badge color token |
|---|---|---|---|
| Available | "Available" | — | `status/positive` |
| Requested | "Requested by [name]" | Requester: "Request pending" · Others: "On hold" | `status/attention` |
| Ready for pickup | "Ready for pickup — reserved for [name]" | Requester: "Approved · arrange pickup" · Others: "On hold" | `status/attention` |
| Checked out | "Checked out to [name]" + due date / no due date / "not on Stacks" flag | "Checked out" (+ return-by date if set) | `status/neutral` |
| Return pending | "[name] marked this returned. Confirm?" | Borrower: "Return submitted, awaiting confirmation" | `status/attention` |
| Overdue | "Overdue, was due [date], with [name]" | Borrower: overdue flag | `status/overdue` |
| Unavailable | "Not available for lending" | "Owner has it, not available for lending" | `status/neutral` |

Seven states, four hues (per the 2026-08-16 achromatic-chassis decision). Hue encodes *does this need me*; the SF Symbol and label encode *which state*. Because three states now share `status/attention` and two share `status/neutral`, **icon selection is no longer cosmetic** — it is the only thing distinguishing them at a glance, which raises the stakes on the open symbol-picking task in §5.

Icon system is settled (SF Symbols), so each badge can now be paired with a real symbol rather than a placeholder — that pairing is still an open task (§5). Badge needs a compact mode (grid cell) and a full mode (list row / detail header), and — new, from the Dynamic Type decision — a **capped fallback layout at AX1** for the grid/compact mode specifically, since dense components don't scale past AX1 (§2).

### D. Buttons & actions
- **Primary (filled)** — Request to borrow, Approve, Mark as handed over, Confirm returned, Lend.
- **Secondary (outline/tonal)** — Decline, Cancel request, Didn't receive it.
- **Careful/low-alarm action** — Mark unavailable. Still open: this must *not* read as destructive-red given the non-punitive principle (Principle 4) — needs its own treatment distinct from a true destructive action.
- **Icon button** — the three header tools (Add, Search, Bell), all SF Symbols.
- **Text/link-style button** — fallback affordances like "Add manually" under a failed scan/search.
- **Global Add affordance** — still open: floating action button vs. a header-embedded icon. Not resolved in the 2026-08-10 round.

### E. Forms & inputs
- **Text field** — profile name, manual title/author search.
- **Search field** — scoped placeholder copy ("Search my shelf" in Library vs. "Search neighbors" in Browse).
- **Toggle/switch** — sharing/visibility default (fast-follow), settings, and the new light/dark override control.
- **Bottom-sheet picker** — due-date selection ("Due in 2 weeks / Custom date… / No due date" — no-due-date must be a first-class, equally-weighted option, not a buried link).
- **Free-text name field** — the off-app borrower entry inside the Lend sheet.
- **Location / condition fields** — on Book detail: edit-your-copy (L8).
- **Photo picker** — optional profile photo (O3), and later cover-photo corrections.

### F. Lists & rows
- **Generic list row** — connections list, settings rows.
- **Section header** — grouping label (e.g., the "Requests & pickups" group already named in the decision log).
- **Avatar** — connection photo with an initials fallback.
- **Connection row** — avatar + name + status + action (approve/decline for pending, or open-library for accepted).
- **Notification row** — icon, message, timestamp, read/unread indicator, deep-link chevron. Needs a copy template per event type (request, return-to-confirm, connection request, approval outcome, overdue).
- **Borrow-history row** — borrower + borrowed/returned dates only, deliberately no on-time/late flag (Accepted decision — do not add one).

### G. Sheets, modals, overlays
Every entry below is already named in the IA doc's modal inventory (§6) — turning each into a component with real states is the remaining work.

- **Bottom sheet (base)** — handle, title, content region, dismiss. Every sheet below is built on this.
- **Action sheet** — Add method picker (Scan / Search / Manual), default-focused on Scan.
- **Edition disambiguation sheet** — bottom sheet with a thumbnail list to resolve an ambiguous scan/search hit without leaving the batch.
- **Due-date sheet** — see Forms above.
- **Borrower-picker sheet** — connections list + "Someone not on Stacks" free-text field (Lend flow, C6).
- **Decline → next-state sheet** — one action forking to available/unavailable, no reason field (Accepted decision).
- **Confirm/dispute sheet** — two explicit paths, "Confirm returned" and "Didn't receive it," not a single happy-path confirm.
- **Cancel-request confirm sheet** — low-friction, releases the lock.
- **Mark unavailable/available toggle sheet.**
- **Native share sheet** — system component, unstyled (Accepted, §2), Invite (P2).

### H. Scanner & cataloging (highest-polish, first impression — Principle 2)
**Redesigned 2026-08-10** — this is no longer a full-bleed camera view:

- **Live scanner surface** — an **inset** camera viewfinder (not full-bleed), paired with a persistent **live result panel** showing: the last-scanned item (cover thumbnail + title), a running count, and three actions — **Continue** (keep scanning), **Add** (?), **Review** (jump straight to the scan review list without ending the batch). Exact composition of Add vs. Continue still needs a pass, but the panel itself is settled.
- **Per-hit confirmation** — visual toast/highlight in the result panel, **plus a soft success haptic** on each hit; **a distinct double-buzz haptic** on a failed/ambiguous scan. Reduced-motion still needs a calm, non-flashing visual fallback.
- **Scan review & confirm list** — editable batch: thumbnail, title, per-item remove, single confirm-all action. Nothing enters the library without this step (Accepted decision, applies to future AI shelf scan too). Now explicitly reachable mid-batch via the panel's Review action, not just at the end.
- **Search results / edition picker list** — cover, title, author, year, enough to disambiguate.
- **Add success / continue** — light confirmation that keeps momentum to the next book, not a full-stop screen.
- **Camera-permission-denied state** — manual-fallback entry point must be reachable from here too, not only from the method picker.

### I. Empty, loading, and error states
- **Empty library state** — routes straight into Add; explicitly not a bare "nothing here" (Principle: empty states do real work).
- **Empty Browse / no connections state.**
- **Empty notification center state.**
- **Skeleton loaders** — book-card skeleton, list-row skeleton. Impeccable's product guidance is skeletons over mid-content spinners; recommended but not yet explicitly confirmed as the house rule (§5).
- **Inline error/retry** — network failure, metadata-resolver miss. Copy standard: say what happened and what to do next, no blame (already a house rule in `ux-heuristics.md`).
- **No-cover-art fallback** — a designed placeholder for editions the resolver can't find art for, used across every book-card instance.

### J. Onboarding-specific (the one brand-register surface)
- **Welcome / value-prop screen** — the natural home for New York at display scale (§1); PRODUCT.md explicitly calls this out as the one brand moment.
- **Sign in with Apple button** — system-styled, do not reskin (§2).
- **Profile setup form** — name, optional photo, approximate location, one screen.
- **First-run prompt** — routes directly into the scanner; another empty-state-that-does-real-work instance.

### K. Motion & transitions (new — elevated priority)
- **Book card → detail transition** — the priority target named directly by the product owner; likely a shared-element/hero treatment carrying the cover from card to detail header. Needs a dedicated Impeccable motion pass once the book card's static design is locked.
- **Sheet presentation/dismissal** — standard but should follow whatever duration/easing system comes out of the transition pass, not an ad hoc default.
- **Tab switches** — lower priority than card → detail, but should be decided in the same pass so the app doesn't end up with one polished transition and everything else on system defaults.

---

## 4. Component × state checklist

Per Impeccable's product-register rule, every interactive component needs its full state set designed, not just default + one alternate. Use this as the build checklist per component:

| Component type | Required states |
|---|---|
| Buttons | default, pressed, disabled, loading (async actions like confirm/approve), destructive-careful variant where relevant |
| Book card | all 6 states × viewer role, no-cover fallback, skeleton/loading, long-press (context menu open) |
| State badge | all 11 role-specific label variants (§3.C), each with color + SF Symbol + text, plus a capped/compact layout at AX1 |
| Text/search fields | default, focused, filled, error/validation, disabled |
| List rows | default, pressed, selected (where applicable), long-press (context menu open) |
| Sheets | entering, resting, dismissing, and — for multi-path sheets — each path's confirm state |
| Scanner | idle/searching, per-hit success (+ haptic), ambiguous/failed hit (+ error haptic, routes to disambiguation), manual-fallback entry, camera-permission-denied |
| Notification row | unread, read, badge-count parent state |
| Toggle/switch | on, off, disabled |

Two states are still easy to forget: **camera-permission-denied** (needs its manual-fallback path, §3.H) and **loading during an async circulation action** (approve, confirm-return, mark-handed-over all hit the network — what does the button do for that half-second?).

---

## 5. Design questions — remaining, prioritized

The 2026-08-10 round resolved most of the blocking foundational and platform questions (§1–§2 above capture the decisions). What's left:

### Needs exact values (direction already set)
1. ~~Primary, neutral-bg, and the six state colors in OKLCH~~ — **closed.** Values locked in `DESIGN.md` §Colors (2026-08-16 achromatic-chassis revision). Remaining work is a resync of the Figma `Color` collection and `tokens/colors.css`, plus confirming where `tokens/colors.css` actually lives — it isn't in this repo.
2. Type scale step values (even numbers, ~1.125–1.2 ratio).
3. Corner-radius scale (spacing scale is set; radius isn't).
4. Book-cover shadow values (blur/spread/opacity) once the card is crafted.
5. Page-transition spec: duration, easing, and the exact mechanic for book card → detail (shared-element on the cover is the likely direction, not yet built).

### Component-level, still open
6. Global Add: floating action button vs. header-embedded icon.
7. Visual treatment for "careful" actions (mark unavailable, decline) that must not read as destructive/red. **Narrowed:** `action/caution` was retired in the achromatic-chassis pass, so the options are now a `status/attention`-toned treatment or a neutral `background/fill` button — not a dedicated amber-brown token.
8. Skeleton-loader adoption as the house loading pattern — recommended, needs a yes.
9. Empty-state illustration style: custom line art, none, or something else — three empty states need this (library, browse/connections, notifications).
10. SF Symbol selection per state badge (icon system is chosen; the specific 11 symbols aren't picked yet).
11. Scanner result panel: exact composition of the Continue / Add / Review action row (three actions named, exact behavior of "Add" vs. "Continue" needs a pass).
12. Creative north star metaphor and 2–3 named anchor references for the visual system (Impeccable's `shape` inputs — still not named).

### Content, tied to components
13. Final microcopy pass on all 11 badge/role-label variants in §3.C (drafted in `circulation/copy-state-model.md`, not yet a copy deck).
14. Notification-row copy templates, one per event type.
15. Error-message copy standard (what happened + what to do next, already a house rule — needs an actual short library of messages).

### Already decided — do not re-open
Two rounds of decisions are now settled and shouldn't be re-litigated while working through the above: the circulation/IA decisions (two-step approve/handoff, off-app borrowers, decline forks, one safe sharing default, borrow history shows dates only, state never color-only, three-tab IA), and the 2026-08-10 foundations/platform round (Restrained warm color strategy, system-driven light/dark, SF Pro + New York typography, flat/tonal elevation with lifted covers, 8pt spacing grid, SF Symbols, no iPad, Dynamic Type capped at AX1 on dense components, haptics on scan, inset scanner with a live result panel, long-press context menus over swipe, pull-to-refresh everywhere, system-styled Sign in with Apple and share sheet). Full rationale for each lives in `design/design-decision-log.md` and `decisions/decision-log.md`.

---

## 6. Recommended sequencing

Matches the build-order already settled in `CLAUDE.md` and `design-decision-log.md` (cataloging first, circulation loop second, connections/browse third) — the visual-system work should follow the same order:

1. **Nail the exact values in §5's first group** (color, type scale, corner radius) — these are the last blockers, and they're now narrow, specific asks rather than open-ended direction calls. A short `/impeccable shape` or direct color/type session closes them.
2. **Craft the book card + state badge** — highest reuse, highest stakes, and the component every other screen depends on. Includes picking the 11 SF Symbols and building the AX1-capped compact layout.
3. **Craft the scanner surface + scan review list** — now a more involved build than originally scoped (inset camera + live result panel + haptics), still the highest-polish, first-impression flow (Principle 2).
4. **Run the motion pass** — book card → detail transition, once the static card design is locked, then extend the resulting duration/easing system to sheets and tab switches.
5. **Craft buttons, form controls, and the sheet system** as the circulation and connections flows get built.
6. **Log every visual decision** — exact color values, type scale, icon choices, and each component's final states — in `design/design-decision-log.md` as it's settled, and reflect the tokens into `DESIGN.md` so the SEED marker can come off entirely.
