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
