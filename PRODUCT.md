# Product

<!-- This is the strategic context file the Impeccable design skill loads before any design work. It answers who/what/why. Visual "how it looks" lives in DESIGN.md. Keep it accurate: every design task inherits from here. Derived from product/product-brief.md and decisions/decision-log.md — update all three together when strategy shifts. -->

## Register

product

<!-- Primary surface is the mobile app UI, so design SERVES the product. The onboarding/welcome sequence has a brand-register moment (selling the neighborhood-library idea) — treat that one flow as brand when working on it, but the default for the app is product. -->

## Users

**The Steward.** Owns 100–500 books, wants them catalogued, willing to lend inside a trusted circle. The anchor user and driver of supply. Context: at home, standing at a bookshelf, phone in one hand, pulling books off the shelf to scan. Wants the shelf catalogued without it feeling like data entry, and wants lent books tracked and returned. Success is a catalogued shelf and low ongoing effort to keep it current.

**The Borrower.** Wants to read without buying. Browses connected neighbors' libraries, requests loans, returns them. Context: idle browsing on a couch, or a deliberate "who near me has this title" search. Success is discovering something good on a shelf they trust and borrowing it without friction or social awkwardness.

Most real users are **both**. The app is one shell with modes, not two separate experiences. The primary job on any given screen is either *get my shelf in fast* (catalog) or *find and borrow something* (circulate).

## Product Purpose

Stacks turns private bookshelves into a shared neighborhood library. Catalog what you own once, choose exactly who can see it, and lend and borrow within a circle you trust. Less consumption, more local community, more books in motion.

The catalog is standalone-useful even if you never lend — it competes with personal cataloging tools like Libib and LibraryThing, so a user gets payoff immediately, alone, before any neighbor is involved. That solves the cold-start problem that kills local networks. **The catalog is the wedge; lending and community is the mission.** This shapes onboarding: lead with the neighborhood-library idea, deliver catalog value first.

Success for the POC is proving one full request-to-return loop between two real users.

## Brand Personality

Three words: **neighborly, trustworthy, effortless.**

Warm and local, not corporate or transactional — this is the Little Free Library feeling with modern coordination on top, not a marketplace. Calm and confidence-building around anything involving other people's property and trust (connections, lending, returns). The cataloging experience should feel a little bit *magical* — batch-scanning a shelf and watching it fill up is the first impression and the hook. Voice is plain, human, and low-pressure; social asks (request, nudge, return) are phrased to remove awkwardness between neighbors, never to gamify or pressure.

## Anti-references

- **Not a marketplace / e-commerce.** No resale, no pricing, no "buy now" energy, no aggressive conversion patterns. This is lending between neighbors, not a store.
- **Not a reading social network.** Not Goodreads. No ratings, reviews, feeds, streaks, or recommendation engines competing for attention. Ownership and circulation are the focus.
- **Not enterprise/library-software sterile.** Avoid the cold, dense, admin-panel feel of institutional library catalog systems (ILS/OPAC). Warm and human, not municipal.
- **No dark patterns around trust.** Never pressure a lend, never expose a precise address by default, never make declining or saying "I don't have it back yet" feel punitive.

## Design Principles

Strategic principles that govern product-design decisions. (Craft-level visual and UX heuristics live in `design/design-principles.md`; visual tokens in `DESIGN.md`.)

1. **Catalog value first, community value second.** Every onboarding and empty-state decision should deliver standalone catalog payoff before requiring a neighbor. Never gate the first win behind another person showing up.
2. **Cataloging must feel like magic, not data entry.** The scan-batch-confirm flow is the make-or-break interaction. Optimize it above everything: speed (target under 5s/book), instant feedback, a visible running count. If this feels like a chore, nothing downstream matters.
3. **Lending state is always legible.** At a glance, on any book card, it's clear whether a copy is available, out, to whom, and when it's due. The copy-state model (`circulation/copy-state-model.md`) is the source of truth every card renders.
4. **Remove social friction and risk.** Requests, approvals, declines, nudges, and returns are designed to protect the relationship between neighbors. Low-pressure, private-by-default, no shame. Trust is the product; the UI must earn it.
5. **One safe default over a wall of choices.** Especially for sharing/visibility and permissions — ship a sensible default ("visible to connections") and don't make people configure to get value.

## Accessibility & Inclusion

- Target **WCAG 2.1 AA**. All lending-state indicators must be distinguishable **without relying on color alone** — pair every state color (amber/blue/red/neutral badges) with a text label and/or icon, since the copy-state model leans heavily on colored badges.
- Mobile-first, single-column, **thumb-reachable** primary actions. The core loop must be operable one-handed while standing at a shelf.
- Respect reduced-motion preferences; the scanner's feedback animation should have a calm, non-flashing fallback.
- Camera-forward flows (barcode/shelf scan) need non-camera fallbacks (manual search) that are equally first-class, not buried.
- Legible type at real-world reading distances and in variable ambient light (a dim room, a sunny window).
