# One-shot prompt — Stacks clickable wireframe demo

*Paste this into Claude Design. Attach `information-architecture.md` and `circulation/copy-state-model.md` as context.*

---

Build a **clickable, low-fidelity wireframe prototype** of a mobile app called **Stacks** — a neighborhood book-lending app where people catalog the books they own and lend them to connected neighbors. I've attached the information architecture doc (the structural source of truth — follow its screen IDs and navigation model exactly) and the copy-state model (how every book card behaves). Use them as the spec.

**Goal:** I want to *feel how the IA comes together* and click through the core flow before investing in visual design. This is a structure-and-navigation test, not a visual-design pass.

**Fidelity — keep it deliberately low.**
- Grayscale / wireframe only: boxes, lines, placeholder rectangles for book covers, simple system font. No brand colors, no imagery, no polish.
- The *one* exception: lending-state badges may use a single muted accent so states are legible — but always pair color with a text label or icon, never color alone.
- Mobile frame, single column, ~390pt wide, thumb-reachable primary actions.

**Navigation model (from the IA doc, §1):**
- Three-tab bottom bar: **Library · Browse · Profile**. Library is the home/default.
- Three global tools available from a top app bar on every screen, NOT tabs: **＋ Add/Scan**, **🔔 Notifications (bell with a badge count)**, **🔍 Search**.
- Start the prototype in a **populated Library / My Shelf** — skip all onboarding (no welcome, auth, or profile setup).

**Screens to include (use the IA doc's IDs):**
- **L1 My Shelf** — a populated grid of ~10 owned books, each card showing its copy state. A status filter chip row: All / Available / On loan / Unavailable. Segmented control at top: *My Shelf* | *Borrowed*.
- **L11 Borrowed** — 2–3 books held from neighbors, with due dates and a Return action.
- **L10 Owner book detail** — one owned copy: metadata block, current copy state, who has it / due date if out, borrow-history list, and owner actions (approve/decline when requested, checkout, confirm/dispute return, mark unavailable).
- **L3 Add Books method picker** — a sheet with Scan / Search / Manual (can be a stub; scan opens a placeholder camera screen). Reached from ＋.
- **B1 Browse home** → **B2 A connected library** → **B3 Borrower book detail** with a "Request to borrow" button.
- **N1 Notification center** — the bell opens an in-app inbox listing pending items for both roles (incoming borrow request, return to confirm, connection request, approval outcome, overdue flag). Each row deep-links to the relevant detail screen.
- **P1 Connections** and **P2 Invite** (Invite can be a simple share stub).

**The one flow that must be fully clickable — the circulation loop (both roles, one prototype):**
1. In **Browse (B3)**, tap **Request to borrow** on an available book → confirmation, copy flips to *Requested*.
2. The **bell** badge increments; open **N1** → tap the incoming request → lands on **L10 Owner book detail** showing "Requested by [name]."
3. **Approve** → set an optional due date (a sheet; "no due date" is a first-class option) → **Checkout**. Copy → *Checked out*.
4. That copy now appears under **My Shelf → On loan filter**, and in the borrower's **Borrowed (L11)**.
5. In **Borrowed**, tap **Mark returned** → copy → *Return pending*.
6. Bell increments again → **N1** → tap "return to confirm" → **L10** → **Confirm returned** (copy → *Available*) OR **Didn't receive it** (bounces back to *Checked out*). Include both buttons.

**Copy-state rules (from the attached copy-state model — apply on every card):**
- Six states: Available, Requested, Checked out, Return pending, Overdue, Unavailable. Each gets a distinct labeled badge.
- Labels are role-specific: the requester sees "Request pending," any other neighbor sees "On hold" (no name). The owner sees "Requested by [name]."
- Populate the demo shelf with a mix of states so the vocabulary is visible at a glance (e.g. several Available, one Checked out, one Unavailable, one Overdue).

**Interaction requirements:**
- Every tab, the bell, and ＋ are tappable and route correctly on every screen.
- Book cards open their detail screen. Back navigation works.
- State transitions in the loop above actually update the UI (badge counts, card states, which list a book appears in).
- Include empty and populated states where relevant, but default everything to populated.

**Deliverable:** a single self-contained clickable prototype (screens linked, navigable on a phone frame). Prioritize that the *navigation and the circulation loop work end to end* over any visual refinement. Where the IA doc and this prompt differ, follow the IA doc.
