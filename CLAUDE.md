# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A self-contained, single-file mobile-friendly menstrual cycle tracker deployed via GitHub Pages at `https://rharder.github.io/cycle-tracker/`. The entire app is `index.html` — no build step, no dependencies to install, no framework. It is designed for use by the person who has the cycle, their partner, or both — whoever is doing the tracking.

The developer is Robert Harder (`rharder`), a senior software engineer. He is proficient in Python and prefers clean, modular code. For any Python tooling, avoid port 5000 (use a random port between 5001–5099 if running Flask).

## Development

There is no build system or package manager. To work on the app:

- Edit `index.html` directly
- Open it in a browser (`open index.html`) or serve locally (`python3 -m http.server 5001`)
- Push to GitHub; GitHub Pages deploys automatically from the `main` branch root

The OAuth Client ID is hardcoded in `index.html` at `const GOOGLE_CLIENT_ID`. Google Drive sync only works when served from `https://rharder.github.io` (the authorized JavaScript origin). It will not work from `localhost` or `file://`.

Icon files (`apple-touch-icon.png`, `favicon.ico`, `favicon-32.png`, `favicon-16.png`) were generated with Pillow. The icon is a four-segment pie chart representing the four cycle phases, clipped to Apple's squircle shape (~22.5% corner radius). Segments are proportional to average phase lengths: menstrual 5/28, follicular 9/28, ovulation 3/28, luteal 11/28. The icons are also embedded as base64 in `index.html` so it is fully self-contained.

## Architecture

Everything lives inside `index.html`: HTML structure, CSS, and JavaScript in a single `<script>` block. No modules, no bundling. CSS uses system font stack (`-apple-system, BlinkMacSystemFont`). Dark mode is handled via `@media (prefers-color-scheme: dark)`.

**Data layer** (`STORE_KEY = 'cycle_tracker_v2'` in `localStorage`):
- `data.settings` — `{ cycleLen, periodLen, ovWin, months }`
- `data.periods` — array of `{ id, start, end }` where dates are ISO strings (YYYY-MM-DD) and `end` may be null (open/ongoing period)
- All mutations go through `saveData(data)` which writes to localStorage and calls `scheduleDriveSync()`

**Cycle logic** (`computeCycleLen`, `buildMaps`):
- `computeCycleLen()` auto-calculates average cycle length from ≥3 logged periods (gaps between consecutive start dates, filtered to 15–60 day range); falls back to `settings.cycleLen`. Returns `{ len, calculated, cycles }` — the `calculated` boolean drives the blue "calculated" vs grey "default" badge on the stat card.
- `buildMaps()` produces two maps keyed by ISO date string: `phaseMap` (predicted phase for every visible date) and `loggedMap` (dates covered by a logged period entry, mapped to period `id`). Phases assigned relative to cycle day: `menstrual (0..periodLen-1) → follicular → ovulation (ovDay±) → luteal`. Ovulation day is `cycleLen - 14`.
- Phase colors: menstrual `#F0997B`, follicular `#9FE1CB`, ovulation `#AFA9EC`, luteal `#FAC775`, logged period `#ED93B1`

**Day tap modal** (`dayTapped`):
- Tapping a calendar day opens a contextual action sheet. The logic determines which actions to show:
  - Always: "Log as period start"
  - If there's an open (no-end) period and tapped day ≥ its start: "Set as end"
  - If tapped day is within a closed logged period, or within 4 days after one ends (`nearbyPeriod` lookahead): "Update period end to this day" — this is intentional to handle the common case where a period ran longer than initially logged
  - If tapped day is inside a logged period: "Remove this period entry"
- The modal inserts itself just above the month block containing the tapped day (not at the top of the page)
- Tapping the same day again dismisses the modal

**Google Drive sync** (Google Identity Services token flow):
- Token stored in `localStorage` under `TOKEN_KEY` with expiry; auto sign-in attempted on page load if stored token is valid
- On sign-in: `findOrCreateDriveFile()` searches Drive for `cycle-tracker-data.json`, creates it if absent → `pullFromDrive()` merges remote data → pushes on every data change (2s debounce via `scheduleDriveSync`)
- Merge strategy: union of periods by `start` date (no duplicates); remote `settings` wins only when new periods are pulled from remote. This means no data is ever lost when two devices diverge.
- Sync status shown in a bar at the top of the app (green dot = synced, orange pulsing = busy, grey = disconnected)

**UI structure**:
- Three tabs: Calendar, Log, Settings — toggled by `showTab()`
- Calendar renders all months from current month forward (`data.settings.months` total) as scrollable month grids. Day cells are circular (`border-radius: 50%`, `aspect-ratio: 1`), sized via CSS grid `grid-template-columns: repeat(7, 1fr)`. Font size uses `clamp(13px, 3.5vw, 18px)` for readability across screen sizes.
- A `<meta name="viewport" content="width=device-width, initial-scale=1">` tag is critical — without it Safari on iPhone renders a 980px desktop viewport and the calendar appears half-width.
- Help panel: a fixed `?` button (bottom-right) opens a bottom sheet overlay with usage instructions and privacy information.
- No state management library — DOM is re-rendered from scratch on each `renderCalendar()` / `renderLog()` / `loadSettings()` call.

## Key design decisions

- **Single file**: chosen for simplicity of deployment and portability. The file is fully self-contained including base64-encoded icons, so it works when saved locally or served from any static host.
- **localStorage over IndexedDB**: simpler API, sufficient for this data size.
- **No framework**: keeps the file small and eliminates dependency rot. The app is intentionally simple enough that vanilla JS is fine.
- **Merge-not-overwrite sync**: when pulling from Drive, periods are unioned by start date rather than the remote overwriting local. This prevents data loss when both devices have made changes since last sync.
- **4-day nearby period lookahead**: when tapping a day just after a period ends, the modal offers to extend that period. The 4-day window was chosen as a practical balance — enough to catch a period that ran a day or two longer than logged, not so wide that it creates confusing suggestions.
- **Circular day cells**: matches iOS Calendar visual language, more finger-friendly than squares.
- **Color-only phase encoding**: no text labels in calendar cells (they were tried and were too small to read on iPhone). Color alone carries the phase information; the legend at the top provides the key.

## Files

```
index.html            — entire app, self-contained
apple-touch-icon.png  — 1024×1024 iOS home screen icon
favicon.ico           — multi-size browser favicon (16, 32, 48, 256)
README.md             — end-user documentation (shown on GitHub repo page)
DEPLOY.md             — developer/deployment guide including Google OAuth setup
CLAUDE.md             — this file
```

## Deployment

- Hosted on GitHub Pages at `https://rharder.github.io/cycle-tracker/`
- Deploys automatically on push to `main`
- GitHub repo: `https://github.com/rharder/cycle-tracker`
- Google Cloud project for OAuth: registered to `robertharder@gmail.com`
- Authorized JavaScript origin: `https://rharder.github.io`
