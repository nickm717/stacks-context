# Decision Log

A running record of decisions made, with rationale and status. Newest context first within each area. Many of these were settled in conversation and would otherwise not appear in any spec, which is the main reason this log exists.

Status key: **Accepted** (settled), **Recommended** (leading option, not yet confirmed), **Deferred** (deliberately postponed).

---

## Product

**Concept and wedge.** *Accepted.* The catalog is standalone-useful (competes with Libib and LibraryThing) and is the wedge that solves cold-start. Lending and community is the mission on top. Onboarding and messaging should lead with the neighborhood-library idea, not a feature list.

**Catalog-first sequencing, validated against the field.** *Accepted.* A competitive scan (2026-08-04, see `product/competitive-analysis.md`) confirms the wedge is the differentiator, not just a go-to-market convenience. Mature cataloging incumbents (Libib, LibraryThing) can't lend to neighbors; every neighbor-lending app reviewed (Libro, Shelfable, NeighborBook, Libraco, and the now-defunct Lenro) leads with lending and stalls or dies on cold start — a new user opens an empty local map and churns. Little Free Library's six-figure physical footprint proves the demand is real but has no coordination layer. Stacks is the only entrant using a genuinely fast, standalone-useful catalog to accumulate users *before* the network needs density, then converting that base into trusted local circulation. Consequence: the catalog→lend conversion rate is the metric that decides whether this bet pays off; if catalogers never become lenders we are only a slower Libib, so cataloging speed (sub-5s, batch-first) is treated as upstream of everything and must beat Libib on its home turf. Open risk logged: Libib already has cataloging plus circulation and could bolt on a trust graph — our defense is depth on local + trust-network UX, not copyable features.

**Trust-network first.** *Accepted.* Launch closed: you see the libraries of people you are connected to. Open neighborhood discovery comes after density exists. This matches the "share with select users" requirement and sidesteps most trust-and-safety and cold-start problems for the POC.

**Books only at launch.** *Accepted.* No other media in v1.

**Working name "Stacks."** *Recommended.* Placeholder, open to change.

---

## Platform and stack

**Repo structure: two repos now, a third if web ever happens.** *Accepted (2026-08-16).* `stacks-context` stays docs-only: specs, PRDs, decisions, design rationale, and the `CLAUDE.md` / `PRODUCT.md` / `DESIGN.md` operating context. App code lives in a new `stacks-ios` repo, created alongside this decision. A `stacks-web` repo follows the same pattern if a companion web surface is ever built (see "Hosting" below, Vercel is the candidate). Rationale: keeps docs history clean of code churn (commits, PRs, CI noise), keeps `Linked doc` URLs into specs stable no matter how the app repo gets refactored, and still lets every repo's agent load the same `CLAUDE.md` rules and decision logs as a lightweight read reference. Tradeoff accepted: a coding agent working in `stacks-ios` has to be pointed at `stacks-context` explicitly rather than finding specs in the same tree; the `Linked doc` convention in Notion's How This Works page now specifies which repo per card.

**iOS first.** *Accepted.* Native scanning quality (VisionKit, on-device Vision, OCR) and reliable push are the deciding factors, since cataloging is the core value. The tradeoff, cutting out Android neighbors in a density-dependent product, is accepted for the POC and revisited before launch.

**React Native with Expo.** *Recommended.* Keeps TypeScript, gives native camera via `react-native-vision-camera`, ships iOS first with Android as later config-and-test. Alternatives: pure Swift (only to push on-device shelf-scan quality), or a web PWA (only to validate the lending loop fastest). Supabase is the backend regardless, so this is not blocking.

**Supabase backend.** *Accepted.* Postgres, Auth, Storage, Realtime, and row-level security. RLS is the sharing mechanism: visibility lives in the database, not app code.

---

## Book data

**Google Books plus Open Library, merged, free.** *Accepted.* Complementary coverage, no key needed. Open Library treated as the canonical, openly-licensed cache. Google Books fills gaps.

**ISBNdb as paid fallback.** *Deferred.* Roughly $15 to $300 per month. Only if free coverage proves insufficient. Not for the POC.

**Do not use Goodreads.** *Accepted.* API deprecated years ago, no ownership or lending model.

**Read-through caching into own DB.** *Accepted.* Resolve once per edition, then serve from Postgres. Standard production pattern. Protects against rate limits and latency and gives schema control.

**Image discipline over metadata bloat.** *Accepted.* Edition text dedupes cheaply. Cover images are the real cost. Store URLs and hot-link or proxy by default, only pull into Storage when durability is needed, and then CDN plus resize. Build a refresh-and-correct path for wrong editions and dead cover URLs.

---

## Data model

**Editions vs copies.** *Accepted.* `book_editions` is shared canonical metadata with no lending state. `copies` are per-user physical books that carry state and get lent. This separation keeps metadata clean and makes "who has this title" a simple query.

---

## Circulation

**Approve and hand off are two steps.** *Accepted (2026-08-09).* Approving a request now moves the copy to a *ready for pickup* state (reserved, off-market) rather than straight to checked out; a separate "mark as handed over" starts the loan and the due-date clock. Keeps due dates honest (clock starts at actual handoff) and confirms approval to the borrower before the two meet. Handoff coordination itself stays out of app for the POC. **Supersedes** the earlier implicit "approve → checked out in one step" behavior baked into the first copy-state model.

**Owner-initiated direct lend.** *Accepted (2026-08-09).* A "Lend" action on any available copy checks it out directly with no request — the everyday "here, take it" case. Skips requested/ready, lands in checked out. Reinforces the tracking-value wedge (know who has your book without waiting on a request).

**Off-app borrowers.** *Accepted (2026-08-09).* A direct-lend borrower may be someone not on Stacks, recorded as a free-text name. Off-app loans have no borrower-side app, so the owner marks the return directly (no return-pending step). Rationale: cold-start reality — most early lending is to people not yet on the app, and tracking must not require the borrower to have an account.

**Owner authoritative on returns, borrower can initiate.** *Accepted.* Borrower marks returned, copy enters return pending, owner confirms. Owner can dispute, which bounces it back to checked out.

**Unavailable state, visible.** *Accepted.* Owner can flag a copy off-market (reading it, or keeping it). It stays visible to neighbors but is not borrowable.

**Overdue included, no nudge.** *Accepted.* Overdue renders as a derived flag on checked out. No reminders or automation in the POC.

**Optional due date.** *Accepted.* Owners can approve a loan with no due date. Such loans stay open-ended and never go overdue. The loans view renders both variants.

**One request per copy, request locks the copy.** *Accepted.* A request flips the copy to requested and off-market. No other request can open until the owner approves or declines. Removes all queue and concurrency logic.

**Decline has no reason, and prompts a next state.** *Accepted.* Borrower sees only that it was declined. The owner, on declining, chooses available or unavailable for the copy.

**Request labels are role-specific.** *Accepted.* Requester sees "Request pending." Third-party neighbors see "On hold" with no requester name.

---

## Design process

**Prototype cataloging first.** *Accepted.* Highest-risk, highest-magic interaction. Batch-scan feel decides onboarding completion. The circulation loop is prototyped second, connections and browse third.
