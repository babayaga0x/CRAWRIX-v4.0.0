# Crawrix

Crawrix scans your search queries and links to the most relevant websites, searching each keyword separately.

Live - https://crawrix-frontend.vercel.app/
Changelog - https://github.com/babayaga0x/CRAWRIX-v4.0.0/blob/554037d87a4da522a4942c31a1399b4a7258e4da/CHANGELOG.md

## Why Crawrix

A regular search engine struggles when a query contains several unrelated words. Crawrix handles each keyword on its own, collects results from several sources and sorts them by category, so you can find what you need quickly. It is useful for everyday users, SEO analysts, and web devs.

## Features

- Enter several unrelated keywords separated by commas
- Get a separate list of relevant links for each keyword from Bing, Yahoo, DuckDuckGo, Wikipedia, Reddit, Qwant and Stack Overflow
- Automatic result categorization (`technical`, `discussion`, `knowledge`, `news`, `general`)
- `normalizer.py` normalizes URLs, removes duplicates and detects the category of each link
- SSRF protection prevents the backend from requesting internal or unsafe addresses
- Layout and UI elements adapt to small screens

Crawrix interface:

![Crawrix interface](images/crawrix-1.png)

## Getting started

- [Installation and quick start](installation.md)
- [Usage guide](usage.md)
- [API documentation](api-endpoints.md)
- [How search works](how-search-works.md)
- [Features](features.md)

## Tech stack

Frontend: React + TypeScript (Vite). Backend: Python + Flask, httpx and BeautifulSoup.
