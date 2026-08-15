# Competitive Analysis

*Last updated: 2026-08-04. Owner: product. Status: living document — revisit when we hear of a new entrant or a competitor ships something that touches our wedge.*

This document maps the landscape Stacks competes in, what is working and what is not for each category, and where the opening is. The short version: the two things Stacks bundles — a fast personal catalog and trusted local lending — exist separately in mature form, but nobody has combined them well, and every pure "borrow from neighbors" app to date has died or stayed tiny on the cold-start problem. That gap is the whole thesis, and this analysis backs it up.

---

## How to read the field

The market splits into four buckets. Only one of them (personal cataloging) has durable, well-run incumbents. The rest are either physical-world movements without real software, or a graveyard of underpowered peer-lending apps.

1. **Personal cataloging tools** — where you log what you own. Mature, sticky, and exactly the "wedge" Stacks has to win first. Libib, LibraryThing.
2. **Reading social networks** — where you track what you've read and what to read next. Huge, but a different job. Goodreads, StoryGraph, Fable, Hardcover.
3. **Peer-to-peer lending / swap apps** — where you lend or trade with others. Directly on our mission. Mostly small, mostly struggling, several dead. Libro, Shelfable, NeighborBook, Libraco, Lenro (defunct), PaperBackSwap, BookMooch.
4. **Physical / civic book sharing** — the non-software version of our mission. Little Free Library, community fridges-for-books, library systems (Libby). Proof the behavior is real; not a software competitor.

Stacks is the only entrant deliberately sitting across buckets 1 and 3 at once, and using bucket 1 to solve the failure that kills bucket 3.

---

## Bucket 1 — Personal cataloging (the wedge we must win)

This is where Stacks earns standalone value and beats the cold-start problem. To win here the catalog has to be at least as fast and pleasant as the incumbents, or the Trojan horse never gets through the gate.

**Libib.** The closest thing to a category default for home cataloging. Free tier holds up to 5,000 items and covers books, movies, music, games; scan-by-barcode, ISBN/UPC lookup, CSV import, tags, notes, multiple libraries. The paid tier (Libib Pro, ~$99/year) adds real circulation and patron management aimed at small institutional libraries, schools, and classrooms.

- *Working:* fast multi-format cataloging, generous free tier, clean mobile scanning, and — notably — a lending/circulation layer already exists. It is the most feature-complete answer to "catalog my shelf."
- *Not working:* the lending features are built for an institution lending to patrons (a librarian model), not for two neighbors who trust each other. There is no social graph, no neighbor discovery, no trust network. It is a catalog with a checkout desk bolted on, not a community.

**LibraryThing.** The connoisseur's tool. Twenty years of bibliographic depth, handles rare books, private-press editions, and cataloging edge cases nobody else touches. Recommendation engine works off shared-library overlap rather than trending titles. Pricing: free up to 200 books, then a one-time ~$25 lifetime membership (it briefly dropped all limits in 2020, then reintroduced the free-tier cap).

- *Working:* unmatched metadata quality and a loyal, high-intent user base. The lifetime-price model signals "own this forever," which is the right emotional register for a personal library.
- *Not working:* the interface is widely called dated and archaic; the 200-book free ceiling pushes casual users away; and it is fundamentally a solo cataloging + light-social experience, not a lending coordinator. No local, no circulation.

**What this means for Stacks:** the bar for the catalog is high and already met by others, so "another catalog" is not a business. Our catalog has to be *faster* (the under-5-seconds-per-book, batch-first goal is the right wedge) and it has to be the on-ramp to something these tools structurally can't offer: lending inside a trusted local circle. Speed and the batch/AI-shelf-scan flows are where we out-execute Libib on its home turf.

---

## Bucket 2 — Reading social networks (adjacent, not our fight)

Named here so we don't accidentally drift into them — the product brief's non-goals already rule this out, and the competitive reality reinforces why.

**Goodreads** (~90M users, Amazon-owned) owns reading-tracking and reviews by sheer scale, despite a famously dated UI. **StoryGraph** (launched 2020) is the credible modern challenger, winning defectors with mood/pace tagging and strong reading stats. **Fable** and **Hardcover** are newer social-first entrants.

- *Working:* massive network effects (Goodreads), genuinely better UX and recommendation intelligence (StoryGraph). These are proven, well-funded categories.
- *Not working / irrelevant to us:* they track *reading*, not *ownership or circulation*. None knows which physical copy you own or where it physically is, and none lets a neighbor borrow it. Competing here means fighting Amazon's install base for a job Stacks explicitly isn't doing.

**What this means for Stacks:** stay out. Ownership + circulation is our lane; ratings/recommendations/reviews are a swamp with an 800-pound incumbent. If we ever want reading data, integrate or import — don't rebuild Goodreads.

---

## Bucket 3 — Peer-to-peer lending & swap (our mission — and a graveyard)

This is the category Stacks is really trying to crack, and the honest read is that it is littered with attempts that never reached escape velocity. That is not a reason to avoid it; it is the reason the opportunity is still open. But we must learn precisely why they stalled.

**The direct neighbor-lending apps — Libro, Shelfable, NeighborBook, Libraco.** All pitch nearly the same one-liner Stacks does: list your books, discover what neighbors have, borrow locally. Libro is the most developed — it leans hard on trust and safety (verified profiles, phone/email verification, an earned-reputation "gems" system, only verified users can request). Shelfable and NeighborBook and Libraco are thinner, earlier, or lightly maintained.

- *Working:* they've correctly identified that **trust and safety is the core problem**, not cataloging. Libro's verification-and-reputation model is the right instinct. The value proposition resonates — "a library's worth of books steps from your door" is a good story.
- *Not working:* the fatal flaw is **cold start / empty shelves**. A lending app is worthless until there's local density, and you can't get density without users, and users won't stay for an empty map. None of these leads with standalone single-player value, so a new user opens the app, sees nothing nearby, and churns. They also make the user do cataloging *work* with no payoff until a neighbor happens to appear. Discovery is location-radius based (strangers nearby), which raises the trust bar right when the network is thinnest.

**Lenro (defunct).** A location-based lend/borrow/exchange network with social-media verification and "karma points" gamification. Now inactive. A clean cautionary tale: right idea (local, trust-verified, gamified), killed by the same cold-start dynamic. Gamification doesn't manufacture liquidity.

**PaperBackSwap & BookMooch (mail-based swap networks, ~2004-era).** Not local and not lending — you mail a book away permanently in exchange for credits to receive others. Both technically still alive, both with fluctuating, aging activity; BookMooch users report wishlists going unfilled for long stretches.

- *Working:* a real credit-economy that solved matching at national scale in its day; durable enough to still exist 20 years on.
- *Not working:* it's *swapping*, not *lending* — you give the book up. Mailing friction, aging user base, dated experience, and no local/community warmth. It solves a different, more transactional need.

**What this means for Stacks:** the mission-layer competitors validate the demand and have mostly diagnosed the trust problem correctly — but every one of them dies on cold start because they are lending-first. Stacks' entire strategic bet is the inversion: **catalog-first, lending-second.** The catalog gives a solo user immediate payoff (Libib-grade value with nobody else present), which lets a user base accumulate *before* the network needs to be dense. By the time lending matters, the shelves aren't empty. This is the single most important differentiator, and the field proves it's the one nobody else has executed.

---

## Bucket 4 — Physical & civic sharing (proof the behavior is real)

**Little Free Library** — the ~180,000-box global movement of front-yard book boxes — is the clearest evidence that people *want* to share books with neighbors and will build physical infrastructure to do it. Its app just helps you find boxes; there's no cataloging, no ownership, no return — books flow anonymously and permanently. **Public libraries / Libby** own free, legit borrowing at scale, but institutionally, for e-books/audiobooks, with holds and wait times, and nothing to do with your neighbor's shelf.

- *Working:* Little Free Library proves the emotional and community demand cold. Libby proves people are fluent in a borrow-and-return loop.
- *Not working / gap:* Little Free Library has zero coordination — you can't see what's in a box, reserve it, or get a specific book back; it's serendipity only. Libby is impersonal and institutional. Neither connects *your* books to *your* neighbors with knowledge of who has what.

**What this means for Stacks:** the demand is validated by a movement with six figures of physical installations. Stacks is "Little Free Library with a catalog and a memory" — the coordination layer that box-on-a-post sharing structurally lacks, scoped to a trusted circle rather than anonymous strangers.

---

## Where Stacks differentiates (the defensible positions)

1. **Catalog-first sequencing beats the cold-start death that killed every lending-first app.** This is the core wedge and the thing the whole field gets wrong. Standalone catalog value means a user is happy on day one, alone; lending liquidity accrues quietly underneath. Libro/Shelfable/Lenro all inverted this and stalled.

2. **Trust-network-first, not radius-first.** The direct competitors discover strangers by location, which raises the trust barrier exactly when the network is weakest and safety fears are highest. Stacks lends inside approved connections (RLS-enforced), so the first lending experience is with someone you already trust — warmer, safer, and higher-converting than "a stranger 0.4 miles away." This also sidesteps the verification-arms-race that Libro has to run.

3. **Real, clear circulation state — the thing casual apps skip and institutional tools over-build.** Libib's circulation is librarian-grade and impersonal; the neighbor apps' is thin. Stacks' explicit copy-state model (available / requested / checked out / return pending / overdue) sized for peer lending — "who has it, since when, due when," always legible — is a genuine middle-ground nobody occupies well. Stewards lend and *trust the book comes back* because the state is unambiguous.

4. **The editions-vs-copies data model** (canonical shared metadata separated from an individual's physical, stateful copy) is the correct architecture for local lending and is not something the cataloging incumbents need or the lending apps have bothered to build. It's quiet infrastructure that makes both the fast catalog and clean circulation possible.

5. **Cataloging speed as a moat.** If Stacks hits sub-5-second, batch-first, AI-shelf-scan cataloging, it out-executes Libib on the exact axis that determines whether the wedge converts. Fast catalog → more users → eventual lending density. Speed is upstream of everything.

---

## Where we're exposed (risks to respect)

- **Libib could add a social/neighbor layer.** It already has cataloging *and* circulation; a trust graph is the missing piece, and they have the userbase to bolt it on. Our defense is depth on local + trust-network UX and community, not features they can copy in a quarter.
- **Cold start is only *mitigated*, not solved.** Catalog-first means a lone user gets value, but the *mission* (lending) still needs local density. We're betting that density accrues passively while people catalog. If catalogers never flip into lenders, we're just a slower Libib. Watch the catalog→lend conversion metric like a hawk.
- **Trust & safety is unavoidable and expensive.** The moment strangers-adjacent lending appears, we inherit Libro's whole verification/reputation burden. Staying strictly inside approved connections is what keeps this tractable early — loosening it later is where risk spikes.
- **Two-sided value takes real neighbor density to feel magical.** Little Free Library proves the desire, but a box-on-a-post has zero onboarding cost; an app has to earn every install. Geographic launch density (one neighborhood at a time) matters more than raw signups.
- **Monetization is unproven in this niche.** Libib charges institutions; LibraryThing charges a tiny lifetime fee; the neighbor apps largely haven't figured revenue out at all. Whatever the model, it can't tax the lending behavior we're trying to encourage.

---

## One-line competitive summary

Mature catalogs (Libib, LibraryThing) that can't lend to neighbors; a graveyard of neighbor-lending apps (Libro, Shelfable, Lenro) that die on empty shelves because they lead with lending; and a six-figure physical movement (Little Free Library) that proves the demand but has no coordination. Stacks is the only one using a genuinely fast, standalone-valuable **catalog** to earn users first, then turning that base into **trusted local circulation** — the exact sequence every prior lending-first attempt got backwards.

---

## Sources

- Libib features & pricing: [Libib Pricing](https://www.libib.com/pricing), [Capterra](https://www.capterra.com/p/207388/Libib/), [G2](https://www.g2.com/products/libib/reviews)
- LibraryThing: [BookWise review](https://bookwiseapp.com/blog/librarything-review-is-it-still-the-best-book-cataloging-tool), [LibraryThing goes free (2020)](https://blog.librarything.com/2020/03/librarything-goes-free/), [Goodreads vs LibraryThing](https://bookwiseapp.com/blog/goodreads-vs-librarything)
- Goodreads / StoryGraph / alternatives: [The Gilt List](https://thegiltlist.com/best-goodreads-alternatives-2026/), [LibraryThing vs StoryGraph](https://booksuggest.xyz/librarything-vs-storygraph-comparison-2025/)
- Neighbor-lending apps: [Libro](https://readwithlibro.app/), [Shelfable](https://shelfable.app/), [NeighborBook](https://neighborbook.org/), [Libraco](https://www.libraco.app/), [TrendHunter: Community Borrowing Apps](https://www.trendhunter.com/trends/community-borrowing-apps)
- Lenro (defunct): [Wikipedia](https://en.wikipedia.org/wiki/Lenro), [The Better India](https://thebetterindia.com/46198/lenro-books-exchange-lend-borrow-delhi/)
- Swap networks: [PaperBackSwap (Wikipedia)](https://en.wikipedia.org/wiki/PaperBackSwap), [BookMooch (Wikipedia)](https://en.wikipedia.org/wiki/BookMooch)
- Physical / civic: [Little Free Library](https://littlefreelibrary.org/start/), [Libby](https://apps.apple.com/us/app/libby-the-library-app/id1076402606)
- Cold-start dynamics: [Cold start in P2P networks (arXiv)](https://arxiv.org/pdf/0912.0985)
