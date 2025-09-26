# Notes App — What it does and what stack it uses

A fast, offline‑friendly bilingual notes app for English ↔ Indonesian word pairs. It’s a small PWA you can install, with instant search, sorting, inline edit/delete, card flip animations, dark mode, and keyboard shortcuts. Data is stored locally in your browser (no server required).

---

## What this app does

- Add a pair: English word + Indonesian translation
- See each pair as a card you can flip to reveal the other side
- Search instantly across both languages
- Sort by newest/oldest or A→Z/Z→A (English)
- Edit inline, delete, copy to clipboard
- Dark mode with a toggle and a keyboard shortcut
- Offline-first: data persists in localStorage; optional PWA install button
- Responsive grid that scales from mobile to desktop

---

## Tech stack (no backend)

- HTML5 + Tailwind CSS (via CDN)
  - Tailwind configured in CDN mode with class-based dark mode
  - Inter font via Google Fonts
- Vanilla JavaScript (no framework)
  - DOM rendering, keyboard shortcuts, and UI logic
  - localStorage for persistence (key: `notes-data-v1`)
  - Clipboard API for copy action
  - `crypto.randomUUID()` (with fallback) for client-side IDs
- PWA pieces
  - Web App Manifest: `manifest.json`
  - Service Worker: `/sw.js` (registered at runtime)
  - Install UX: listens for `beforeinstallprompt`, shows a floating Install button
- Assets
  - App icon/logo: `images/letter-n.png`

Note: There is no server or database. Everything runs in the browser.

---

## How it works (step by step)

1) Boot and theme
- Tailwind is loaded from CDN; dark mode uses the `class` strategy.
- The app determines theme on load from localStorage (key: `theme-preference`) or OS preference via `matchMedia`. It then sets `meta[name="theme-color"]` to match.

2) Service worker and install prompt (PWA)
- If `navigator.serviceWorker` is available, the app registers `/sw.js`.
- It listens to `beforeinstallprompt` to show the floating Install button. Clicking it triggers the browser’s PWA install prompt.

3) State initialization
- Notes are read from localStorage (`notes-data-v1`). Each note has:
  - `id`: a unique ID (UUID or fallback)
  - `en`: English word
  - `id`: Indonesian translation (named `id` in code)
  - `createdAt`: timestamp

4) Rendering pipeline
- `renderNotes()` filters notes using the search query (matches either language), sorts them based on the selected mode, and renders a grid of cards.
- An empty state message is shown if there are no matching notes.
- The total count appears as a badge in the navbar.

5) Cards & interactions
- Each card is focusable and supports click/keyboard to flip (3D rotateY).
- Action buttons on hover: Copy, Edit, Delete.
  - Copy writes "English — Indonesian" to the clipboard.
  - Edit switches the card into inline edit mode with Save/Cancel.
  - Delete removes the note after confirmation.

6) CRUD and persistence
- Add: The Quick Add form creates a note and prepends it to the list.
- Edit: Inline editor updates fields and re-renders.
- Delete: Removes the note and saves the new state.
- Clear All: Wipes the list after confirmation.
- After any change, state is saved back to localStorage.

7) Search and sort
- Search input (desktop navbar and mobile toolbar) filters as you type.
- Sort selector: Newest, Oldest, A→Z, Z→A (by English term).

8) Keyboard shortcuts
- `/` focuses the search input (unless you’re typing in another input)
- `N` focuses the English input to add a new note
- `D` toggles dark mode

9) Responsiveness & accessibility
- Responsive grid via Tailwind breakpoints (1–4 columns).
- Cards have `tabindex="0"` and support flipping via Enter/Space.
- Color contrast is tuned for dark/light modes.

---

## How to run locally

You can open `index.html` directly in a browser for most features. For PWA/service worker behavior (install prompt, offline caching), you should serve over HTTP.

Option A — Python (Windows PowerShell):
- Python 3: `py -m http.server 5173` (or `python -m http.server 5173`)
- Then open: http://localhost:5173/

Option B — Node (if you have npm):
- `npx serve -l 5173`
- Then open: http://localhost:5173/

Note: Service workers don’t activate on `file://` URLs; use a local server to test install/offline.

---

## File map (expected)

- `index.html` — Main UI (Tailwind CDN + JS logic)
- `manifest.json` — PWA manifest (icons, name, theme)
- `sw.js` — Service worker (must be at site root path to control the scope `/`)
- `images/letter-n.png` — App icon used in UI and possibly in the manifest

---

## Extending the app (ideas)

- Import/Export: JSON/CSV backup and restore
- Favorites/tags: filter by tag or star
- Spaced repetition: review sessions (e.g., SM-2)
- TTS buttons: speak English/Indonesian via Web Speech API
- Multi-language: support more language pairs
- Cloud sync: optional backend/API if you need cross-device syncing
- Build pipeline: self-hosted Tailwind build instead of CDN for production

---

## Privacy & data

- All data is stored in your browser’s localStorage.
- No analytics or network calls are required for core features.

---

## Troubleshooting

- Install button not showing: ensure you’re serving over HTTP(S) and `manifest.json` + `sw.js` are reachable.
- Changes not persisting: check that your browser allows localStorage in your context (not in private mode with restrictions).
- Safari/iOS specifics: PWA install flow differs; you may need “Add to Home Screen”.

---

## Quick recap (stack)

- UI: HTML5 + Tailwind CSS (CDN), Inter font
- Logic: Vanilla JS (DOM, events, keyboard shortcuts)
- Data: localStorage
- PWA: Manifest + Service Worker + Install prompt
- APIs: Clipboard, `crypto.randomUUID()` (fallback provided)
