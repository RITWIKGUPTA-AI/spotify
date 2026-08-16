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
