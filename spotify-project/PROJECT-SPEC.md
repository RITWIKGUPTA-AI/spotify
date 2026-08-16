# Spotify (workshop project) — Combined Project Document

This single file merges every spec-driven document in this project, in the
order they were written (README → guardrails → requirements → design →
tasks → seed data), so the whole project can be read or submitted as one
document. The actual app is `index.html`; an optional Express server
(`server.js` + `package.json`) is included for platforms that want a Node
backend instead of pure static hosting. This file documents the process and
decisions behind it.

---


<!-- ============================================================ -->

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



<!-- ============================================================ -->

# AGENTS.md — Project Guardrails

These are permanent rules for this project. They do not change once set, unless a
new rule is explicitly added here first (per spec-driven development: change the
spec, not the code, and never the other way around).

## Product

- Product name: **Spotify** (workshop project) — a browser-based, Spotify-*style* music player, per the workshop's own AGENT.md naming example. See the trademark note in README.md before publishing this publicly under that name.
- Renamed from "Spotify" deliberately: this is a portfolio/learning project meant
  to be shown publicly on GitHub/LinkedIn, and "Spotify" is a registered
  trademark. Using an original name avoids any trademark issue while keeping the
  entire spec-driven workflow taught in the class. Swap the name back in one
  place (`APP_NAME` constant) if you ever want to relabel it privately.

## Technical constraints

- Single-page app. Core app logic (HTML + CSS + JS) lives in one `index.html`,
  no framework, no bundler, no build step, no `npm install`.
- Two small auxiliary files are permitted as an explicit exception: `manifest.json`
  and `sw.js`, required for the installable/offline (PWA) requirement. They do
  not count as "a build step" — they're static files served as-is.
- Runs entirely client-side, no backend/server/database. State lives in
  `localStorage`. Any static file server (VS Code Live Server, GitHub Pages,
  `python -m http.server`) must be able to serve it with zero configuration.
- No copyrighted third-party song files are bundled. Demo tracks are generated
  procedurally in the browser using the Web Audio API (oscillators + envelopes
  rendered to short audio loops). Real songs are never required to demo the app.
  Users may optionally load their **own** local audio files via a file picker —
  those never leave the browser.
- Must run fully offline after first load (service worker caches the app shell).

## Non-negotiable UX floor

- Fully keyboard operable; every icon-only control has an accessible label.
- Visible focus states everywhere; `prefers-reduced-motion` is respected.
- Responsive from ~360px mobile width up to desktop.
- Every requirement in `requirements.md` is written in EARS format and every
  task in `tasks.md` references the requirement ID it implements.

## Explicitly out of scope for v1

- User accounts / login / multi-user sync.
- Streaming real licensed music or a paid catalog.
- Native mobile app packaging (PWA install is enough).


<!-- ============================================================ -->

# requirements.md — Spotify (workshop project)

All requirements use the EARS shape: `WHEN <trigger> THE SYSTEM SHALL <behavior>`.
Each has an ID so `tasks.md` and the code can trace back to it.

## A. Core player (baseline, from the live demo)

- **R1** WHEN the app loads THE SYSTEM SHALL display a library grid of all
  available tracks with title, artist, duration, and cover art.
- **R2** WHEN the user clicks a track THE SYSTEM SHALL start playback of that
  track and switch the play control to a pause icon.
- **R3** WHEN a track is playing and the user clicks pause THE SYSTEM SHALL
  pause playback and switch the icon back to play.
- **R4** WHEN the user clicks next/previous THE SYSTEM SHALL advance to the
  next/previous track according to the current queue and repeat/shuffle state.
- **R5** WHEN the user drags the seek bar THE SYSTEM SHALL jump playback to the
  selected position and update the displayed elapsed/remaining time.
- **R6** WHEN the user adjusts the volume control THE SYSTEM SHALL change
  playback volume immediately and persist the level for the session.

## B. Required homework features (from the workshop)

- **R7 (Liked Songs)** WHEN the user opens "Liked Songs" THE SYSTEM SHALL
  display only tracks the user has liked, in the order they were liked
  (most recent first).
- **R8 (Liked Songs — toggle)** WHEN the user clicks the heart icon on any
  track THE SYSTEM SHALL toggle its liked state and persist it across reloads.
- **R9 (Recently Played)** WHEN a track finishes starting playback THE SYSTEM
  SHALL record it in Recently Played, most recent first, deduplicated, capped
  at 50 entries, persisted across reloads.
- **R10 (Play Queue)** WHEN the user clicks "Add to queue" on a track THE
  SYSTEM SHALL append it to the play queue.
- **R11 (Play Queue — priority)** WHEN the current track ends or the user
  clicks next AND the queue is non-empty THE SYSTEM SHALL play the next queued
  track before resuming normal library order, and remove it from the queue.
- **R12 (Queue management)** WHEN the user opens the Queue view THE SYSTEM
  SHALL let them reorder or remove queued tracks.
- **R13 (Theme)** WHEN the user toggles light/dark theme THE SYSTEM SHALL
  switch the appearance immediately and remember the choice on reload.

## C. Advanced features (beyond the workshop, for a stronger submission)

- **R14 (Playlists — create)** WHEN the user creates a playlist with a name
  THE SYSTEM SHALL add it to the sidebar and persist it across reloads.
- **R15 (Playlists — manage tracks)** WHEN the user adds/removes a track from
  a playlist THE SYSTEM SHALL update that playlist's track list immediately
  and persist the change.
- **R16 (Playlists — delete)** WHEN the user deletes a playlist THE SYSTEM
  SHALL remove it and ask for confirmation first.
- **R17 (Search)** WHEN the user types in the search field THE SYSTEM SHALL
  filter the visible track list live by title or artist, case-insensitively.
- **R18 (Shuffle)** WHEN shuffle is enabled THE SYSTEM SHALL randomize the
  next-track order without repeating a track until all have played once.
- **R19 (Repeat)** WHEN repeat is set to "one" or "all" THE SYSTEM SHALL loop
  the current track or the current list accordingly when playback would
  otherwise stop.
- **R20 (Upload)** WHEN the user selects a local audio file THE SYSTEM SHALL
  add it to the library as a playable track for the current session, tagged
  "Uploaded," without uploading the file anywhere.
- **R21 (Visualizer)** WHILE a track is playing THE SYSTEM SHALL render a
  real-time audio-reactive visualizer driven by the actual output signal.
- **R22 (Keyboard control)** WHEN the user presses Space/←/→/↑/↓/N/P/L/M
  outside a text field THE SYSTEM SHALL play-pause / seek / adjust volume /
  skip next-prev / toggle like / toggle mute respectively.
- **R23 (Offline/PWA)** WHEN the app has been loaded once online THE SYSTEM
  SHALL remain fully usable offline on a subsequent visit, and SHALL be
  installable to the home screen/desktop.
- **R24 (Accessibility)** WHEN a screen reader user navigates the app THE
  SYSTEM SHALL announce the current now-playing track via a live region and
  expose every icon-only control with an accessible name.
- **R25 (Responsive layout)** WHEN the viewport is narrower than 720px THE
  SYSTEM SHALL collapse the sidebar into a bottom navigation bar and stack the
  now-playing bar to fit the width without horizontal scrolling.

## D. Advanced UI — streaming-app parity (v2, referenced from open.spotify.com's layout patterns)

- **R26 (Home screen)** WHEN the app loads THE SYSTEM SHALL show a Home view
  with a time-of-day greeting, quick-access tiles (Liked Songs, recent
  playlists), and horizontally-scrolling shelves ("Recently played," "Picked
  for you," mood-based shelves) instead of a flat grid.
- **R27 (List rows)** WHEN the user opens Library, Liked Songs, or a playlist
  THE SYSTEM SHALL display tracks as numbered list rows (index, art, title/
  artist, duration, actions revealed on hover) rather than only cards.
- **R28 (Now-playing indicator)** WHILE a track is playing THE SYSTEM SHALL
  show an animated equalizer icon next to that track's row/card instead of a
  static highlight, and a static icon when reduced motion is preferred.
- **R29 (Queue side panel)** WHEN the user opens the queue panel from the
  player bar THE SYSTEM SHALL slide in a right-hand panel showing the current
  track and "Up Next," without navigating away from the current view.
- **R30 (Branding)** THE SYSTEM SHALL display the product name "Spotify" per
  the workshop's AGENT.md example, with a one-line trademark note in the
  README since this is a public portfolio project.

## E. Advanced v3 — contest-differentiation features

- **R31 (Full-screen Now Playing)** WHEN the user expands the player THE
  SYSTEM SHALL show a full-screen view with large album art, an ambient
  color-matched background, and a larger real-time visualizer, without
  interrupting playback.
- **R32 (Radio mode)** WHEN the user starts Radio from a track THE SYSTEM
  SHALL keep generating and playing new procedurally-composed tracks in a
  related mood indefinitely once the queue and library are exhausted, until
  the user stops Radio.
- **R33 (Listening stats)** WHEN the user opens Stats THE SYSTEM SHALL show
  total plays, estimated listening time, top track, and top mood, computed
  from Recently Played.
- **R34 (Shortcuts help)** WHEN the user presses `?` THE SYSTEM SHALL show a
  modal listing every keyboard shortcut; Escape or the close button dismisses it.
- **R35 (Transition fade)** WHEN a track is about to end naturally THE SYSTEM
  SHALL fade its volume out over its final ~1.5s and fade the next track in
  over its first ~1.2s, instead of cutting abruptly.
- **R36 (Playlist backup/restore)** WHEN the user exports a playlist THE
  SYSTEM SHALL download it as a JSON file; WHEN the user imports a previously
  exported JSON file THE SYSTEM SHALL recreate the playlist from tracks
  currently in the library, skipping any that no longer exist.

## F. Advanced v4 — closing the gap with real streaming apps

- **R37 (3-band EQ)** WHEN the user adjusts the Bass/Mid/Treble sliders THE
  SYSTEM SHALL apply real-time audio filtering to playback via the Web Audio
  API, not just a cosmetic control.
- **R38 (Share / deep link)** WHEN the user clicks Share THE SYSTEM SHALL
  copy a link that, when opened, navigates directly to that track in the
  library and highlights it.
- **R39 (Sleep timer)** WHEN the user sets a sleep timer THE SYSTEM SHALL
  automatically pause playback once the chosen duration elapses.
- **R40 (Genre/Mood Browse)** WHEN the user opens Browse THE SYSTEM SHALL
  show colorful tiles for each mood/category (Chill, Upbeat, Focus, Radio
  Mixes, Liked, Uploaded), each linking to a filtered track list.
- **R41 (Artist pages)** WHEN the user clicks an artist's name anywhere THE
  SYSTEM SHALL navigate to a page listing every track by that artist across
  the whole library (demo, uploaded, and radio-generated).


<!-- ============================================================ -->

# design.md — Spotify (workshop project)

## File shape

```
index.html      -- everything: markup, <style>, <script>. One file, no build step.
manifest.json   -- PWA metadata (R23)
sw.js           -- app-shell service worker (R23)
Tracks.md        -- human-readable seed track list (mirrors the DEMO_TRACKS array)
```

## Data model

```js
Track = {
  id: string,
  title: string,
  artist: string,
  duration: number,       // seconds
  color: string,          // hex, drives the vinyl label + card art gradient
  patch: "pad" | "arp" | "pulse" | "bell" | "keys" | "bass",  // synth patch for R.C procedural audio
  src: string | null,     // Blob URL, filled in lazily on first play
  uploaded: boolean
}

State (persisted to localStorage under key "wavelength:v1") = {
  theme: "light" | "dark",
  liked: string[],            // track ids, most-recent-first
  recent: {id, at}[],         // capped at 50
  queue: string[],            // track ids
  playlists: { id, name, trackIds: string[] }[],
  volume: number,             // 0..1
  repeat: "off" | "one" | "all",
  shuffle: boolean
}
```

Uploaded tracks (R20) are **not** persisted — their Blob URLs die on reload
anyway, so they live in an in-memory array for the session only. The UI says
so explicitly rather than pretending otherwise.

## Audio engine

- One `<audio id="player">` element is the single source of truth for playback
  (gives real seek/duration/volume for free, and works identically for
  procedural and uploaded tracks).
- One `AudioContext`, created lazily on first user gesture (autoplay policy).
  A single `MediaElementAudioSourceNode` wraps the `<audio>` element, fans out
  to an `AnalyserNode` (for the visualizer, R21) and to `audioCtx.destination`.
- **Demo tracks** are never bundled as files. On first play, each demo track is
  rendered once via `OfflineAudioContext` (a short generative loop — pad,
  arpeggio, pulse-bass, bell, keys, or bass patch depending on `track.patch`),
  encoded to a WAV `Blob`, and cached on the track object as `src`. This keeps
  the app copyright-clean and is itself the "advanced" showcase feature.
- **Uploaded tracks** (R20) get `src = URL.createObjectURL(file)` directly.

## View/router

No URL routing needed for a single page; a `currentView` string
(`"library" | "liked" | "recent" | "queue" | "playlist:<id>"`) drives which
track list renders in the main grid. Sidebar buttons set `currentView` and
re-render.

## Rendering

Plain DOM (`renderLibrary()`, `renderPlayerBar()`, `renderQueue()`,
`renderPlaylists()`, `renderVisualizer()` on `requestAnimationFrame`). No
framework, per AGENTS.md.

## Visualizer signature element

A circular ring of frequency bars around a spinning vinyl disc, colored per
track (`track.color`), instead of a generic flat bar chart — ties the
visualization to the "record" metaphor instead of a stock audio-app look.
`prefers-reduced-motion` swaps the spin/bar animation for a static state.

## v2 — Streaming-app UI layer (R26–R30)

- `currentView` gains a `"home"` state as the default landing view, separate
  from `"library"` (the full flat list). Home is composed of `buildQuickTile()`
  quick-access tiles + `buildShelf(title, tracks)` horizontal shelves; Library/
  Liked/Playlist views switch to `buildTrackRow()` numbered list rows instead
  of cards, matching how a real streaming app separates "browse" (Home) from
  "manage" (Library/playlists) surfaces.
- A single reusable `.eq` animated bar element (3 CSS-animated bars) swaps in
  wherever a track is the currently-playing one, in both card and row layouts;
  `prefers-reduced-motion` freezes it to a static shape (R28).
- The queue panel (R29) is a fixed-position `aside` that slides in from the
  right (`transform: translateX`), independent of the 2-column app grid, so it
  overlays rather than reflowing the layout on desktop, and covers the screen
  on mobile. It re-renders from the same `state.queue` data as the Queue nav
  view — no separate state.
- No Spotify assets, code, or copyrighted screenshots are used anywhere —
  only the general structural pattern (sidebar + shelves + list rows + queue
  drawer + bottom player) common across streaming apps.

## v3 — Contest-differentiation layer (R31–R36)

- **Full-screen player (R31)** is a fixed-position overlay (`#fullscreenPlayer`)
  reusing the same playback state/controls as the mini bar — no separate audio
  element, just a bigger rendering of the same source of truth.
- **Radio mode (R32)** reuses `generateTrackAudio()`'s synth engine. A seed
  track's `mood` picks a patch pool; `generateRadioTrack(mood)` builds a new
  `Track` object with a randomized title (word-bank combinator), pushes it
  into a `radioTracks` array (session-only, like uploads), and `nextTrackId()`
  falls back to generating one more when radio is active and everything else
  is exhausted — genuinely infinite playback, not a fixed loop.
- **Stats (R33)** is a pure read of `state.recent`: no new persisted data, so
  it can't drift from what's actually been played.
- **Shortcuts modal (R34)** is static markup toggled by a class; `?` is
  `Shift+/` so the keydown handler checks `e.key === "?"` directly.
- **Fade transitions (R35)** are volume ramps on the single `<audio>`
  element's `.volume`, driven off the existing `timeupdate` listener (fade-out
  window) and a short `setInterval` ramp right after a new track starts
  (fade-in). This is a fade, not a true overlapping crossfade — a second
  `<audio>` element would be needed for real overlap, which was judged not
  worth the added failure surface for a workshop-grade project (documented
  here so it's a stated trade-off, not an oversight).
- **Playlist backup/restore (R36)** exports `{name, trackIds}` as JSON;
  import re-resolves `trackIds` against the current `allTracks()` list and
  drops anything missing, since uploaded/radio tracks aren't persisted
  (see Persistence layer above) and can't be restored across sessions.

## v4 — Closing the brand-parity gap (R37–R41)

- **EQ (R37)** extends the audio graph: `sourceNode → bassFilter (lowshelf,
  200Hz) → midFilter (peaking, 1kHz) → trebleFilter (highshelf, 3kHz) →
  analyser → destination`. Slider values are real `BiquadFilterNode.gain`
  values (-12..+12dB), persisted in `state.eq` and reapplied via
  `setEq(band, value)` — this is genuine signal processing, not a fake UI.
- **Share/deep link (R38)** builds `?track=<id>` off `location.origin +
  pathname`, copies it via the Clipboard API (falls back to `prompt()` if
  clipboard access is denied). On load, `checkDeepLink()` reads the query
  param, jumps to Library, and highlights the matching row. Playback isn't
  auto-started — browser autoplay policy blocks unsolicited audio, and
  respecting that is more honest than working around it.
- **Sleep timer (R39)** is a single `setTimeout`, cycling through preset
  durations on repeated clicks (Off → 15 → 30 → 45 → 60 → Off) rather than a
  custom-duration input, keeping the interaction to one button.
- **Browse (R40)** is static `BROWSE_TILES` metadata mapped over
  `listForView()` — no new data model, just a friendlier entry point into
  filters that already existed (mood, radio, uploaded, liked).
- **Artist pages (R41)** reuse the existing view-router pattern
  (`artist:<encoded name>`), filtering `allTracks()` by exact artist-string
  match. Artist names are clickable buttons in every track row.

## Persistence layer

`Store.load()` / `Store.save(state)` wrap `localStorage.getItem/setItem` with
try/catch (private-browsing / quota safety) and a version key so future
migrations don't crash on old saved state.


<!-- ============================================================ -->

# tasks.md — Spotify (workshop project)

## Phase 1 — Core player + required homework features

1. Scaffold `index.html`: layout shell (sidebar, main grid, now-playing bar), theme tokens. — R1, R13, R25
2. Seed `DEMO_TRACKS` data + procedural WAV generator (`renderTrack()` via OfflineAudioContext). — R1
3. Wire `<audio>` element + play/pause/next/prev/seek/volume controls. — R2, R3, R4, R5, R6
4. Liked Songs: heart toggle on cards + dedicated Liked view. — R7, R8
5. Recently Played: record on playback start, capped/deduped list + view. — R9
6. Queue: add-to-queue action, priority-in-next-track logic, Queue view with reorder/remove. — R10, R11, R12
7. Theme toggle, persisted. — R13
8. **Checkpoint:** open `index.html` in a live server — library renders, a track plays audibly, like/recent/queue/theme all work and survive a reload.

## Phase 2 — Advanced features

9. Playlists: create / rename via prompt, add-to-playlist menu on cards, delete with confirm, sidebar list. — R14, R15, R16
10. Search field filtering the active view live. — R17
11. Shuffle (no-repeat-until-exhausted) + Repeat (one/all) modes wired into `getNextTrackId()`. — R18, R19
12. Upload: file input, in-memory session tracks, tagged "Uploaded" in the grid. — R20
13. Visualizer: `AnalyserNode` + circular canvas ring around the vinyl, `prefers-reduced-motion` fallback. — R21
14. Global keyboard shortcuts (ignored while typing in inputs). — R22
15. `manifest.json` + `sw.js` app-shell caching; verify install prompt + offline reload. — R23
16. Accessibility pass: aria-live now-playing announcer, aria-labels on all icon buttons, focus-visible styling, skip link. — R24
17. Responsive pass: sidebar → bottom nav under 720px, now-playing bar reflow. — R25
18. **Checkpoint:** every requirement in `requirements.md` has a working, visible behavior; no console errors; Lighthouse PWA/Accessibility checks pass.

## Phase 3 — Polish for submission

19. README with setup instructions, feature list, and a note on the spec-driven files.
20. Screenshot/GIF for the repo + LinkedIn post.


<!-- ============================================================ -->

# Tracks.md

Demo tracks are **procedurally generated in-browser** (see design.md — Audio
engine) rather than bundled as files, so there's nothing to license. This is
the seed data driving both the UI cards and the generator patch per track.

| id | title              | artist            | duration | patch  | color   |
|----|--------------------|-------------------|---------:|--------|---------|
| t1 | Amber Drift        | The Night Owls    | 26s      | pad    | #E8A33D |
| t2 | Copper Skyline     | Marigold Static   | 22s      | arp    | #4FD1C5 |
| t3 | Low Tide Pulse     | Reef & Wire       | 24s      | pulse  | #C77DFF |
| t4 | Glass Bell Garden  | Ionia             | 20s      | bell   | #F26D6D |
| t5 | Velvet Keys        | Marcel & the Moon | 28s      | keys   | #6BCB77 |
| t6 | Subterranean Bass  | Deep End Society  | 25s      | bass   | #FFD166 |
| t7 | Paper Lantern      | The Night Owls    | 23s      | arp    | #E8A33D |
| t8 | Quiet Static Hymn  | Ionia             | 27s      | pad    | #4FD1C5 |

Add real songs by dropping them in via the in-app **Upload** button (R20) —
they play locally in your browser and are never sent anywhere.

