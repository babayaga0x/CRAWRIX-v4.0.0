# How search works

Crawrix handles every keyword on its own: it queries several sources for each keyword and returns a separate list of links for it. This page describes the pipeline.

## Pipeline

1. **Input.** The frontend splits the input string by commas and trims each part. Every part is one keyword. The list is sent to `POST /parse` (see [API endpoints](api-endpoints.md)).
2. **Validation.** The backend accepts up to 10 keywords, each up to 50 characters. The request is limited to 10 per minute per IP.
3. **Concurrent search.** For each keyword, seven sources are queried at the same time: Bing, Yahoo, DuckDuckGo, Wikipedia, Reddit, Qwant and Stack Overflow. Each request has a 3-second timeout; a source that fails is skipped.
4. **Parsing.** Bing and Yahoo are HTML pages parsed with BeautifulSoup (Bing also provides title and snippet, and its redirect links are decoded to the real URL). The other sources are JSON APIs.
5. **Safety check.** Every URL passes the SSRF check (`is_safe_url`) before it is used.
6. **Normalization.** URLs are normalized and duplicates are removed (`normalizer.py`).
7. **Merging.** Results from all sources are merged by URL, combining snippets and source names.
8. **Categorization.** Each link gets a `type`: `technical`, `discussion`, `knowledge`, `news` or `general`.
9. **Response.** The API returns one entry per keyword with its list of links. The frontend shows each link with a colored category badge.

## Why per-keyword search

A regular search engine tries to find pages matching the whole query. When a query mixes unrelated topics, such pages rarely exist and the results are poor. Searching each keyword on its own gives relevant results for every part of the query.

## Categories

| Category     | Badge color | Typical source |
|--------------|-------------|----------------|
| `technical`  | blue        | Stack Overflow, Stack Exchange |
| `discussion` | orange      | Reddit |
| `knowledge`  | purple      | Wikipedia |
| `news`       | red         | Domains containing "news" |
| `general`    | gray        | Everything else |

## See also

- [Features](features.md)
- [API endpoints](api-endpoints.md)
