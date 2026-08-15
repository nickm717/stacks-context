<!-- SEED -->
<!--
This is the visual-system context file the Impeccable design skill loads before design work. It answers "how it looks."
STATUS: FOUNDATIONS SETTLED, EXACT VALUES STILL OPEN. Color strategy, theme, typography, elevation, spacing, icons,
and motion direction were decided 2026-08-10 (see design/design-decision-log.md, "Visual system" + "Cross-cutting").
Remaining [TODO]s are exact numeric/hex values, not open direction. Sections are structured per the Google Stitch
DESIGN.md format (six sections, fixed order, do not rename or reorder). Once exact color values and the type scale
are filled in, remove the SEED marker entirely. To capture real tokens once there's code, run `/impeccable document`.
-->

---
name: Stacks
description: A neighborhood book-lending app — catalog your shelves, lend within a circle you trust.
colors:
  # Direction locked (2026-08-10): warm, paper-adjacent neutrals and near-blacks — not sepia/vintage, not cool/corporate.
  # Every token below needs a light AND dark value (system-driven theme support is accepted). Exact OKLCH values [TODO].
  primary: "#000000"        # [TODO] warm accent hue, light + dark variants
  neutral-bg: "#000000"     # [TODO] warm paper-adjacent background, light + dark variants
  # Lending-state colors are semantically REQUIRED (see copy-state-model.md) and inherit the same warm undertone.
  # Roles fixed, values [TODO], each needs light + dark variants verified against WCAG AA with its label/icon:
  state-available: "#000000"  # [TODO] calm/positive
  state-pending: "#000000"    # [TODO] amber — requested / on hold / ready for pickup
  state-return: "#000000"     # [TODO] blue — return pending
  state-overdue: "#000000"    # [TODO] red — overdue
typography:
  display: { fontFamily: "New York", fontSize: "[TODO]", fontWeight: 400, lineHeight: 1.1, letterSpacing: "normal" }
  body:    { fontFamily: "SF Pro", fontSize: "[TODO]", fontWeight: 400, lineHeight: 1.5, letterSpacing: "normal" }
  label:   { fontFamily: "SF Pro", fontSize: "[TODO]", fontWeight: 600, lineHeight: 1.2, letterSpacing: "normal" }
  # All fontSize values must be even numbers, on the 8pt-aligned scale below. Concrete step values still [TODO].
rounded:
  sm: "[TODO]"   # corner radius scale not yet set — should follow the same even-number discipline as spacing
  md: "[TODO]"
spacing:
  sm: "8"
  md: "16"
  # Fuller 8pt-grid scale (4px minimum increment): 4 / 8 / 16 / 24 / 32 / 40 / 48. See design-decision-log.md.
components:
  # [TODO] one entry per real component variant once designed. The book card is the primary object;
  # it must render all six copy states legibly. Example shape to fill in:
  book-card: { backgroundColor: "{colors.neutral-bg}", rounded: "{rounded.md}", elevation: "lifted" }
  state-badge-available: { backgroundColor: "{colors.state-available}", textColor: "{colors.neutral-bg}", rounded: "{rounded.sm}", elevation: "flat" }
---

## Overview

**Creative North Star:** [TODO — a single named metaphor for the whole system, e.g. "The Corner Bookshelf," honoring the neighborly/paper/trust personality in PRODUCT.md.]

Mood and philosophy: [TODO — 2–3 sentences.] Anchor to the brand personality: **neighborly, trustworthy, effortless**; warm and local like a Little Free Library, not a marketplace or an institutional catalog. Anti-references (binding, from PRODUCT.md): not e-commerce, not a reading social network, not sterile library software, no trust-eroding dark patterns.

**Binding constraints (already decided):**
- Mobile-first, single-column, thumb-reachable primary actions. One-handed operation at a bookshelf. iPhone only for the POC — no iPad layout.
- WCAG 2.1 AA. **Never encode lending state in color alone** — every state pairs its color with a text label and/or icon.
- Respect `prefers-reduced-motion`. The scanner feedback must have a calm, non-flashing fallback.
- System-driven light and dark mode, with a user override available in settings.
- Full standard Dynamic Type range everywhere; accessibility range (AX1–AX5) capped at AX1 on dense components (tab bar, badges, book-card grid metadata, chips), full range on text-heavy screens.

## Colors

**Strategy: Restrained**, per the product register default and PRODUCT.md's steer — tinted neutrals plus one accent, not a saturated or full-palette surface. Use OKLCH as the working space; never `#000`/`#fff`.

**Direction locked, exact values open.** Warm-toned grays and near-blacks with a paper-adjacent (not sepia) neutral background. The same warm undertone should carry through to the semantic state palette below, not just the neutrals — this is what keeps the six state colors feeling like one family instead of a generic status-color set bolted on. Every token needs a light and a dark variant (system-driven theme is accepted).

**Semantic lending-state palette is required and its roles are fixed** (see `circulation/copy-state-model.md`): available (calm/positive), requested & ready-for-pickup (amber), return-pending (blue), overdue (red), unavailable (neutral/muted), checked-out (muted cover). Each state MUST also carry a label/icon, never color alone.

## Typography

**SF Pro carries the UI** — body, labels, buttons, data, nearly everything. **New York (serif)** is reserved for display/heading type only, giving an editorial, book-ish accent without sacrificing native legibility elsewhere. Onboarding's brand-register moment is the natural home for New York at its largest scale. Type scale: even-numbered sizes only, tighter product-UI ratio (roughly 1.125–1.2 between steps); concrete step values still open. Must stay legible at reading distance in variable ambient light, and scale correctly through the Dynamic Type ranges defined above.

## Elevation

**Mostly flat/tonal, with lifted book covers.** Card chrome, sheets, and list rows stay flat or tonal — restrained, not heavy. Book covers specifically get a light shadow/lift so they read as physical objects with dimension, reinforcing the book card as the app's central object. Don't extend the lifted treatment to badges, buttons, or sheets.

## Components

Document each component as it's designed. Priority order matches build sequence:

1. **Book card** — the primary object. Must render all six copy states clearly (available, requested, ready for pickup, checked out, return pending, overdue, unavailable) with color + label + icon, in both grid and list layouts. Cover gets the lifted-shadow treatment; card chrome stays flat. Gains a long-press context menu for row/card-level actions (replaces swipe — needed because My Shelf supports both list and grid layouts). This is the most important component in the system.
2. **Barcode scanner surface** — not full-bleed camera. Inset live camera view paired with a live result panel: last-scanned item, running count, and Continue / Add / Review actions. Soft success haptic on each hit, distinct double-buzz haptic on a failed/ambiguous scan. Highest-polish, first impression.
3. **Scan review list** — editable batch, remove misfires, confirm all. Reachable directly from the scanner panel's Review action.
4. Buttons (primary request/approve/return actions), badges, search field, list/grid toggle, tab bar (Library · Browse · Profile), pull-to-refresh on Library/Browse/notification center, empty states. Icon system is SF Symbols throughout.

## Do's and Don'ts

**Do**
- Make cataloging feel fast and a little magical — instant feedback, visible running count, haptic confirmation on scan hits.
- Keep lending state legible at a glance on every card; pair color with a label/icon always.
- Honor the 8pt spacing grid (4px minimum) and even-numbered type scale everywhere.
- Ship one safe sharing default; don't force configuration to get value.
- Keep social actions (request, decline, nudge, return) low-pressure and private-by-default.
- Invest real craft in page transitions, especially book card → detail — motion is a priority, not an afterthought.

**Don't**
- Don't build a full-bleed scanner camera view — it's inset with a live result panel (see Components).
- Don't use swipe actions on book cards/rows — long-press context menu instead, since My Shelf has both list and grid layouts.
- Don't lean on marketplace, feed, or gamification patterns (streaks, badges-as-rewards, urgency).
- Don't expose precise addresses by default.
- Don't rely on color alone to signal a state.
- Don't design an iPad layout for the POC.
