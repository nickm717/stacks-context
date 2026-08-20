# Open Questions and Deferred Items

What is genuinely unresolved, and what has been deliberately postponed so it is not rediscovered later as a surprise.

---

## Open, needs a decision

- **Framework confirmation.** iOS-first is settled. React Native with Expo is the recommendation, but pure Swift and web PWA are still on the table depending on how far the on-device shelf-scan experience should be pushed versus how fast the lending loop should be validated. See `engineering/platform-and-stack.md`.
- **Seed strategy for local density.** The hardest product problem. Whose network do we start with? Do we partner with existing Little Free Library stewards or a neighborhood community to seed a first pod? Nothing decided.
- **Product name.** "Stacks" is a placeholder.
- **Privacy handling detail.** Approximate location is the principle. The *in-app* representation of handoff is now settled (approve → ready → handed over; see `circulation/copy-state-model.md`), but the specifics of coordinating the physical swap and meet points are still out of app and undesigned.
- **Return authority on marking, third-party edge cases.** The owner-confirms model is set, and off-app borrowers are now handled (owner marks the return directly; see decision-log, "Off-app borrowers"). The exact dispute UX beyond a single "did not receive" bounce is still not fully designed.
- **Sharing granularity.** POC ships one default ("visible to connections"). Whether long-term control is per-library, per-copy, or both is not settled. (2026-08-17) Refined into a three-tier shape to design toward: public by default, searchable-then-request, or private/invite-only per user. Conflicts directly with trust-network-first (below), so the two need to be resolved together, not independently.
- **Hyperlocal search + trust/verification model, flagged as a near-term scale priority (2026-08-17).** Discussed as the fast-follow after the POC, not a someday item: search across nearby libraries without requiring a prior connection, gated by a trust layer that makes stranger-adjacent lending safe enough to open up. Needs its own design pass covering identity verification (phone/email at minimum, possibly ID), a reputation/rating system (completed loans, on-time returns, endorsements from mutual connections), and how "immediate area" gets defined and queried (radius/geohash search, precision traded against the approximate-location principle already set for privacy). `competitive-analysis.md` flags Libro's verification-and-reputation "gems" model as the right instinct among direct competitors, and warns this is also where the verification-arms-race cost shows up — worth studying closely before designing our version. Supersedes treating "trust, safety, and reputation scoring" as an indefinitely-deferred item below; it's now the named unlock for widening past the connections-only graph.

---

## Deferred, intentionally out of the POC

- **Auto-expiring requests.** A stale request should eventually release the copy lock back to available. For the POC, a request holds the copy until the owner acts.
- **Notify when available.** Unavailable and checked-out copies stay visible, so a future "tell me when this frees up" fits naturally. Not built.
- **Notifications infrastructure and overdue nudges.** In-app badges only for the POC.
- **Multiple concurrent requests or a request queue.** Explicitly excluded. One request per copy is the rule.
- **Neighborhood pods, map, and open discovery.** The community and scale-up layer. See hyperlocal search + trust model above — this is where it lands once that's designed.
- **Search across connected libraries.** Wanted early, but not required to prove the loop.
- **AI shelf scan, printed-ISBN OCR, CSV import.** Cataloging methods beyond barcode and manual, sequenced after the POC.
