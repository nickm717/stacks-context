# Platform and Stack

Context: Stacks is a mobile-first local book-lending app. Cataloging quality and scanning speed are the core value, so the platform choice is weighted toward the camera and scanning experience.

---

## Platform: iOS first (decided)

Launch on iOS. The reasoning and the tradeoff are both real and worth holding onto.

**Why native, and why iOS**
- Scanning is meaningfully better native. Apple's VisionKit data scanner and the on-device Vision framework give live barcode reading, batch scanning, and on-device OCR for spines and printed ISBNs. Free, private, offline, and faster than the browser. This directly upgrades the core value prop.
- Reliable push notifications, which the loan and request loop leans on. Web push is the weak link.
- Contacts access for invites, better offline support, App Store distribution and trust.

**The tradeoff, stated honestly**
- iOS-only cuts out every Android neighbor. For a product whose value scales with how many people on your block are on it, that is a strategic cost, not a minor one. It is accepted for the POC and revisited before any real launch.

## Framework: native Swift / SwiftUI (decided)

**Full native Swift.** No React Native, no Expo. Chosen over RN + Expo primarily for two reasons: uncapped access to VisionKit's `DataScannerViewController` and on-device Vision for batch barcode scan and future printed-ISBN OCR, with no bridge or third-party-library-lag risk; and automatic, app-wide adoption of Apple's Liquid Glass design material as the OS ships it, rather than depending on third-party RN wrapper libraries that trail platform releases and cover surfaces piecemeal.

**Tradeoff accepted knowingly.** Day-to-day iteration is slower across the *whole* app — every screen goes through Xcode's compile/install/launch loop rather than RN's near-instant Fast Refresh, not just the scanner. SwiftUI Previews cover isolated view/component iteration well; testing full flows (navigation, live Supabase calls, auth) still needs a simulator run.

**Alternatives considered, not chosen**
- **React Native with Expo.** Was the leading option through most of the discussion. Faster iteration loop app-wide, and a real (if partial) web-reuse path via `react-native-web`. Lost out because the app's two highest-value differentiators — scan quality and native visual fidelity — are exactly the two things RN makes you chase rather than get by default.
- **Web PWA.** Only if the real goal is validating the lending loop as fast as possible rather than the cataloging magic. Weaker camera and notifications. Effectively foreclosed by catalog-first sequencing already being validated (see decision-log, "Catalog-first sequencing").

**Web companion, if it ever happens:** an independent, separately-scoped build (browsing/managing, not scanning) — no shared client code with the iOS app, since it's no longer React Native. See "Repo structure" in `decisions/decision-log.md`.

Supabase is the backend regardless, so none of this blocks backend work.

This was the one platform question still formally open; it's now resolved, see `decisions/decision-log.md`.

---

## Backend and data: Supabase

- **Postgres** for the relational core.
- **Auth** for identity, with Sign in with Apple on iOS.
- **Row-level security** as the sharing mechanism. Visibility rules ("share with select users") live in the database, not scattered across app code. This is the natural fit for the trust-network model and worth leaning on heavily.
- **Storage** for any cover images pulled into durable hosting (see the caching strategy in `book-data-and-apis.md`).
- **Realtime** for live loan and request status.

## Hosting

- Any server-side resolver logic runs as **Supabase Edge Functions**.
- Native app distribution through **Xcode** archive builds and **TestFlight** for the POC.
- If a companion web surface is ever needed, **Vercel**.

## Key libraries

- **Scanning:** VisionKit's `DataScannerViewController` and the on-device Vision framework, native. `@zxing/browser` if a web surface is ever built (independent build, not shared client code).
- **Vision (AI shelf scan):** the Anthropic API for the shelf-photo to titles step.

---

## Why row-level security matters here

The product requirement is "share with select users." RLS lets that be expressed as data: a policy that a copy is visible to its owner and to users with an accepted connection or an explicit share grant. The app queries normally and the database enforces who can see what. This keeps the permission logic in one place and hard to get wrong, which for a product built on trust is exactly where it should be.
