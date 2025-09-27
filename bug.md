# Bug log and status

Date: 2025-09-27

Current state
- No blocking JavaScript errors after recent fixes to index.html (review modal markup and card template strings) and adding a service worker guard for file://.
- Review, Import, and Export buttons are wired and functional when served over HTTP.

Expected messages when opening via file:// (not a bug)
- Tailwind CDN warning:
  - "cdn.tailwindcss.com should not be used in production" — Safe to ignore in local development.
- Manifest/Service Worker errors due to CORS and protocol:
  - Access to manifest at 'file:///.../manifest.json' blocked by CORS
  - Failed to register a ServiceWorker (on file://)
  - Reason: PWA pieces require http/https. These messages disappear when using a local server.

How to run (to avoid the above messages)
- Python: `py -m http.server 5173`
- Node: `npx serve -l 5173`
Then open http://localhost:5173/

Verification checklist
- Review button opens a session or shows "No cards are due for review right now."
- Export → JSON/CSV downloads a file.
- Import → JSON/CSV opens the file picker and successfully imports.

If issues persist
- Open DevTools → Console, reproduce, and paste any red error messages here with timestamps and steps to reproduce.
