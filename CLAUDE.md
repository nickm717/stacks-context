# CLAUDE.md — Operating Rules for Stacks

This file is the entry point for any AI agent working on **Stacks** (Claude Code, Claude Design, Cowork, or otherwise). Read it first. It defines how we work, where knowledge lives, and the rules that must be respected on every task. These rules travel with the repo — they apply identically no matter which tool opens it.

*Stacks turns private bookshelves into a shared neighborhood library. Catalog what you own once, choose who can see it, and lend within a circle you trust. The personal catalog is the standalone-useful wedge; lending and local community is the mission. Status: pre-build, seeding the knowledge base and preparing a proof of concept.*

---

## The rules (non-negotiable)

1. **Respect the decision logs. Do not silently re-litigate settled calls.**
   - Product/engineering/circulation decisions live in `decisions/decision-log.md`.
   - Product and UX **design** decisions live in `design/design-decision-log.md`.
   - Before proposing anything that contradicts an *Accepted* decision, surface the conflict, say which decision it touches, and make the case explicitly. Never reverse a settled decision by accident.

2. **Log every major design decision as you make it.** Any consequential product-design or UX call — a flow, a state treatment, an information-architecture choice, a tradeoff between two patterns — gets appended to `design/design-decision-log.md` using the entry template there. If it was decided in conversation and isn't written down, it didn't happen. This log is the single most important artifact for continuity.

3. **Design work follows our heuristics.** Before designing or critiquing any interface, read `design/design-principles.md` (our house rules) and use `design/ux-heuristics.md` as the review checklist. **Any task that writes a user-facing string must first read `design/voice-and-tone.md` and obey it.** No em dashes, no trailing qualifier clauses, no banned vocabulary. For substantial design or redesign work, invoke the **Impeccable** skill, which auto-loads `PRODUCT.md` and `DESIGN.md` from the repo root. See "Design workflow" below.

4. **Keep the knowledge base current.** When a task changes a fact — a resolved open question, a new constraint, a shipped decision — update the owning document in the same change. Move resolved items out of `decisions/open-questions.md`. Stale docs are worse than no docs.

5. **Match the existing structure.** Add to the right folder; don't invent parallel schemes. The map is below.

---

## Where knowledge lives

```
stacks-context/
├── CLAUDE.md                     ← you are here. Rules + orientation for every agent.
├── README.md                     Human-facing start point and glossary.
├── PRODUCT.md                    Impeccable context (required): register, users, brand, anti-refs, principles.
├── DESIGN.md                     Impeccable context (visual): color, type, elevation, components. SEED scaffold.
│
├── product/
│   ├── product-brief.md          Vision, mission, personas, metrics, the wedge.
│   ├── prd.md                    High-level requirements, feature areas, POC scope, build sequence.
│   ├── journeys-and-screens.md   Journey maps, screen inventory, screen flow, prototype order.
│   ├── information-architecture.md  Navigation model, screen hierarchy, and IA rationale.
│   ├── competitive-analysis.md   The landscape, and where Stacks sits in it.
│   └── wireframe-demo-prompt.md  The prompt used to generate the IA wireframe prototype.
│
├── engineering/
│   ├── platform-and-stack.md     iOS-first, React Native + Expo, Supabase, RLS as the sharing mechanism.
│   ├── data-model.md             Tables; the editions-vs-copies modeling call.
│   ├── book-data-and-apis.md     Metadata APIs, the resolver, caching strategy.
│   └── cataloging-methods.md     Barcode scan, AI shelf scan, OCR, import.
│
├── circulation/
│   └── copy-state-model.md       The lending state machine: states, transitions, state-by-role grid.
│
├── design/
│   ├── design-decision-log.md    Running log of every major product/UX design decision. APPEND HERE.
│   ├── design-principles.md      House design heuristics (distilled) + how to use Impeccable.
│   ├── voice-and-tone.md         UX writing ruleset. Sentence rules, banned words, before/afters.
│   ├── component-spec.md         Component inventory with states, anatomy, and behavior.
│   ├── ux-heuristics.md          The review checklist to audit any screen against.
│   └── sitemap-draft.html        Visual sitemap. Open in a browser.
│
├── prototypes/
│   └── stacks-ia-wireframe.html  Clickable IA wireframe. Open in a browser.
│
└── decisions/
    ├── decision-log.md           Every product/eng/circulation decision, with rationale and status.
    └── open-questions.md         What's unresolved, and what's deliberately deferred.
```

**Fastest way to get current:** read `decisions/decision-log.md` and `design/design-decision-log.md`. Between them they hold the reasoning that specs omit.

---

## Design workflow (how the rules and Impeccable fit together)

For any screen, flow, component, or visual work:

1. **Orient.** Read `design/design-principles.md` and the relevant product docs (`product/journeys-and-screens.md`, and `circulation/copy-state-model.md` for anything touching a book card or a lending state).
2. **Check prior decisions.** Scan `design/design-decision-log.md` for anything that already constrains this surface.
3. **Design.** For non-trivial work, invoke **Impeccable** (`/impeccable` — `shape`, `craft`, `audit`, `critique`, `polish`, etc.). It reads `PRODUCT.md` (required) and `DESIGN.md` (visual system) from the root automatically, so output stays on-brand and consistent across tools.
4. **Review.** Run the screen against `design/ux-heuristics.md`.
5. **Log.** Append every consequential decision to `design/design-decision-log.md`. This step is not optional.

`PRODUCT.md` and `DESIGN.md` are the contract between this repo and the Impeccable skill. Keep them accurate — when the brand, principles, or visual system evolve, update these two files so every future design task inherits the change.

---

## Core concepts (glossary)

- **Edition** — a specific published version of a book. Canonical, shared metadata (ISBN, title, authors, cover, publisher, year). Cached from external APIs. Carries no lending state.
- **Copy** — one person's physical book of an edition. Carries location, condition, visibility, and a lending state. This is the object that gets lent.
- **Steward** — an owner who catalogs and lends. Supply side, anchor user.
- **Borrower** — a connected neighbor who browses and borrows. Most people are both.
- **Connection** — an approved link between two users. Sharing is trust-network first.
- **Copy state** — where a copy sits in the lending lifecycle: available, requested, checked out, return pending, overdue, unavailable. Defined fully in `circulation/copy-state-model.md`.

---

## Current focus

A thin proof of concept proving one full request-to-return cycle between two real users. Scope is in `product/prd.md`. The highest-risk, highest-value design work is the **cataloging flow**, so prototype that first, the circulation loop second, connections and browse third.
