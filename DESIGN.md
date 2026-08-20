<!--
This is the visual-system context file the Impeccable design skill loads before design work. It answers "how it looks."
STATUS: TYPE + SPACING LOCKED. COLOR REVISED 2026-08-16 — NOT YET RESYNCED to the Figma variables library or
tokens/colors.css. The accent color was removed entirely and the status palette collapsed; see
design/design-decision-log.md, "Achromatic chassis: brand accent removed, status palette collapsed to four hues."
Type, spacing, and radius values are unchanged and still synced. Sections are structured per the Google
Stitch DESIGN.md format (six sections, fixed order, do not rename or reorder). To re-sync after a code change, run
`/impeccable document`.
-->

---
name: Stacks
description: A neighborhood book-lending app — catalog your shelves, lend within a circle you trust.
colors:
  # ACHROMATIC CHASSIS. There is no brand accent color. Persistent chrome — nav, tab bar, chips, buttons —
  # is warm near-black on warm near-white. Color appears only in three momentary roles: lending status,
  # cover tint, and scanner feedback. OKLCH working space; every token has a light AND dark value.
  primary: { light: "oklch(0.255 0.007 62)", dark: "oklch(0.945 0.003 80)" }    # ink — primary action fill (inverts in dark)
  neutral-bg: { light: "oklch(0.988 0.003 78)", dark: "oklch(0.195 0.004 72)" } # barely-warm near-white / near-black
  # Lending status uses FOUR hues, not one per state. Hue encodes "does this need me"; the paired
  # icon + label encode which exact state (see copy-state-model.md). Never color alone. All verified WCAG AA.
  status-positive:  { light: "oklch(0.500 0.090 150)", dark: "oklch(0.800 0.095 150)" } # green — available
  status-attention: { light: "oklch(0.520 0.090 82)",  dark: "oklch(0.820 0.090 88)" }  # amber — someone must act
  status-overdue:   { light: "oklch(0.530 0.115 28)",  dark: "oklch(0.785 0.115 30)" }  # red — overdue
  status-neutral:   { light: "oklch(0.510 0.006 70)",  dark: "oklch(0.740 0.006 74)" }  # gray — informational only
typography:
  display: { fontFamily: "New York", fontSize: "40px", fontWeight: 400, lineHeight: 1.12, letterSpacing: "-0.02em" }
  body:    { fontFamily: "SF Pro", fontSize: "16px", fontWeight: 400, lineHeight: 1.5, letterSpacing: "0em" }
  label:   { fontFamily: "SF Pro", fontSize: "14px", fontWeight: 600, lineHeight: 1.22, letterSpacing: "0em" }
  # Full even-numbered scale (px): 12 / 14 / 16 / 18 / 20 / 24 / 28 / 32 / 40. See "Typography" section below.
rounded:
  sm: "8px"    # small chips, thumbnails
  md: "12px"   # cards, list rows, callouts
spacing:
  sm: "8"
  md: "16"
  # Full 8pt-grid scale (4px minimum increment): 4 / 8 / 12 / 16 / 24 / 32 / 40 / 48. See design-decision-log.md.
components:
  # One entry per real component variant. The book card is the primary object; must render all six copy states legibly.
  book-card: { backgroundColor: "{colors.neutral-bg}", rounded: "{rounded.md}", elevation: "lifted" }
  status-badge-positive: { backgroundColor: "status/positive/bg", textColor: "{colors.status-positive}", rounded: "999px", elevation: "flat" }
  button-primary: { backgroundColor: "{colors.primary}", textColor: "text/on-action", rounded: "999px", elevation: "flat" }
  chip-selected: { backgroundColor: "{colors.primary}", textColor: "text/on-action", rounded: "999px", elevation: "flat" }
---

## Overview

**Creative North Star:** "The Corner Bookshelf" — warm, unpretentious, and neighborly, like a Little Free Library rather than a marketplace or an institutional catalog.

Mood and philosophy: paper-toned and calm, with **no brand accent color at all** — the book covers are the color. The interface is a warm-neutral frame that gets out of their way. Restrained by default, with craft concentrated in the moments that matter most (the scanner, the book cover, page transitions) rather than spread evenly. Anchor to the brand personality: **neighborly, trustworthy, effortless**. Anti-references (from PRODUCT.md): not e-commerce, not a reading social network, not sterile library software, no trust-eroding dark patterns.

**Binding constraints (already decided):**
- Mobile-first, single-column, thumb-reachable primary actions. One-handed operation at a bookshelf. iPhone only for the POC — no iPad layout.
- WCAG 2.1 AA. **Never encode lending state in color alone** — every state pairs its color with a text label and/or icon.
- Respect `prefers-reduced-motion`. The scanner feedback must have a calm, non-flashing fallback.
- System-driven light and dark mode, with a user override available in settings.
- Full standard Dynamic Type range everywhere; accessibility range (AX1–AX5) capped at AX1 on dense components (tab bar, badges, book-card grid metadata, chips), full range on text-heavy screens.

## Colors

**Strategy: Achromatic chassis, momentary color.** There is no brand accent. OKLCH is the working space throughout; never `#000`/`#fff`.

Everything persistent — nav bars, tab bar, chips, buttons, cards, list rows, dividers — is warm near-black ink on a warm near-white ground. Color is not part of the frame. It appears in exactly three roles, all of them transient or content-derived:

1. **Lending status** — four muted hues on pale badge fills, always paired with an icon and label.
2. **Cover tint** — a hue sampled from the book's own cover, clamped, used on the book detail header only.
3. **Scanner feedback** — the single most saturated color in the system, on screen for a moment at a time.

The warmth is deliberately faint: neutrals sit at hue ~72–78° with chroma **0.003**, low enough to read as white/gray at a glance but warm enough to feel like paper rather than a spec sheet when placed beside a true-neutral UI. This is what carries "Corner Bookshelf" now that no accent color does. Every token has a light and a dark variant.

**Primary actions are ink, not accent.** The primary button is a solid `action/primary` pill — near-black in light mode, near-white in dark. Secondary actions use `background/fill` with `text/primary`. Selected chips and tabs use the same ink fill as the primary button; unselected use `background/fill`. **Links are underlined `text/primary`, not colored** — there is no link color.

**Token naming: `category/role/variant`, direct semantic values, no primitives layer.** Tokens are named e.g. `background/default`, `text/primary`, `action/primary`, `status/overdue/bg` in Figma, and `--color-background-default`, `--color-text-primary`, `--color-action-primary`, `--color-status-overdue-bg` in CSS — one Figma variable per CSS custom property, values kept in sync by hand-verified spot-checks rather than a build pipeline. A primitives tier (raw color ramp → semantic alias) was prototyped and deliberately rejected: with only ~36 semantic tokens and no case of one primitive backing many semantic roles, the extra indirection cost more than it bought. Revisit only if a genuine multi-token fan-out shows up (e.g. a real dark-mode-by-brand-partner scenario). See the decision log for the full reasoning.

**Locked values** (light / dark, OKLCH):

*Neutral chassis — the whole persistent interface is built from these twelve.*

| Token | Light | Dark |
|---|---|---|
| `background/default` | `oklch(0.988 0.003 78)` | `oklch(0.195 0.004 72)` |
| `background/raised` | `oklch(0.999 0.002 78)` | `oklch(0.235 0.005 72)` |
| `background/sunken` | `oklch(0.962 0.004 78)` | `oklch(0.175 0.004 72)` |
| `background/fill` | `oklch(0.940 0.005 78)` | `oklch(0.300 0.006 72)` |
| `background/fill-strong` | `oklch(0.905 0.006 78)` | `oklch(0.350 0.006 72)` |
| `border/default` | `oklch(0.908 0.005 78)` | `oklch(0.335 0.005 72)` |
| `border/subtle` | `oklch(0.938 0.004 78)` | `oklch(0.285 0.005 72)` |
| `text/primary` | `oklch(0.255 0.007 62)` | `oklch(0.945 0.003 80)` |
| `text/secondary` | `oklch(0.430 0.006 62)` | `oklch(0.800 0.004 80)` |
| `text/tertiary` | `oklch(0.595 0.005 66)` | `oklch(0.660 0.005 76)` |
| `text/faint` | `oklch(0.700 0.005 70)` | `oklch(0.560 0.005 74)` |
| `text/on-action` | `oklch(0.988 0.003 78)` | `oklch(0.195 0.004 72)` |

*Actions — ink, inverting between modes. No accent hue.*

| Token | Light | Dark |
|---|---|---|
| `action/primary` | `oklch(0.255 0.007 62)` | `oklch(0.945 0.003 80)` |
| `action/primary-hover` | `oklch(0.330 0.007 62)` | `oklch(0.880 0.003 80)` |
| `action/primary-press` | `oklch(0.395 0.007 62)` | `oklch(0.820 0.004 80)` |
| `action/danger/fg` · `/bg` (rare, true destructive) | `oklch(0.530 0.170 28)` · `oklch(0.936 0.048 30)` | `oklch(0.790 0.155 30)` · `oklch(0.350 0.075 30)` |
| `overlay/scrim` | `oklch(0.20 0.006 62 / 0.32)` | `oklch(0.10 0.006 62 / 0.50)` |
| `overlay/tabbar` (translucent, blurred) | `oklch(0.988 0.003 78 / 0.86)` | `oklch(0.195 0.004 72 / 0.82)` |

**Lending status — four hues, seven states.** Hue answers *"does this need me?"*; the icon and label answer *"which state exactly?"* This is the whole point of the collapse: seven distinct hues asked color to carry information it was never allowed to carry alone anyway.

| Token | fg (light) | bg (light) | fg (dark) | bg (dark) | Covers |
|---|---|---|---|---|---|
| `status/positive` (green) | `oklch(0.500 0.090 150)` | `oklch(0.945 0.028 150)` | `oklch(0.800 0.095 150)` | `oklch(0.325 0.038 150)` | Available |
| `status/attention` (amber) | `oklch(0.520 0.090 82)` | `oklch(0.944 0.032 85)` | `oklch(0.820 0.090 88)` | `oklch(0.328 0.042 85)` | Requested · Ready for pickup · Return pending |
| `status/overdue` (red) | `oklch(0.530 0.115 28)` | `oklch(0.942 0.036 30)` | `oklch(0.785 0.115 30)` | `oklch(0.340 0.058 30)` | Overdue |
| `status/neutral` (gray) | `oklch(0.510 0.006 70)` | `oklch(0.935 0.004 70)` | `oklch(0.740 0.006 74)` | `oklch(0.318 0.005 74)` | Checked out · Unavailable |

Overdue carries the highest chroma of the four (0.115 vs 0.090) so it still wins the glance without being an alarm.

**Cover tint — one token, computed per book.** Sample the dominant hue from the cover, discard its lightness and chroma, and substitute fixed values. Hue is the only free variable, so contrast against `text/primary` is guaranteed by construction rather than by inspection.

| Token | Light | Dark |
|---|---|---|
| `background/cover-tint` | `oklch(0.945 0.045 <H>)` | `oklch(0.300 0.050 <H>)` |

Rules: **book detail header only** — never shelf cards, grid tiles, or list rows, which would turn a browse screen polychrome and break the achromatic chassis. If extraction fails, or the cover's dominant chroma is below 0.02 (a black/white/gray cover), fall back to `background/sunken`. The sampled hue is cached with the book record; it is not recomputed per render.

**Scanner feedback — the loudest color in the app.** Deliberately a hue used nowhere else, so a scan hit can never be misread as a lending status. On screen for well under a second at a time.

| Token | Light | Dark |
|---|---|---|
| `feedback/scan-hit/fg` · `/bg` | `oklch(0.700 0.140 195)` · `oklch(0.930 0.050 195)` | `oklch(0.780 0.130 195)` · `oklch(0.330 0.060 195)` |
| `feedback/scan-miss` | aliases `action/danger/fg` | aliases `action/danger/fg` |

Per the reduced-motion constraint, the scan-hit treatment must have a non-flashing fallback: a held tint rather than a pulse.

**Retired tokens** (do not reintroduce without a decision-log entry): `action/primary` as terracotta, `action/primary-hover`/`-press` in terracotta, `background/brand-subtle`, `background/brand-subtle-strong`, `text/link`, `text/on-accent` (renamed `text/on-action`), `action/caution/fg`·`/bg` (folded into `status/attention` — two near-identical ambers was one too many), and the per-state `state-*` palette.

Full values, code syntax, and shadow tokens live in `tokens/colors.css` in the design-system package; the Figma library (`Design System` file, `Color` variable collection, Light/Dark modes) is kept pixel-identical to it. **Both are now out of sync with this file and need a resync pass.**

## Typography

**SF Pro carries the UI** — body, labels, buttons, data, nearly everything. **New York (serif)** is reserved for display/heading type only, giving an editorial, book-ish accent without sacrificing native legibility elsewhere. Onboarding's brand-register moment is the natural home for New York at its largest scale. Both ship as system font stacks, no downloaded webfont binaries.

**Locked type scale** (even-numbered, ~1.12–1.2 ratio):

| Style | Size | Line-height | Letter-spacing | Weight | Usage |
|---|---|---|---|---|---|
| Display | 40px | 1.12 | -0.02em | Regular (serif) | Onboarding value-prop |
| Large Title | 32px | 1.12 | -0.02em | Bold | Large iOS titles |
| Title 1 | 28px | 1.12 | -0.01em | Bold | Screen titles |
| Title 2 | 24px | 1.22 | -0.01em | Semibold | Section / pushed-screen titles |
| Title 3 | 20px | 1.22 | -0.01em | Semibold | Detail titles |
| Headline | 18px | 1.22 | 0em | Semibold | Row titles, emphasis |
| Body | 16px | 1.5 | 0em | Regular | Base body / control text |
| Footnote | 14px | 1.35 | 0em | Regular | Secondary text, list subtitles |
| Caption | 12px | 1.35 | 0em | Regular | Badges, timestamps, fine print |
| Eyebrow | 12px | 1.35 | 0.06em (caps) | Semibold | All-caps section labels |

Weights available: Regular (400), Medium (500), Semibold (600), Bold (700), Heavy (800). Must stay legible at reading distance in variable ambient light, and scale correctly through the Dynamic Type ranges defined above.

## Elevation

**Mostly flat/tonal, with lifted book covers.** Card chrome, sheets, and list rows stay flat or tonal — restrained, not heavy. Book covers specifically get a light two-layer shadow (`shadow/cover`, `shadow/cover-sm` for smaller covers) so they read as physical objects with dimension, reinforcing the book card as the app's central object. Sheets get their own shadow (`shadow/sheet`). Don't extend the lifted treatment to badges, buttons, or other sheets.

## Components

Document each component as it's designed. Priority order matches build sequence:

1. **Book card** — the primary object. Renders all seven copy states clearly (available, requested, ready for pickup, checked out, return pending, overdue, unavailable) using the four `status/*` hues + label + icon, in both grid and list layouts. Card chrome is achromatic; no cover tint at card level. Cover gets the lifted-shadow treatment; card chrome stays flat. Long-press context menu for row/card-level actions (replaces swipe — needed because My Shelf supports both list and grid layouts). Built in the Figma library (`Components` page, `Book Card` master + Light/Dark demo frames) and in code (`components/books/BookCard.jsx`, `Cover.jsx`, `StateBadge.jsx`).
2. **Barcode scanner surface** — not full-bleed camera. Inset live camera view paired with a live result panel: last-scanned item, running count, and Continue / Add / Review actions. Soft success haptic on each hit, distinct double-buzz haptic on a failed/ambiguous scan. Highest-polish, first impression. Not yet built.
3. **Scan review list** — editable batch, remove misfires, confirm all. Reachable directly from the scanner panel's Review action. Not yet built.
4. Buttons (primary request/approve/return actions), badges, search field, list/grid toggle, tab bar (Library · Browse · Profile), pull-to-refresh on Library/Browse/notification center, empty states. Icon system is SF Symbols throughout.

## Do's and Don'ts

**Do**
- Make cataloging feel fast and a little magical — instant feedback, visible running count, haptic confirmation on scan hits.
- Keep lending state legible at a glance on every card; pair color with a label/icon always.
- Honor the 8pt spacing grid (4px minimum) and even-numbered type scale everywhere.
- Ship one safe sharing default; don't force configuration to get value.
- Keep social actions (request, decline, nudge, return) low-pressure and private-by-default.
- Invest real craft in page transitions, especially book card → detail — motion is a priority, not an afterthought.
- Name design tokens `category/role/variant` (Figma) / `--color-category-role-variant` (CSS) and keep both in sync by direct value comparison, not a primitives/alias layer.
- Keep every neutral warm-cast at hue 72–78°, chroma 0.003. If a neutral ever needs to go cooler, that's a decision-log entry, not a tweak.
- Let covers be the color. When a screen feels flat, add a cover, not a hue.

**Don't**
- Don't build a full-bleed scanner camera view — it's inset with a live result panel (see Components).
- Don't use swipe actions on book cards/rows — long-press context menu instead, since My Shelf has both list and grid layouts.
- Don't lean on marketplace, feed, or gamification patterns (streaks, badges-as-rewards, urgency).
- Don't expose precise addresses by default.
- Don't rely on color alone to signal a state.
- **Don't introduce a brand accent color.** It was tried (terracotta clay, ~42°) and removed — it collided with overdue red at 28°, and it forced the accent to be both the brand signature and every CTA fill. Primary actions are ink.
- **Don't color links.** Underlined `text/primary`, no hue.
- **Don't apply cover tint outside the book detail header.** Tinted grid cards defeat the achromatic chassis.
- Don't design an iPad layout for the POC.
- Don't add a primitives/raw-color tier under semantic tokens unless a real multi-token fan-out case shows up — it was tried and reverted once already.
