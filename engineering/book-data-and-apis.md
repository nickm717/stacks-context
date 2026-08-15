# Book Data, APIs, and Caching

Context: how Stacks resolves an ISBN or title into rich book metadata and cover art, and how that data is stored. Cataloging methods (how the ISBN gets captured in the first place) are in `cataloging-methods.md`.

---

## Metadata APIs

**Recommended default: a multi-source resolver over free APIs, with your own cache as the canonical layer.**

- **Primary: Google Books and Open Library, queried together and merged.** Both are free and need no key for basic lookups. Google Books is strong on mainstream and international titles. Open Library, backed by the Internet Archive, is stronger on older and obscure editions and exposes a clean RESTful API plus a dedicated Covers API by ISBN or Open Library ID. Merging the two gives materially better hit rates than either alone.
- **Cover art.** Open Library Covers API (by ISBN or OLID) and Google Books thumbnails.
- **Paid fallback only if needed: ISBNdb.** Roughly $15 to $300 per month depending on tier, 100M-plus titles, up to 19 data points, updated daily. Worth it only if free-source coverage proves insufficient for real users' shelves. Skip for the POC.
- **Do not build on Goodreads.** Its developer API was deprecated years ago and it has no real ownership or lending model anyway.

## The resolver pattern

A single server-side function (Supabase Edge Function) takes an ISBN or a title query, fans out to Google Books and Open Library, normalizes and merges the responses, and writes the result to the `book_editions` table.

- Everything is cached in your own DB. You hit the external APIs once per edition, then serve from your own data.
- This protects you from rate limits, gives you a stable schema, and lets you correct or enrich records over time.
- Respect source etiquette: Google Books has a request quota, and Open Library expects polite request pacing and a descriptive User-Agent.

---

## Caching strategy

**Cache, do not pull live on every read.** This is the standard production pattern, not the exception. External APIs are a source to sync from, not a database to query live.

Why:
- **Rate limits.** Re-hitting the API on every shelf view or search would burn quota fast. Cache means you pay the cost once per edition, ever.
- **Latency.** A live fan-out to two APIs per book is hundreds of milliseconds to seconds and fails when they are slow. A read from your own Postgres is single-digit milliseconds and works through their outages.
- **Control.** Your own data can be normalized into one schema, merged across sources, corrected, and extended with your own fields.

**The pattern is read-through (lazy) caching.** On first encounter with a book, resolve it live, write it to `book_editions`, and serve every subsequent read from your copy. Live calls are reserved for genuine cache misses.

### Managing bloat

The instinct that a metadata cache balloons is half right. The text is cheap. The images are the trap.

- **Deduplication saves you.** `book_editions` is shared across all users and the universe of books is finite. One edition row is roughly 1 to 3 KB. You only cache editions someone actually owns, and popular titles recur heavily. Even a million distinct editions is a couple of GB of text, which Postgres does not care about.
- **Cover images are the real cost.** They are 20 to 200 KB each. Store the cover URL and hot-link or proxy from Open Library and Google rather than downloading by default. Only pull an image into your own Storage when you need durability, and when you do, put it behind a CDN and resize it.
- **Staleness is a correctness issue, not a size issue.** The bigger long-term risk than bytes is a wrong edition or a dead cover URL cached forever. Build a re-fetch and correction path from day one so a record can be refreshed or fixed. Bibliographic facts barely change, so this is about repair, not routine refresh.

### Licensing

Not legal advice, verify against current terms.

- **Open Library data is openly licensed** and safe to persist. Treat it as the canonical cache.
- **Google Books terms are more restrictive** about storing and redisplaying their data. Use it to fill gaps at lookup time, and check the current terms of service before persisting much of its metadata.
