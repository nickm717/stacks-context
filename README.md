# Stacks: Project Knowledge Base

**Stacks** turns private bookshelves into a shared neighborhood library. You catalog what you own once, choose exactly who can see it, and lend and borrow within a circle you trust. Less consumption, more local community, more books in motion. The personal catalog is the standalone-useful wedge. The lending and community layer is the mission.

*"Stacks" is a working name and can change.*

**Status:** Pre-build. Seeding the knowledge base and preparing a proof of concept. No code yet.

---

## How this knowledge base is organized

```
stacks-context/
├── CLAUDE.md                          Operating rules for any AI agent (Claude Code, Design, Cowork). Read first.
├── README.md                          You are here. Human start point and glossary.
├── PRODUCT.md                         Impeccable context (required): register, users, brand, anti-refs, principles.
├── DESIGN.md                          Impeccable context (visual): color, type, elevation, components. SEED scaffold.
│
├── product/
│   ├── product-brief.md               Vision, mission, personas, metrics, the wedge.
│   ├── prd.md                         High-level requirements, feature areas, POC scope, build sequence.
│   ├── journeys-and-screens.md        Journey maps, screen inventory, screen flow, prototype order.
│   ├── information-architecture.md    Navigation model, screen hierarchy, and IA rationale.
│   ├── competitive-analysis.md        The landscape, and where Stacks sits in it.
│   └── wireframe-demo-prompt.md       The prompt used to generate the IA wireframe prototype.
├── engineering/
│   ├── platform-and-stack.md          iOS-first decision, stack, hosting, RLS as the sharing mechanism.
│   ├── data-model.md                  Tables, and the editions-vs-copies modeling call.
│   ├── book-data-and-apis.md          Metadata APIs, the resolver, and the caching strategy.
│   └── cataloging-methods.md          Barcode scan, AI shelf scan, OCR, import.
├── circulation/
│   └── copy-state-model.md            The lending state machine. States, transitions, state-by-role grid.
├── design/
│   ├── design-decision-log.md         Running log of every major product/UX design decision. APPEND HERE.
│   ├── design-principles.md           House design heuristics (distilled) + how to use Impeccable.
│   ├── voice-and-tone.md              UX writing ruleset. Sentence rules, banned words, before/afters.
│   ├── component-spec.md              Component inventory with states, anatomy, and behavior.
│   ├── ux-heuristics.md               The review checklist to audit any screen against.
│   └── sitemap-draft.html             Visual sitemap. Open in a browser.
├── prototypes/
│   └── stacks-ia-wireframe.html       Clickable IA wireframe. Open in a browser.
└── decisions/
    ├── decision-log.md                Every product/eng/circulation decision, with rationale and status.
    └── open-questions.md              What is still unresolved, and what is deliberately deferred.
```

## How to use it

- **First, if you're an AI agent:** read `CLAUDE.md`. It carries the operating rules and travels with the repo, so Claude Code, Claude Design, and Cowork all follow the same conventions.
- **For planning and building:** the two decision logs are the fastest way to get current. `decisions/decision-log.md` covers product/engineering/circulation; `design/design-decision-log.md` covers product and UX design. Together they capture what was settled and why — including calls made in conversation rather than in a spec.
- **For product context:** start with `product/product-brief.md` (or the condensed `PRODUCT.md`), then `product/prd.md`.
- **For design work:** read `design/design-principles.md`, then `product/journeys-and-screens.md` plus `circulation/copy-state-model.md` for every screen and state to design against. `PRODUCT.md` and `DESIGN.md` are auto-loaded by the Impeccable skill; review against `design/ux-heuristics.md`; log decisions in `design/design-decision-log.md`.
- **For build work:** `engineering/*` plus `circulation/copy-state-model.md`, which maps one to one onto the loan table and its transitions.

## How the rules travel to Claude Code and Claude Design

`CLAUDE.md` is auto-loaded as project context by Claude Code and Cowork; it states the non-negotiables (respect the decision logs, follow the design heuristics, log every major design decision). For design specifically, `PRODUCT.md` and `DESIGN.md` at the repo root are the exact files the **Impeccable** skill reads before it does any work — so invoking Impeccable in any tool inherits this project's strategy and visual system automatically. Keep those two files current and the rest follows.

---

## Key concepts (glossary)

- **Edition.** A specific published version of a book. Canonical metadata (ISBN, title, authors, cover, publisher, year). Shared across all users and cached from external APIs.
- **Copy.** One person's physical book. One edition can have many copies across many owners. This is the object that gets lent, and the thing that carries a lending state.
- **Steward.** An owner who catalogs books and lends them. The supply side and anchor user.
- **Borrower.** A connected neighbor who browses and borrows. Most people are both a steward and a borrower.
- **Connection.** An approved link between two users. Sharing is trust-network first: you see the libraries of people you are connected to.
- **Copy state.** Where a copy sits in the lending lifecycle: available, requested, checked out, return pending, overdue, or unavailable. Defined fully in `circulation/copy-state-model.md`.

---

## Current focus

A thin proof of concept that proves one full request-to-return cycle between two real users. Scope is defined in `product/prd.md`. The highest-risk, highest-value design work is the cataloging flow, so that is what to prototype first.
