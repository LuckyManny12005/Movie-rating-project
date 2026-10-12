# Movie-rating-project
# MovieDB

An IMDB-style movie browsing site in a single HTML file. Search a small catalogue, rank titles by rating, rate films yourself and keep a watchlist. There is no build step, no server and no dependencies.

## Quick start

1. Download `moviedb.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).

That's it. You can also host the file on any static host (GitHub Pages, Netlify, an S3 bucket) without changes.

## Features

- **Browse** a poster grid with a featured film that changes each day.
- **Search** by title, director, actor or year, as you type.
- **Filter** by genre and **sort** by rating, most votes, newest, title or runtime.
- **Top rated** view with a numbered ranking that respects your search and genre filter.
- **Details window** with plot, director, cast, runtime, genres and similar films. Close it with the ✕ button, Escape or a click outside.
- **Your rating** from 1 to 10 stars. Click the same star again, or use "Clear rating", to remove it.
- **Watchlist** you can add to and remove from in any view, with a live counter in the header.
- **Light and dark themes**, following your system setting until you switch manually.
- **Saved between visits**: ratings, watchlist and theme are stored in your browser's `localStorage`.
- **Responsive and accessible**: works on phones, with keyboard navigation, visible focus, screen-reader labels and reduced-motion support.

## Project structure

Everything lives in one file, split into three parts:

| Part | What it contains |
| --- | --- |
| `<style>` | Colour tokens for dark and light themes, layout, cards, modal, responsive rules |
| `<body>` | Static page shell: header, search, genre chips, sort menu, content area, details window |
| `<script>` | Movie data, app state, rendering functions and event handling |

### How the code works

- State is held in one object, `S` (current view, search text, genre, sort, ratings, watchlist, open title).
- `render()` redraws the hero, genre chips and the grid or ranking list. `renderModal()` redraws the details window.
- A single click listener on `document` handles every button through `data-*` attributes (`data-open`, `data-wl`, `data-nav`, `data-g`, `data-star`).
- User-facing text from the data is escaped before it is inserted into the page.
- `localStorage` access is wrapped in `try/catch`, so the site still works if storage is blocked (it just won't remember your choices).

## Adding or editing movies

Movies are in the `RAW` array near the top of the script. Each entry is a list in this order:

```js
[id, "Title", year, rating, votesInThousands, runtimeMinutes,
 "Genre1;Genre2", "Director", "Actor1;Actor2;Actor3", "One-sentence plot."]
```

Example:

```js
[25, "Blade Runner", 1982, 8.1, 800, 117,
 "Sci-Fi;Thriller", "Ridley Scott", "Harrison Ford;Rutger Hauer;Sean Young",
 "A retired detective hunts rogue androids in a rain-soaked future city."]
```

Rules to keep it working:

- `id` must be a unique number.
- Separate genres and cast members with `;` (semicolons), not commas.
- `votesInThousands` of 1000 or more is shown as millions (3100 shows as 3.1M).
- New genres appear automatically as filter chips.

## Customising the look

Colours are CSS variables at the top of the stylesheet:

```css
--bg  /* page background */      --acc  /* accent (logo, buttons) */
--panel /* cards and header */   --star /* rating stars */
--txt /* main text */            --ok   /* watchlist and your-rating highlight */
```

Light theme values are defined separately, so change both sets. Fonts (Barlow and Barlow Condensed) load from Google Fonts, with system fonts as a fallback if they can't load.

## Posters

The page doesn't load external images. Each poster is a gradient generated from the film's title, with the title and year on top. To use real artwork, swap the `poster()` function in the script for one that returns an `<img>` using your own image URLs or embedded data URIs.

## Limitations

- The catalogue is a small sample of 24 films, and the ratings and vote counts are approximate demo values, not live IMDb data.
- There are no accounts, user reviews, or cast pages.
- Ratings and the watchlist are stored per browser. They don't sync across devices and are cleared if you clear site data.

## Ideas for next steps

- Load a larger catalogue from a JSON file or a movie API such as TMDB (needs a hosted page and an API key).
- Add cast and director pages, trailers, and user reviews.
- Add a "Surprise me" button and a year or runtime range filter.
- Sync ratings and watchlist through a backend or an account system.

## Browser support

Any current version of Chrome, Edge, Firefox or Safari. The page uses the `inert` attribute and CSS `aspect-ratio`, which older browsers (before about 2022) may not support.
