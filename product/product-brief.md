# Product Brief

## The problem

Most books get bought, read once, and sit idle. Meanwhile neighbors buy new copies of things sitting on a shelf two doors down, and rarely know each other well enough to ask. There is no lightweight way to see what is on your neighbors' shelves and borrow it.

## The vision

Stacks turns private bookshelves into a shared neighborhood library. Catalog what you own once, choose exactly who can see it, and lend and borrow within a circle you trust. It is the Little Free Library idea with modern coordination on top: less consumption, more local community, more books in motion.

## The wedge

The catalog has standalone value even if you never lend a thing. It competes with personal cataloging tools like Libib and LibraryThing, so a user gets a payoff immediately, on their own, before any neighbor is involved. That solves the cold-start problem that kills most local networks. The catalog is the Trojan horse. The lending and community layer is the mission.

## Goals

- Cataloging is fast enough that it is not a chore. Target under 5 seconds per book, with a batch flow.
- Owners control precisely who sees their library.
- Lending state is always clear: available, checked out, by whom, since when, due when.
- Connected users can browse each other's shelves and request to borrow in a couple of taps.

## Non-goals for v1

- Not a reading tracker or review social network. Ownership and circulation are the focus, not ratings and recommendations.
- Not resale or e-commerce.
- Not a fully open public marketplace at launch. Trust-network first, open discovery later.
- Books only at launch. No games, movies, or music.

## Personas

**The Steward.** Has 100 to 500 books, wants them catalogued, and is willing to lend inside a trusted circle. The anchor user and the driver of supply. Cares that lending is tracked and that books come back. Success for them is a catalogued shelf and low ongoing effort to keep it current.

**The Borrower.** Wants to read without buying. Browses connected libraries, requests loans, returns them. Success for them is discovering something good on a shelf they trust and borrowing it without friction or awkwardness.

Most real users are both. The app is one shell with modes, not two separate experiences.

## Success metrics (post-POC)

- **Activation:** percent of new users who catalog at least 10 books in the first session.
- **Cataloging speed:** median seconds per book added.
- **Connection density:** accepted connections per active user.
- **Circulation:** loans initiated and loans completed.
- **Retention signal:** repeat borrow rate.
