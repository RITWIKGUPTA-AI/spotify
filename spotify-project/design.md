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
