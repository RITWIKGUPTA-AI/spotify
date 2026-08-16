# Spotify (workshop project) 🎧

A Spotify-*style* music player built end-to-end with **spec-driven development**
(rather than "vibe coding") — plan the rules, requirements, design, and tasks
first; let the code follow. Built as a workshop project and extended with a
full set of advanced, streaming-app-grade features.

> **Naming note:** named "Spotify" per the workshop's own `AGENT.md` example
> ("Product name is single page music player, or I can name it Spotify"). Since
> this will likely be public on GitHub/LinkedIn: "Spotify" is a live trademark,
> so treat this as a portfolio clone project, not a product you'd publish
> commercially under that name. No Spotify code, assets, or screenshots are
> used anywhere — only the general layout pattern (sidebar + shelves + list
> rows + queue drawer + bottom player) common across streaming apps. Rename
> freely — it's one string in the `<title>`/brand markup.

## Run it

No build step, no install. Pick one:

- Open `index.html` directly in a browser, **or**
- VS Code → right-click `index.html` → "Open with Live Server", **or**
- `python3 -m http.server` from this folder, then visit `http://localhost:8000`

## The spec-driven files

This repo intentionally keeps the full spec trail, matching the process taught
in the workshop — the code is the *last* step, not the first:

| File | Gate | What it is |
|---|---|---|
| `AGENTS.md` | 1 — Rules | Permanent guardrails for the project |
| `requirements.md` | 2 — Requirements | Every feature, written in EARS format (`WHEN … THE SYSTEM SHALL …`) |
| `design.md` | 3 — Design | Architecture, data model, audio engine design |
| `tasks.md` | 4 — Tasks | Ordered build phases, each task tagged with the requirement ID it implements |
| `index.html` | 5 — Implementation | The actual app |
| `Tracks.md` | — | Human-readable seed track list |

## Features

**Core player:** play/pause/next/prev, seek, volume.

**Required (from the workshop brief):**
- ❤️ Liked Songs — like any track, dedicated view
- 🕘 Recently Played — auto-tracked, most recent first
- ☰ Play Queue — add to queue, reorder, priority-plays-next
- 🌗 Light/Dark theme, persisted

**Advanced — closing the gap with real streaming apps (v4):**
- **3-band EQ** (Bass/Mid/Treble) — real `BiquadFilterNode` signal processing
  wired into the playback graph, not a cosmetic slider
- **Share / deep links** — copy a link to any track that opens straight to
  it, highlighted in the library
- **Sleep timer** — auto-pause after a chosen duration
- **Genre/Mood Browse page** — colorful tiles (Chill/Upbeat/Focus/Radio
  Mixes/Liked/Uploaded), matching Spotify's Browse grid and JioSaavn's
  genre/mood sections
- **Artist pages** — click any artist name to see everything by them

**Advanced — contest differentiation (v3):**
- **Full-screen Now Playing** — expand from the player bar into a full-screen
  view with large album art, an ambient color-matched blurred background, and
  a bigger real-time circular visualizer
- **Radio mode** — start it from the full-screen view; the app generates an
  *endless* stream of new procedurally-composed tracks in a related mood once
  your library/queue runs out, using the same synth engine as the demo tracks
  (tagged "Radio" wherever it appears in a list)
- **Stats page** — total plays, estimated listening time, top track, top mood
  — computed live from Recently Played, no separate tracking system
- **Smooth transitions** — tracks fade out over their last ~1.5s and fade in
  over their first ~1.2s instead of cutting abruptly
- **Keyboard shortcuts help** — press `?` for a modal listing every shortcut
- **Playlist backup/restore** — export any playlist as a JSON file, import it
  back later (or in another browser) via the sidebar import button

**Advanced — streaming-app UI (v2):**
- **Home screen** — time-of-day greeting, quick-access tiles, horizontally
  scrolling shelves ("Recently played," "Picked for you," mood shelves)
- **List-style Library/Liked/Playlist views** — numbered rows with hover
  actions, like a real streaming app's tracklist, not just cards
- **Animated equalizer indicator** on whatever's currently playing, in both
  shelf cards and list rows (frozen to a static icon under reduced motion)
- **Slide-out Queue panel** — "Now Playing" + "Up Next" without leaving your
  current view, plus a full Queue page for mobile
- Playlists — create, add/remove tracks, delete
- Live search across title/artist, surfaced right on Home when typing
- Shuffle (no-repeat-until-exhausted) + Repeat (one/all)
- Upload your own local audio files (session-only, never leaves your browser)
- Full keyboard control (Space, ←/→, ↑/↓, N/P, L, M, Esc, ?)
- Installable **PWA**, works offline after first load
- Accessibility: live-region "now playing" announcements, labeled icon
  buttons, visible focus states, skip link
- Responsive down to ~360px (sidebar collapses to a bottom nav bar)

## Deploy it and get a shareable link

Two options — pick one:

### Option A — GitHub Pages (simplest, static-only, recommended)

1. Create a public GitHub repo and upload all files *except* `server.js` and
   `package.json` (those are only needed for Option B). You need:
   `index.html`, `AGENTS.md`, `requirements.md`, `design.md`, `tasks.md`,
   `Tracks.md`, `manifest.json`, `sw.js`, `README.md`.
2. In the repo: **Settings → Pages → Source → Deploy from a branch → main /
   (root)** → Save.
3. Wait ~1 minute, refresh Settings → Pages — you'll get a live URL like
   `https://yourusername.github.io/your-repo/`.
4. That one link works as: the contest submission link, the GitHub repo link
   (for the spec files), and something you can post on LinkedIn.

No config changes needed — everything uses relative paths already.

### Option B — Render, with the included Express server

`server.js` + `package.json` turn this into a tiny Node/Express app instead
of a purely static site — use this if you want to show a working backend as
part of the submission.

1. Push the **entire folder** (including `server.js` and `package.json`) to
   a public GitHub repo.
2. Go to [render.com](https://render.com) → New → Web Service → connect that
   repo.
3. Build command: `npm install`. Start command: `npm start`. Leave the port
   field default — `server.js` already reads `process.env.PORT`, which
   Render sets automatically.
4. Deploy. Render gives you a live URL like
   `https://your-service-name.onrender.com`.

Render's free tier spins down after inactivity and takes ~30–60s to wake up
on the next visit — worth knowing if a judge opens it after it's been idle.

## Honest gap vs. real streaming platforms (open.spotify.com, JioSaavn)

Worth stating plainly rather than glossing over — this is a frontend-only
project, so some things are structurally out of reach without a backend:

- A real licensed music catalog (ours is legally generated instead — see below)
- User accounts, login, or syncing your library across devices
- Social features: following friends, collaborative playlists, activity feed
- Casting to speakers/TVs (Spotify Connect–style)
- Podcasts, or charts driven by real aggregate listening data
- A native mobile app (this is a PWA, the closest honest equivalent)

Everything else feasible for a static, no-backend app has been built in:
full-screen player, generative Radio, EQ, sleep timer, genre/mood Browse,
artist pages, shareable deep links, stats, playlists, queue, and the full
required feature set.

## Why no real songs?

The 8 demo tracks aren't audio files at all — they're generated **in the
browser** on first play, using the Web Audio API (`OfflineAudioContext` +
oscillators, rendered to a WAV blob). This sidesteps any music licensing issue
entirely while still demonstrating real, working audio playback, seeking, and
a live visualizer driven by the actual output signal. Drop in your own MP3s
via the **Upload** button for real music.

## Extending it

Following the same process the workshop taught: add a new requirement to
`requirements.md` in EARS format, note the design change in `design.md`, add a
task in `tasks.md` referencing that requirement ID, *then* touch `index.html`.

