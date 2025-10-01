# Notes

A fast, offline-first bilingual notes app (English–Indonesian) with spaced repetition review, text-to-speech, tagging, import/export, and PWA install support.

## Features
- Quick add English–Indonesian pairs
- Instant search, sort (newest/oldest/A→Z/Z→A)
- Tagging and tag filter
- Favorites filter
- Card flip interactions
- Spaced repetition (SM-2) review flow with progress
- Text-to-Speech for EN and ID (if supported by the browser)
- Import and Export (JSON/CSV)
- Light/Dark theme with persistence
- Installable PWA with offline support

## Storage and Data
- Uses IndexedDB for notes and preferences (theme, TTS voice/rate/pitch/volume).
- Automatic migration: on first load, any legacy localStorage notes are read and saved into IndexedDB.
- Your data stays in the browser — no servers involved.

## Running locally
This is a static site.

- Easiest: open `index.html` directly in a modern browser.
- For full PWA behavior (service worker), serve over HTTP using any simple server, e.g.:
  - Node: `npx http-server .` (or any dev server)
  - Python: `python -m http.server 8080`

Note: The service worker only registers when the page is served over HTTP/HTTPS.

## Keyboard Shortcuts
- `/` Focus search
- `N` New note
- `D` Toggle dark mode
- `R` Start review
- Space (in review) Reveal answer
- `1/2/3/4` Grade during review

## Import/Export
- Export as JSON or CSV from the toolbar.
- Import JSON/CSV and the app will skip exact duplicates and keep scheduling fields when available.

## Card “More” (ellipsis) menu
- Click the three dots on a card to open the actions menu (favorite, copy, edit, delete).
- Click the three dots again to close it.
- Clicking anywhere outside a menu closes any open menus.

## Privacy
All data is stored locally in your browser (IndexedDB). Clearing site data will remove your notes unless you export them first.

## License
MIT (or your preferred license — update if different).
