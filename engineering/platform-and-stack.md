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

## Framework: React Native with Expo (recommended, pending confirmation)

**Leading option: React Native with Expo.** Keeps TypeScript, gives native camera and on-device scanning through `react-native-vision-camera`, ships iOS first, and makes Android largely a config-and-test effort later rather than a second codebase.

**Alternatives considered**
- **Pure Swift.** Only if the goal is to push on-device shelf-scan quality as far as Apple's Vision and Core ML allow. Costs a new language and an iOS-only codebase.
- **Web PWA.** Only if the real goal is validating the lending loop as fast as possible rather than the cataloging magic. Weaker camera and notifications.

Supabase is the backend in all three cases, so this choice does not block backend work.

This is the one platform question still formally open, see `decisions/open-questions.md`.

---

## Backend and data: Supabase

- **Postgres** for the relational core.
- **Auth** for identity, with Sign in with Apple on iOS.
- **Row-level security** as the sharing mechanism. Visibility rules ("share with select users") live in the database, not scattered across app code. This is the natural fit for the trust-network model and worth leaning on heavily.
- **Storage** for any cover images pulled into durable hosting (see the caching strategy in `book-data-and-apis.md`).
- **Realtime** for live loan and request status.

## Hosting

- Any server-side resolver logic runs as **Supabase Edge Functions**.
- Native app distribution through **Expo EAS** and TestFlight for the POC.
- If a companion web surface is ever needed, **Vercel**.

## Key libraries

- **Scanning:** `react-native-vision-camera` on native. `@zxing/browser` if a web surface is ever built.
- **Vision (AI shelf scan):** the Anthropic API for the shelf-photo to titles step.

---

## Why row-level security matters here

The product requirement is "share with select users." RLS lets that be expressed as data: a policy that a copy is visible to its owner and to users with an accepted connection or an explicit share grant. The app queries normally and the database enforces who can see what. This keeps the permission logic in one place and hard to get wrong, which for a product built on trust is exactly where it should be.
