# Breathe

Minimal, Apple-style breathing PWA for settling (downregulating) and charging (upregulating) physiology.
Live: https://davecube123.github.io/breath/ (GitHub Pages; deploys on every push to `main` via `.github/workflows/pages.yml`).

Plain HTML/CSS/JS. No build step, no dependencies. Run locally with `python3 -m http.server 8000`.

## Files
- `index.html`: all views (home, journal, setup, editor, session, summary) plus sheets (check-in, step picker, install).
- `app.js`: everything else. Navigation stack with history API, library, setup, session engine, check-in, summary, journal, pattern editor, install prompt.
- `audio.js` (`window.BreathAudio`): Web Audio synthesis for phase cues, the breath-sound swell, speechSynthesis voice, and generative music (solemn, warm, pulse, ocean, bowls).
- `patterns.js` (`window.BREATH_PATTERNS`): built-in research-backed patterns, with citations. The format is documented at the top of the file.
- `sw.js`: network-first service worker. **Bump `CACHE` when shipping changes.**
- `manifest.webmanifest`, `icons/`: icons are generated with PIL (teal orb on white). Bump the `?v=` query on icon URLs when they change.

## Key behaviour
- Session timing runs off `AudioContext.currentTime`. Pausing suspends the context, so everything freezes in place.
- A pattern is made of sets. Each set has `reps` and `phases` (`in`, `in2`, `holdIn`, `out`, `holdOut`, `rest`). A hold with `open: true` lasts until the user taps, and its duration is logged.
- Timing: a single set with one rep is timed in minutes; everything else is timed in rounds.
- localStorage keys:
  - `br.prefs`: settings
  - `br.custom`: user patterns
  - `br.cfg`: per-pattern length and music
  - `br.log`: session journal (duration, breaths, holds, before/after tension and energy, note)
- The design is always light (white). Category colours: settle teal, focus indigo, charge orange.
- iOS cannot install a PWA programmatically, so the install card shows the Share → Add to Home Screen steps there. Chromium browsers use `beforeinstallprompt`.

## Conventions
- Keep it minimal. Match the existing style (2-space indent, small helpers `$`, `$$`, `h`).
- Before pushing, check the UI in Playwright (Chromium is preinstalled at /opt/pw-browsers) at a phone viewport.
- Health content must stay evidence-based. Cite sources in `patterns.js` and keep the safety note on charge patterns.
