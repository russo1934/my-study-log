# Copilot instructions for my-study-log

This repository is a small Japanese study-log single-page PWA built with plain HTML, CSS, and JavaScript. There is no framework, build pipeline, or package manager.

## Build, test, and lint

There are no npm scripts, package.json files, test runners, or linting configs in this repo.

Use the app directly in a browser:

```bash
# Open directly (regular browser use; PWA features require a secure context)
xdg-open index.html

# Serve locally for PWA installation and offline testing
python3 -m http.server 8000
# then open http://localhost:8000
```

There is no automated test suite to run, so validation is primarily manual in the browser. If you change behavior, open the page and verify the affected flow (save, edit, delete, filter, chart update).

## Architecture

The app is intentionally simple and centered in three files:

- `index.html`: page structure for the form and history panel
- `styles.css`: layout, theme, responsive UI, and chart styling
- `app.js`: all application logic, rendering, storage, and user interactions

The app is a single-page experience with no backend. State is kept in browser `localStorage` under the key `tiny-diary.entries.v1`. `manifest.webmanifest` describes the installable app; `sw.js` precaches the static shell for offline use. Bump `CACHE_NAME` in `sw.js` when changing the precached app shell. Service Workers require HTTPS or localhost, so opening `index.html` with `file://` does not enable PWA installation or offline caching.

The core runtime flow is:

1. `entries` is loaded from localStorage on startup.
2. User submits a date, mood, and note.
3. The app upserts one record per date (`date` acts as the identity key).
4. The UI re-renders the list, summary chips, and recent 7-day rhythm chart.
5. Filter/search controls apply client-side filtering without server requests.

## Key conventions in this codebase

- Data shape is a plain array of study entries. Each item includes:
  - `id`
  - `date` (YYYY-MM-DD)
  - `mood` (`happy`, `calm`, `normal`, `tired`, `sad`)
  - `note`
  - `createdAt`
  - `updatedAt`

- The app treats one date as one record. When a date already exists, it updates the record instead of creating a duplicate.

- Mood values are not free-form strings; they are constrained by the `moods` object in `app.js`, which also provides labels, emoji, and numeric scores used for the chart and summary.

- Rendering is done by generating HTML strings with template literals and then assigning them to elements (`innerHTML`). Keep escaping in mind when inserting note text; the code uses `escapeHtml()` for safety.

- The app stores and displays dates in Japanese locale conventions (`Intl.DateTimeFormat("ja-JP")`). When changing date formatting or chart labels, preserve the existing Japanese UI expectations.

- Search/filtering is applied solely on the in-memory `entries` array, not on a server API.

- The rhythm chart is built from the most recent 7 entries, and the score map in `moods` is the source of truth for the graph.

## Repository context

This project is a static educational sample app. The README describes it as a local diary for study tracking and notes; keep changes aligned with that lightweight, browser-only model rather than introducing frameworks, backend code, or external dependencies.
