# Features

## Multi-keyword queries

Enter several unrelated keywords separated by commas (up to 10). Crawrix processes each keyword on its own.

## Multiple sources

For each keyword Crawrix searches Bing, Yahoo, DuckDuckGo, Wikipedia, Reddit, Qwant and Stack Overflow at the same time (asynchronous search with `asyncio.gather`), so results arrive quickly.

## Result processing

- URL normalization and removal of duplicates (`normalizer.py`)
- Merging of the same link found in different sources
- Titles and snippets when available

## Automatic categorization

Every link gets a category, shown as a colored badge: `technical`, `discussion`, `knowledge`, `news` or `general`.

## SSRF protection

The backend validates every URL and rejects non-HTTP(S) schemes, `localhost` and private, reserved, loopback or multicast IP addresses.

## Rate limiting

The API allows 10 requests per minute per IP address to prevent abuse.

## Responsive interface

Layout and UI elements adapt to small screens.

## Multilingual interface

The interface is available in English and Spanish.

## Progressive Web App

Crawrix can be installed as an app and uses a service worker for offline-friendly loading.

## Who it is for

Everyday users, SEO analysts and web developers.

## Tech stack

- Frontend: React + TypeScript (Vite)
- Backend: Python + Flask (async), BeautifulSoup, httpx

For release history see the [changelog](https://github.com/babayaga0x/CRAWRIX-v4.0.0/blob/554037d87a4da522a4942c31a1399b4a7258e4da/CHANGELOG.md).
