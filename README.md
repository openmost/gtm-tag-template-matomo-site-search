# Matomo Site Search — Google Tag Manager template

A Google Tag Manager community template that records an **internal site search** in Matomo through the standard `_paq` queue.

Authored by Ronan HELLO — [Openmost](https://openmost.com).

---

## What this tag does

When the tag fires, it pushes a `trackSiteSearch` call to the Matomo tracker:

```js
_paq.push(['trackSiteSearch', searchKeyword, searchCategory, searchResults]);
```

The search is then visible in Matomo under *Behaviour → Site Search*, with the keywords, categories and "no result" keywords reports.

> **Note:** when `trackSiteSearch` is called, Matomo records a **search action** instead of a regular pageview for that page — so do **not** also fire a Matomo pageview on the search results page; fire the Site Search tag only.

---

## Prerequisites

1. A working **Matomo** instance (self-hosted or Matomo Cloud).
2. A **Google Tag Manager** container loaded on your site.
3. The Matomo base tracker (`_paq`) must already be initialised on the page.
4. The search keyword (and optionally category and number of results) must be available in the dataLayer, the URL, the DOM, or any source GTM can read through a variable.

This template only **adds the call to the `_paq` queue**; it does not load the Matomo tracker itself.

---

## Installation

### Recommended — install from the Community Template Gallery

This is the easiest path and gives you automatic update notifications when a new version is published.

1. In GTM, open your workspace and go to **Templates → Tag Templates → Search Gallery**.
2. Search for **"Matomo Site Search"** by **Openmost**.
3. Click **Add to workspace** and accept the requested permissions.

That's it — the tag type **Matomo Site Search** is now available when you create a new tag.

### Alternative — import `template.tpl` manually

Use this only if you can't access the Community Gallery (e.g. private GTM environment) or want to fork / customise the template.

1. In GTM, go to **Templates → Tag Templates → New**.
2. Open the menu (⋮) → **Import**.
3. Select `template.tpl` from this repository.
4. Save.

---

## Configuration

Once the template is added, create a new tag of type **Matomo Site Search** and fill in the fields below.

| Field | Required | Description |
|---|---|---|
| **Search keyword** | Yes | The search query the user entered. Typically a GTM variable that reads the `q` (or equivalent) URL parameter, or a value pushed to the dataLayer by the search component. |
| **Search category** | No | Optional category the search was scoped to (e.g. `Products`, `Articles`, `Help`). Leave empty if not applicable. |
| **Number of results on the results page** | No | Optional integer with the count of results returned. Useful to track searches with **zero results**. |

### Common patterns for the keyword

- URL Query Parameter variable on `?q=...` / `?s=...` / `?search=...`
- Data Layer Variable populated by the site's search component
- DOM element variable reading the search input

---

## Triggering

Fire the tag on the **search results page**, on the same lifecycle as you would fire a pageview — typically:

- A *Page View* trigger filtered to the search results URL
- A *Custom Event* trigger (`search_performed`) pushed by the site

> **Important:** because `trackSiteSearch` replaces the pageview for that page, configure your Matomo Pageview tag to **not fire** on the search results URL — otherwise the search will be double-counted as both a pageview and a search.

---

## Permissions

The template requests one permission:

- **Access global variables → `_paq`** (read / write / execute) — required to push the call into Matomo's tracker queue.

No external network requests are made directly by the template.

---

## Troubleshooting

- **No search shown in Matomo** — confirm the tag fired in GTM Preview, and that the *Search keyword* field is not empty (an empty keyword aborts the call in Matomo).
- **Pageview is recorded instead of a search** — your standard Matomo Pageview tag is also firing on the same URL; exclude the search results URL from its trigger.
- **Zero-result searches not appearing in the "No-result keywords" report** — make sure the **Number of results** field is filled with `0` (not left empty) when no results are returned.

---

## License

See [LICENSE](LICENSE).
