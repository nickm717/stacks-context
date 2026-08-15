# Cataloging Methods

Context: how a physical book on a shelf becomes a row in the catalog. Speed here is the single biggest driver of whether someone finishes cataloging or abandons it. Once an ISBN or title is captured, resolution and storage are handled by the resolver in `book-data-and-apis.md`.

Support a layered set of methods.

---

## 1. Barcode and ISBN scanning (workhorse, build first)

Phone camera reads the EAN-13 Bookland barcode, extracts the ISBN, resolves metadata. Roughly 2 seconds per book.

- **Native:** Apple VisionKit data scanner or `react-native-vision-camera` with a barcode module. On-device, fast, offline.
- **Web fallback:** ZXing (`@zxing/browser`) for a POC, a commercial SDK (Dynamsoft, Scandit) later if reliability on worn, curved, or dim barcodes becomes a pain point.
- **Batch mode is table stakes.** Scan continuously without stopping between books, with a running count and per-hit confirmation. This is what makes cataloging a full shelf feel fast rather than tedious.

## 2. Whole-shelf AI vision (the differentiator, fast follow)

User photographs a shelf. An LLM vision model (the Anthropic API) identifies the visible spines and titles, then each candidate is resolved against the book APIs.

- Especially valuable for older or pre-barcode books and for bulk entry.
- A proven pattern, but imperfect on spines. It must feed a review-and-confirm step rather than adding silently.

## 3. Printed-ISBN OCR

For pre-2007 or no-barcode books, OCR the ISBN printed on the copyright page. On-device OCR via Apple Vision on native.

## 4. Manual search and add

Always needed as a final fallback. Search by title or author against the resolver.

## 5. CSV import

Let people bootstrap from an existing Goodreads, LibraryThing, or Libib export. Cheap to build, removes a major migration barrier for anyone with an existing catalog elsewhere.

---

## POC recommendation

Barcode scan plus manual as primary. Fastest to build, highest accuracy. AI shelf scan is the first post-POC feature since it is the most demo-able differentiator. Printed-ISBN OCR, CSV import, and batch AI resolution come after.
