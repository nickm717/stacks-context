# Design Principles — House Heuristics for Stacks

The design rules we hold ourselves to, distilled for this product. Read this before designing or critiquing any screen. It's deliberately concise; for deep design work, invoke the **Impeccable** skill (see the end of this doc), which carries the full craft references and reads our `PRODUCT.md` and `DESIGN.md` automatically.

Two layers govern design here:
- **Strategic principles** — the five in `../PRODUCT.md` under "Design Principles". Those decide *what* we design.
- **Craft heuristics** — below. These decide *how well* we design it.

---

## The five that matter most for Stacks

These are our strategic principles restated as design tests you can apply to a screen:

1. **Catalog value first.** Does this screen deliver payoff without requiring a neighbor to exist? The first win must never be gated behind another person.
2. **Cataloging feels like magic, not data entry.** Is the scan → confirm path fast (target <5s/book), with instant feedback and a visible running count? This flow is graded harder than any other.
3. **Lending state is legible at a glance.** On every book card, is it instantly clear whether the copy is available, out, to whom, and when it's due — using label + icon, not color alone?
4. **Social friction removed.** Do requests, declines, nudges, and returns protect the relationship between neighbors? Low-pressure, private-by-default, never shaming.
5. **One safe default over a wall of choices.** Especially sharing and permissions — did we ship a sensible default instead of a configuration screen?

---

## Craft heuristics (the review lens)

Grouped the way we audit a screen. The itemized checklist version lives in [`ux-heuristics.md`](ux-heuristics.md).

**Cognitive load.** Every screen has one primary job — name it, make it the visually dominant action. Cut fields, options, and decisions to the minimum that still works (Steward onboarding is one screen; sharing is one default). Don't make people think about things the system can decide for them.

**Hierarchy & focus.** The most important thing on the screen looks the most important. One primary action per screen, thumb-reachable. Secondary actions recede. Empty states do real work — the empty library routes straight into the scanner, it doesn't just say "nothing here."

**Feedback & state.** Every action has immediate, visible response — nowhere more than the scanner (per-hit confirmation, running count). Never leave the user wondering if something worked. The six copy states each have a distinct, labeled treatment; loading, success, and error states are designed, not afterthoughts.

**Interaction & touch.** Mobile-first, single-column, one-handed at a bookshelf. Primary actions in the thumb zone. Tap targets ≥44pt. Batch flows (scanning) never force a stop between items.

**UX writing.** Plain, human, low-pressure. Social asks are phrased to remove awkwardness ("Request to borrow," not pressuring language; a nudge stays human). Labels are role-accurate (requester sees "Request pending," neighbors see "On hold"). No jargon, no marketplace or gamification tone.

**Accessibility.** WCAG 2.1 AA. State never by color alone — always color + label/icon. Respect reduced motion (calm, non-flashing scanner fallback). Camera flows always have an equally first-class manual fallback. Legible type in variable ambient light.

**Trust & privacy by design.** Never expose a precise address by default. Declining, disputing a return, or marking unavailable must never feel punitive. Default to the most private safe option. The UI's job is to earn trust, because trust is the product.

**Consistency.** Reuse the book-card and state-badge vocabulary everywhere rather than inventing per-screen treatments. New patterns are a decision — log them in `design-decision-log.md`.

---

## What to avoid (anti-patterns for this product)

Marketplace/e-commerce pressure (urgency, "buy now," conversion funnels). Reading-social-network features (ratings, feeds, streaks, recommendation engines). Institutional-library sterility (dense admin-panel density, municipal coldness). Any dark pattern around trust, lending, or address exposure. Color-only state signaling. Configuration walls where a default would do.

---

## Using the Impeccable skill

For any substantial design or redesign — a new screen, a flow, a component system, a critique, a polish pass — invoke **Impeccable** rather than designing ad hoc. It encodes production-grade craft (color strategy in OKLCH, type, spatial and interaction design, motion, UX writing, anti-pattern detection) and, crucially, **auto-loads `PRODUCT.md` and `DESIGN.md` from the repo root**, so its output stays on-brand and consistent with everything here.

Useful sub-commands:
- `/impeccable shape` — interview + design brief before building a new surface.
- `/impeccable craft <target>` — design and implement a screen or component.
- `/impeccable audit` / `/impeccable critique` — review an existing interface against heuristics and anti-patterns.
- `/impeccable polish` / `/impeccable clarify` / `/impeccable distill` — refine an existing surface.

Workflow reminder (also in `../CLAUDE.md`): orient here → check `design-decision-log.md` for prior constraints → design (Impeccable) → review against `ux-heuristics.md` → **log the decision**. Keep `PRODUCT.md` and `DESIGN.md` current so every future task inherits the change.
