# High-Level PRD

For vision, personas, and success metrics, see `product-brief.md`. This document covers what gets built and in what order. Deep dives live in the engineering and circulation docs, referenced inline.

---

## Core user journeys

1. **Onboard and catalog.** Scan barcodes in batch, fall back to AI shelf photo or manual search, confirm results.
2. **Set visibility.** Choose sharing: private, specific connections, or a neighborhood pod.
3. **Connect.** Invite people directly, approve inbound requests, or join a pod.
4. **Browse and request.** Look through a connected library, request to borrow a specific copy.
5. **Lend.** Owner approves, marks checked out, tracks it, confirms return.
6. **Nudge.** Gentle overdue visibility.

Full journey maps and the screen inventory are in `journeys-and-screens.md`.

---

## Feature areas

Each becomes its own feature-level PRD.

- **A. Cataloging and metadata ingestion.** Barcode scan, AI shelf scan, printed-ISBN OCR, manual search, CSV import. See `engineering/cataloging-methods.md` and `engineering/book-data-and-apis.md`.
- **B. Library management.** Shelves, physical location tags, status, condition, custom tags, search within your own library.
- **C. Identity, connections, and sharing.** Auth, invites, connection requests, visibility permissions. Enforced with Postgres row-level security, see `engineering/platform-and-stack.md`.
- **D. Discovery and search across connected libraries.** Find a title across everyone you are connected to, see who has it and whether it is available.
- **E. Circulation.** The lending, checkout, return, and history engine. Fully specified in `circulation/copy-state-model.md`.
- **F. Notifications.** Requests, approvals, due dates, returns. Deferred past the POC.
- **G. Trust, safety, and privacy.** Identity confidence, address handling, meet points, reputation. Deferred past the POC.
- **H. Community layer.** Neighborhood pods, local density, optional map, activity.

---

## Key product risks

1. **Cold-start and local density.** A local sharing network is worthless without neighbors on it. Mitigated by the standalone catalog value, then seeding density through invite-only pods. Seed strategy is still open, see `decisions/open-questions.md`.
2. **Trust model.** Launch closed. You see the libraries of people you are connected to. Layer open neighborhood discovery once density exists. This matches the "share with select users" requirement.
3. **Privacy.** Approximate location only. Coordinate exact handoffs privately. Never expose a precise address by default.
4. **Wedge clarity.** Catalog is the hook, lending is the mission. This shapes onboarding and messaging.

---

## POC scope

The thinnest build that proves the core loop end to end.

**In scope**
- Supabase auth.
- Add books by barcode scan and manual search, resolved through Google Books and Open Library, with cover art.
- Personal library view (grid and list) with a status field.
- Invite and connect one other user, share a library read-only via row-level security.
- The full loop: request, approve, check out, borrower-initiated return, owner confirmation.
- Deploy to a real device.

**Explicitly out of the POC**
- AI shelf scan, printed-ISBN OCR, CSV import.
- Neighborhood pods and map.
- Notifications infrastructure and overdue automation (overdue is shown as a flag, but not nudged).
- Reputation and trust scoring.

Proving the request-to-return loop between two real users is the milestone that de-risks everything else.

---

## Suggested feature-PRD sequence

1. **Cataloging and metadata ingestion.** The resolver, barcode scan, manual add, confirm flow. The heart of the POC.
2. **Library management and status.** Copies, locations, statuses.
3. **Identity, connections, and sharing.** Auth, invites, visibility rules.
4. **Circulation.** The lending state machine.
5. **Discovery and search across libraries.**
6. **AI shelf scan.** The differentiator.
7. **Notifications.**
8. **Community and pods, plus trust and safety.** The scale-up layer.
