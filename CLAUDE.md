# CLAUDE.md — Fireglass Project Context

## What this project is

Fireglass is a single-page web app for tracking glass kiln firing sessions. It visualizes GlassMaster controller firing programs as a temperature-vs-time chart and shows real-time progress during an active firing session.

## Repository structure

```
Fireglass/
├── index.html      ← entire app (HTML + CSS + JS, self-contained)
├── CLAUDE.md       ← this file
└── README.md       ← user-facing documentation
```

No build tools, no dependencies to install. The only external resource is Chart.js loaded via CDN.

## Branches

| Branch | Purpose |
|---|---|
| `claude/glass-fire-progress-viz-Chpaa` | Active development branch |
| `gh-pages` | Production deployment (GitHub Pages) |

**Never push directly to `gh-pages`.** Merge from the dev branch only.

## Deploy workflow

```bash
git checkout gh-pages
git merge claude/glass-fire-progress-viz-Chpaa
git push
git checkout claude/glass-fire-progress-viz-Chpaa
```

Or just tell Claude: **"deploy"**

## Live URL

`https://r3g0ry.github.io/Fireglass/`

## Key code sections in index.html

| Line range (approx) | What it contains |
|---|---|
| `<style>` block | All CSS, dark fire theme, CSS variables |
| `PROGRAMS_F` object | All 9 firing programs in °F (source of truth) |
| `customPrograms` / `CUSTOM_KEY` | User-editable overrides stored in localStorage |
| `buildTimeline()` | Converts segment data to a flat list of ramp+hold phases |
| `getCurrentState()` | Interpolates current temperature and phase from elapsed time |
| `initChart()` / `refreshChart()` | Chart.js setup and redraw |
| `vertLinePlugin` | Custom Chart.js plugin for the real-time progress indicator |
| `startSession()` / `stopSession()` | Firing session lifecycle |
| `renderSegTable()` | Renders the segment data table (view + edit mode) |
| `enterEditMode()` / `saveEditMode()` | In-app program editor |

## Program data format

Each of the 9 programs (3 types × 3 speeds) has 8 segments stored in `PROGRAMS_F`:

```js
// segment: { rate: °F/hr, temp: °F, hold: minutes }
// rate = 9999 means "as fast as possible" (near-instant drop)
'Full Fuse': {
  SLOW: [ {rate, temp, hold}, ... ],  // 8 segments
  MED:  [ ... ],
  FAST: [ ... ],
}
```

Celsius values are computed on the fly from the °F data. Custom user overrides are stored in `localStorage` under `fireglass_custom` and always saved in °F.

## localStorage keys

| Key | Contents |
|---|---|
| `fireglass_session` | Active firing session `{type, speed, unit, startTime}` |
| `fireglass_custom` | User-edited program overrides (in °F) |

## Design decisions

- **Single file**: No build step, works offline, easy to deploy anywhere
- **°F as canonical unit**: All data stored in °F, Celsius is always derived
- **rate 9999**: Treated as instant (0 travel time) — creates a vertical drop on the chart
- **setInterval 1s**: Sufficient for kiln timescales (hours); `rAF` would waste CPU
- **Custom plugin** for the progress line: avoids a second CDN dependency
