# Open Questions and Deferred Items

What is genuinely unresolved, and what has been deliberately postponed so it is not rediscovered later as a surprise.

---

## Open, needs a decision

- **Framework confirmation.** iOS-first is settled. React Native with Expo is the recommendation, but pure Swift and web PWA are still on the table depending on how far the on-device shelf-scan experience should be pushed versus how fast the lending loop should be validated. See `engineering/platform-and-stack.md`.
- **Seed strategy for local density.** The hardest product problem. Whose network do we start with? Do we partner with existing Little Free Library stewards or a neighborhood community to seed a first pod? Nothing decided.
- **Product name.** "Stacks" is a placeholder.
- **Privacy handling detail.** Approximate location is the principle. The *in-app* representation of handoff is now settled (approve → ready → handed over; see `circulation/copy-state-model.md`), but the specifics of coordinating the physical swap and meet points are still out of app and undesigned.
- **Return authority on marking, third-party edge cases.** The owner-confirms model is set, and off-app borrowers are now handled (owner marks the return directly; see decision-log, "Off-app borrowers"). The exact dispute UX beyond a single "did not receive" bounce is still not fully designed.
- **Sharing granularity.** POC ships one default ("visible to connections"). Whether long-term control is per-library, per-copy, or both is not settled.

---

## Deferred, intentionally out of the POC

- **Auto-expiring requests.** A stale request should eventually release the copy lock back to available. For the POC, a request holds the copy until the owner acts.
- **Notify when available.** Unavailable and checked-out copies stay visible, so a future "tell me when this frees up" fits naturally. Not built.
- **Notifications infrastructure and overdue nudges.** In-app badges only for the POC.
- **Multiple concurrent requests or a request queue.** Explicitly excluded. One request per copy is the rule.
- **Neighborhood pods, map, and open discovery.** The community and scale-up layer.
- **Trust, safety, and reputation scoring.**
- **Search across connected libraries.** Wanted early, but not required to prove the loop.
- **AI shelf scan, printed-ISBN OCR, CSV import.** Cataloging methods beyond barcode and manual, sequenced after the POC.
