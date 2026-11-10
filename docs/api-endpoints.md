# API endpoints

The backend is a Flask application (`server/app.py`). When you run it locally, the terminal shows its address. The production backend is `https://crawrix-backend.vercel.app`.

CORS is enabled only for `http://localhost:5173`, `http://localhost:4173` and `https://crawrix-frontend.vercel.app`. Requests from other origins are rejected by browsers.

## Rate limiting

Every endpoint is limited to **10 requests per minute per IP address**. When the limit is exceeded, the API returns `429 Too Many Requests`.

## `POST /parse`

Searches each keyword separately across several sources and returns the merged results.

### Request

Content type: `application/json`

```json
{
  "keywords": ["python", "coffee"],
  "lang": "en"
}
```

| Field      | Type            | Required | Description |
|------------|-----------------|----------|-------------|
| `keywords` | array of string | yes      | Up to 10 keywords, each up to 50 characters. |
| `lang`     | string          | no       | UI language sent by the frontend (`en` or `es`). Currently not used by the backend. |

### Response `200 OK`

An array with one object per keyword, in the same order as the request:

```json
[
  {
    "keyword": "python",
    "links": [
      {
        "url": "https://www.python.org",
        "title": "Welcome to Python.org",
        "snippets": ["The official home of the Python Programming Language."],
        "sources": ["bing"],
        "type": "general"
      },
      {
        "url": "https://en.wikipedia.org/wiki/Python_(programming_language)",
        "title": "",
        "snippets": [],
        "sources": [],
        "type": "knowledge"
      }
    ]
  }
]
```

| Field     | Description |
|-----------|-------------|
| `keyword` | The keyword the results belong to. |
| `links`   | All merged results for this keyword. May be empty. |

Fields of each item in `links` are described in [Result structure](#result-structure).

### Errors

| Status | Body | Cause |
|--------|------|-------|
| `400`  | `{"error": "Keywords must be a list of max 10 items"}` | `keywords` is not a list, or has more than 10 items. |
| `400`  | `{"error": "Keywords must be strings <= 50 symbols"}` | A keyword is not a string or is longer than 50 characters. |
| `429`  | rate limit message | More than 10 requests per minute. |

A source that fails or times out (3 seconds) is skipped silently, so the response can contain fewer results than expected.

## `GET /ping`

Health check. Returns `pong` with status `200`.

## Result processing

Search results are processed before being returned by the API.

### Sources

Each keyword is searched concurrently (`asyncio.gather`) in: Bing, Yahoo, DuckDuckGo, Wikipedia (English), Reddit, Qwant and Stack Overflow. Each source contributes at most 15 links.

Only Bing results include a title and a snippet. Other sources return URLs only, so for them `title` is empty and `snippets` is an empty list unless the same URL was also found by Bing.

### URL normalization

The URL normalizer (`server/utils/normalizer.py`) standardizes URL representations and removes duplicates.

It performs the following operations:

- Converts the URL's network location (`netloc`) to lowercase.
- Removes trailing slashes from the URL path.
- Removes path parameters (`;params`) and fragments (`#...`).
- Preserves the query string.
- Removes duplicate URLs after normalization.

Normalization and deduplication are applied to the links of each source. After that, results from all sources are merged by exact URL: `snippets` and `sources` of identical URLs are combined into one result.

URLs with different query strings are treated as different URLs.

### Result categorization

The categorizer (`detect_source`) assigns a category to each search result based on its domain. The assigned category is returned in the `type` field.

| `type`       | Rule |
|--------------|------|
| `technical`  | Domain contains `stackoverflow.com` or `stackexchange.com`. |
| `discussion` | Domain contains `reddit.com`. |
| `knowledge`  | Domain contains `wikipedia.org`. |
| `news`       | Domain contains the text `news`. |
| `general`    | Everything else. |

### Result structure

Each item in `links` contains the following fields:

| Field      | Description |
|------------|-------------|
| `url`      | Normalized URL of the result. |
| `title`    | Page title, when available (empty string otherwise). |
| `snippets` | List of text excerpts, when available (may be empty). |
| `sources`  | List of search providers associated with the result (may be empty). |
| `type`     | Category assigned by the categorizer. |

### SSRF protection

Every URL is checked by `is_safe_url` before it is requested or returned. A URL is rejected if:

- its scheme is not `http` or `https`;
- it has no hostname, or the hostname is `localhost`, `localhost.localdomain` or `ip6-localhost`;
- the hostname is an IP address that is private, reserved, loopback or multicast.

## See also

- [How search works](how-search-works.md)
- [Usage guide](usage.md)
