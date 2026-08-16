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
