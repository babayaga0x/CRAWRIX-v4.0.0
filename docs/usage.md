# Usage guide

Open the Crawrix frontend in your browser: the [live version](https://crawrix-frontend.vercel.app/), or your local address if you ran it yourself (see [Installation](installation.md)).

## Basic search

1. Type your keywords into the input field, **separated by commas**, for example: `python, coffee, football`.
2. Submit the query.
3. For each keyword Crawrix shows a list of links collected from several sources. Each link has a colored badge with its category (`technical`, `discussion`, `knowledge`, `news` or `general`).
4. Click a link to open it in a new tab. Long links are shortened in the list; hover over a link to see the full URL.
5. Use the back button to return and start a new search.

## Limits

- Up to **10 keywords** per search.
- Each keyword can be up to **50 characters**.
- Up to **10 searches per minute** per IP address. If you exceed this, wait a minute and try again.

## Language

Use the language toggle to switch the interface between English and Spanish.

## Tips

- Use short, specific keywords rather than full sentences.
- Unrelated keywords are fine: each one is searched separately.
- Use the category badges to quickly find what you need: for example, `technical` links come from Stack Overflow, `knowledge` links from Wikipedia.

## Installable app

Crawrix is a Progressive Web App, so it can be installed from your browser and used like a regular app.

## Using the API directly

You can call the backend without the UI. See [API endpoints](api-endpoints.md).

## Troubleshooting

| Problem | Solution |
|---------|----------|
| No results appear | The backend may be unavailable or the rate limit was exceeded. Wait a moment and try again. |
| A keyword shows no links | All sources failed or timed out for it. Try a different keyword. |
| Error on submit | Check that you have at most 10 keywords and that each is 50 characters or shorter. |

See also [How search works](how-search-works.md).
