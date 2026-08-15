# UX Heuristics — Review Checklist

Run any screen or flow against this before it's considered done. It's the itemized version of the craft heuristics in [`design-principles.md`](design-principles.md), tuned to Stacks. Not every item applies to every screen — but every item should be *considered*. When a screen fails an item, either fix it or log the deliberate exception in [`design-decision-log.md`](design-decision-log.md).

For a rigorous automated pass, use `/impeccable audit` (it can detect many anti-patterns programmatically). This checklist is the human lens.

---

## 1. Purpose & cognitive load
- [ ] The screen has **one primary job**, and I can state it in a sentence.
- [ ] The primary action is the most visually prominent element.
- [ ] Every field, option, and choice is necessary; anything the system can decide, it decides.
- [ ] No decision is asked of the user before they have what they need to make it.

## 2. Hierarchy & layout
- [ ] Visual weight matches importance — the key thing looks like the key thing.
- [ ] Single-column, mobile-first, scannable top to bottom.
- [ ] Spacing and alignment are consistent with the rest of the app.
- [ ] Empty states do real work (route the user forward), not just "nothing here."

## 3. Feedback & system state
- [ ] Every user action produces immediate, visible feedback.
- [ ] Scanner/batch flows show per-hit confirmation and a running count.
- [ ] Loading, success, and error states are explicitly designed.
- [ ] Copy state (if a book card) matches the state-by-role grid in `../circulation/copy-state-model.md`.

## 4. Interaction & touch
- [ ] Fully operable one-handed; primary actions sit in the thumb zone.
- [ ] Tap targets are ≥44pt with adequate spacing.
- [ ] Batch actions don't force a stop between items.
- [ ] Gestures (if any) have a visible, tappable equivalent.

## 5. Copy & tone
*Full ruleset: [`voice-and-tone.md`](voice-and-tone.md). Read it before writing any string.*
- [ ] No em dashes, no trailing qualifier clause after a comma, no "not X, but Y", no adjective triplets.
- [ ] No banned vocabulary (seamless, effortless, simply, just, unlock, curate, delight, magical).
- [ ] Nothing flatters the user or tells them how to feel.
- [ ] Language is plain, human, and low-pressure.
- [ ] Social asks (request, decline, nudge, return) remove awkwardness rather than create it.
- [ ] Labels are role-accurate (e.g. "Request pending" for the requester, "On hold" for other neighbors).
- [ ] No marketplace, gamification, or urgency language.
- [ ] Error messages say what happened and what to do next, without blame.

## 6. Accessibility
- [ ] Meets WCAG 2.1 AA contrast.
- [ ] **No state is signaled by color alone** — color is paired with a label and/or icon.
- [ ] Respects `prefers-reduced-motion`; scanner feedback has a calm, non-flashing fallback.
- [ ] Camera-based flows have an equally first-class manual fallback.
- [ ] Text stays legible at reading distance and in variable ambient light.
- [ ] Interactive elements are reachable and labeled for assistive tech.

## 7. Trust & privacy
- [ ] No precise address or sensitive location exposed by default.
- [ ] Declining, disputing a return, or marking a copy unavailable never feels punitive.
- [ ] The most private safe option is the default.
- [ ] Nothing pressures a user into lending or borrowing.

## 8. Consistency
- [ ] Reuses the shared book-card and state-badge vocabulary rather than a one-off treatment.
- [ ] Matches established patterns; any genuinely new pattern is logged as a design decision.
- [ ] Terminology matches the glossary in `../CLAUDE.md` (edition, copy, steward, borrower, connection).

---

### If it fails
Fix it, or record the intentional exception with its rationale in [`design-decision-log.md`](design-decision-log.md). An undocumented failure is a bug waiting to be rediscovered.
