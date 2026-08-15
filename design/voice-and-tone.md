# Voice and Tone for Stacks

How Stacks writes. This is the copy counterpart to `design-principles.md`, and it is enforceable: every string in the product should pass the rules in §2 and the smell test in §8. Status is **draft**, seeded 2026-08-13. Open calls are parked in §9.

Strategy lives in [`../PRODUCT.md`](../PRODUCT.md) (brand personality: neighborly, trustworthy, effortless). State labels are owned by [`../circulation/copy-state-model.md`](../circulation/copy-state-model.md) and this doc does not override them.

---

## 1. The voice in one line

**Write like you are texting a neighbor you like but don't know well.**

Friendly, specific, a little dry. You say what happened and what they can do. You don't perform enthusiasm, you don't explain their feelings to them, and you don't take up more of their day than the message needs.

Three tests for any string:
1. Would a real person say this sentence out loud?
2. Does it name a person, a book, or a date instead of a category?
3. Could it appear in any other app? If yes, it isn't ours yet.

---

## 2. Sentence rules

These are mechanical. They are the difference between copy that reads human and copy that reads generated.

1. **No em dashes.** None. If a sentence reaches for one, break it into two sentences or cut the aside.
2. **One idea per sentence.** Aim for 12 words or fewer in the UI. Two short sentences beat one balanced one.
3. **No trailing qualifier clauses.** This is the failure mode we care most about. A comma followed by a floating fragment is an em dash wearing a disguise.
   - Bad: "Devi approves or not, no obligation."
   - Good: "Devi will see your request. She can say no."
4. **No "not X, but Y" and no "X, not Y."** Antithesis is the loudest machine tell in modern interface copy. State the thing you mean and stop.
   - Bad: "This isn't a marketplace, it's a neighborhood."
   - Good: "Nothing here is for sale."
5. **No lists of three.** "Fast, simple, and delightful" is out. So is any adjective triplet.
6. **No colon-led fragments as UI copy.** "One shelf: infinite reach" is advertising, and we are not advertising.
7. **No semicolons in the product.** They are fine in these docs, never on a screen.
8. **Contractions on.** "You'll", "it's", "can't", "won't".
9. **Full sentences take periods. Labels, buttons, and badges don't.**
10. **Second person for what the user does. First-person plural only for what the system does.** "We couldn't read that barcode" is fine. "We think you'll love this" is not.
11. **Exclamation marks: zero.** The onboarding brand moment can request an exception, one per flow, in review.

---

## 3. Words and moves we don't use

**Banned vocabulary.** Seamless, effortless, effortlessly, simply, just, easily, unlock, empower, elevate, curate, journey, ecosystem, delight, reimagined, supercharge, powerful, robust, magical.

Note that "cataloging should feel like magic" is a *design goal* in PRODUCT.md. It is not a word we ever put on a screen.

**Banned moves.**

- Flattering the user. "Nice work!", "You're on a roll", "Great choice".
- Explaining the user's feelings to them. "No obligation", "Don't worry", "No pressure", "Take your time".
- Cutesy error voice. "Oops", "Uh oh", "Something went wrong", "Yikes".
- Onboarding filler. "Let's get started", "Welcome aboard", "You're all set".
- Urgency and scarcity. "Only one copy left", "Act now", "Don't miss".
- Gamification. Streaks, levels, badges as reward, "you're in the top 10%".
- Marketplace framing. Anything that treats a book as inventory or a neighbor as a seller.
- Rhetorical questions as headers. "Ready to build your shelf?"
- Vague reassurance in place of information. If a thing is safe, show what is shared, don't say "your privacy matters".

---

## 4. Tone by moment

| Moment | The feeling | How we write |
| --- | --- | --- |
| Cataloging and scanning | Momentum, quiet | Counts, not compliments. Never interrupt a batch to congratulate. |
| Empty states | Useful, unembarrassed | Name what's missing in three words, then point at the action. |
| Asking to borrow | Low stakes | Short and factual. Do not editorialize about pressure. |
| Receiving a request | Unhurried, clean out | Both options equally weighted. No default-yes styling, no guilt in the copy. |
| Declining | No shame, no apology theatre | State the outcome once. Do not apologize on either person's behalf. |
| Overdue | Neutral, factual | Dates and names. The badge carries the urgency, the sentence does not. |
| Errors | Cause, then next step | What happened, what to do. No blame, no personality. |
| Success | Understated | Confirm and get out of the way. |
| Trust and privacy | Concrete | Show exactly who can see what. Never abstract it into a promise. |

---

## 5. Before and after

The house examples. When in doubt, pattern-match against these.

**The one that started this.**
- Before: "Devi approves or not, no obligation."
- Why it fails: two clauses joined by a comma with a floating fragment on the end, and it explains a feeling to the user.
- After: "Devi will see your request. She can say no."

**Request sent.**
- Before: "Your request has been submitted and Devi will be notified shortly."
- After: "Request sent to Devi." Secondary line: "She'll reply when she can."

**Declined.**
- Before: "Unfortunately Devi was unable to fulfill your request at this time."
- After: "Devi can't lend this one right now."

**Overdue, owner view.**
- Before: "This book is overdue! Please follow up as soon as possible."
- After: "Due back Jun 3. Still with Marcus."

**Overdue, borrower view.**
- Before: "You're late returning this item."
- After: "This was due Jun 3."

**Empty library.**
- Before: "Your library is empty. Let's get started by adding your first book!"
- After: "No books yet." Button: "Scan a shelf".

**Scan confirmation.**
- Before: "Success! Book added to your library."
- After: "Added. 12 books this session."

**Scan error.**
- Before: "Oops, something went wrong. Please try again later."
- After: "We couldn't read that barcode. Try searching the title."

**Sharing default.**
- Before: "Take control of your privacy with granular visibility settings."
- After: "People you connect with can see this shelf." Link: "Change who can see it".

**Return to confirm.**
- Before: "Marcus has indicated that this item has been returned. Please confirm receipt."
- After: "Marcus says he returned this." Buttons: "Got it back" / "Not yet".

**Connection request.**
- Before: "Jenna wants to connect with you and view your library!"
- After: "Jenna wants to connect." Body: "If you accept, she can see the books you share."

**Reminder.**
- Before: "Don't forget to remind Sam to return your book!"
- After: "Remind Sam about this book?" Body: "We'll send a short note with the title."

---

## 6. Labels, buttons, badges

- **Buttons name the action the user is taking.** "Request to borrow", "Lend", "Mark returned", "Got it back", "Scan a shelf".
- **Never** "Submit", "OK", "Continue" on a decision, or "Confirm" standing alone.
- **Sentence case everywhere.** No title case, no all caps.
- **Badges use the state word from the copy-state model.** Available, Requested, On hold, Ready for pickup, Checked out, Return pending, Overdue, Not lending. Do not invent synonyms.
- **Labels are role-accurate.** The requester sees "Request pending". Everyone else sees "On hold" with no name attached.
- **Every state label pairs with an icon.** Color never carries meaning alone.

---

## 7. Names, numbers, dates

- Use people's names. "Still with Marcus" beats "currently with the borrower".
- Never name a third party where the copy-state model says the request stays private.
- Prefer counts to adjectives. "12 books" beats "a lot of books".
- Dates inside a week use the weekday: "Due Friday". Beyond that use short form: "Due Jun 3". Never "09/03/2026".
- No due date is a real state and reads as "No due date", never as "Indefinite" or "Forever".

---

## 8. The smell test

Read the string out loud. Rewrite it if any of these are true.

- You paused where an em dash would go.
- A comma is holding two clauses together that should be two sentences.
- It compliments the user.
- It tells the user how to feel.
- It reassures without showing anything.
- It contains a word from §3.
- It would work, unchanged, in a food delivery app.
- No human would say it out loud to another human.

---

## 9. Open questions

- **"We" for the system.** Recommended: allowed for system actions ("We couldn't read that barcode"), banned for opinions or decisions. Confirm before the copy deck is written.
- **The onboarding brand moment.** PRODUCT.md treats onboarding as brand register. How much more warmth and rhythm is allowed there than in the product, and does the exclamation ban hold?
- **Notification copy.** In-app only for the POC, so the constraint is loose. Push copy will need its own length and tone rules later.
- **Off-app loans.** How we refer to a person who isn't on Stacks without making them sound like a lesser record.
- **Full copy deck.** The 11 badge and role-label variants in `component-spec.md` §3.C still need a pass against this doc.
