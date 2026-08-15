# Data Model

Context: the schema behind Stacks. The one modeling call that matters most is the separation of editions from copies.

---

## The core distinction: editions vs copies

- A **book_edition** is canonical, shared data about a published book, cached from external APIs. Many users reference the same edition. It never carries lending state.
- A **copy** is one user's physical book of that edition. It carries location, condition, visibility, and a lending state. This is the thing that gets lent.

Keeping these separate keeps metadata clean, makes duplicates trivial, and makes "who near me has this title" a simple query: find the edition, then find visible available copies of it.

---

## Tables

- **users.** Profile, approximate location, reputation (later).
- **libraries.** Owned by a user. Has a default visibility.
- **book_editions.** Canonical edition data cached from the APIs: ISBNs, title, authors, cover reference, publisher, year, page count, description. Shared across all users. Written by the resolver, see `book-data-and-apis.md`.
- **copies.** A user's physical copy of an edition. Holds library, location, condition, per-copy visibility, and current lending state.
- **connections.** Edges between users with a status: pending, accepted, blocked.
- **shares.** Grants exposing a library or copy to a specific user or pod, beyond the default connection visibility.
- **loans.** The circulation record: copy, borrower, checked_out_at, due_at (nullable), returned_at, status. Maps directly onto the state machine in `circulation/copy-state-model.md`.
- **pods.** Neighborhood groups, with membership. Post-POC.

---

## How state lives in the data

A copy's current lending state is derived from its loan records plus an owner-set availability flag:

- No open loan and available: **Available**.
- An open request: **Requested** (and the copy is locked, no other request can open).
- An approved active loan: **Checked out**, with `due_at` set or null.
- Checked out with `due_at` in the past: **Overdue** (derived, not stored separately).
- A borrower-submitted return awaiting owner confirmation: **Return pending**.
- Owner has flagged the copy off-market: **Unavailable**.

The `due_at` column is nullable on purpose. Owners can approve a loan with no due date, in which case the loan stays checked out open-ended and never becomes overdue. See the circulation doc for the full transition rules.

---

## Visibility

Sharing is enforced with Postgres row-level security, not application code. A copy is readable by its owner, and by users who have an accepted connection or an explicit share grant. See `platform-and-stack.md`.
