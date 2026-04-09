# Fireglass

A real-time kiln firing progress tracker for glass artists. Visualizes GlassMaster controller firing programs as a temperature-over-time chart and shows exactly where you are in the process — no more guessing.

**Live app:** [r3g0ry.github.io/Fireglass](https://r3g0ry.github.io/Fireglass/)

---

## Features

- **9 built-in programs** — Full Fuse, Tack Fuse, and Slump in SLOW / MED / FAST speeds
- **Temperature profile chart** — full firing curve with labeled phases (Pre-Process Heating, Firing, Pre-Annealing Cooling, Annealing, Final Cool)
- **Real-time progress** — animated marker shows your current position on the chart, updates every second
- **Status panel** — current phase, interpolated temperature, elapsed time, time remaining, estimated finish time
- **Custom start time** — enter the time you actually started the kiln, even if you opened the app late
- **Session persistence** — start time saved in browser storage; refresh the page without losing your place
- **Editable programs** — adjust any segment's Rate, Target Temp, or Hold time to match your exact controller settings
- **°F / °C toggle** — switch display units at any time
- **No install** — single HTML file, works offline after first load

---

## How to use

1. **Select a program** — choose type (Full Fuse / Tack Fuse / Slump) and speed (SLOW / MED / FAST)
2. **Set the start time** — defaults to now; change it if you started the kiln earlier
3. **Press START FIRING** when (or after) you turn the kiln on
4. **Watch the chart** — the white dashed line moves in real time
5. **Press STOP** when the session ends or to reset

---

## Firing programs

Each program follows the standard GlassMaster Ramp-Hold format: 8 segments, each with a **Rate** (°F or °C per hour), a **Target Temperature**, and a **Hold Time**.

| Phase | Segments | Description |
|---|---|---|
| Pre-Process Heating | 1–3 | Gradual warm-up ramps with holds |
| Firing / Process | 4–5 | Ramp to peak temperature and hold |
| Pre-Annealing Cooling | 6 | Rapid drop to annealing range (rate: MAX) |
| Annealing | 7 | Slow controlled cooling hold |
| Final Cooling | 8 | Cool to near room temperature |

### Editing a program

Open the **Program Segments** panel at the bottom of the page and click **✎ Edit**. Rate, Target Temp, and Hold fields become editable. Click **✔ Save** to apply — changes are stored in your browser and the chart updates immediately. A **Modified** badge appears on edited programs. Use **↺ Reset Default** to restore factory values.

---

## Approximate firing times

| Program | Duration |
|---|---|
| Full Fuse SLOW | ~14 hours |
| Full Fuse MED | ~9 hours |
| Full Fuse FAST | ~5 hours |
| Tack Fuse SLOW | ~10 hours |
| Tack Fuse MED | ~7 hours |
| Tack Fuse FAST | ~4 hours |
| Slump SLOW | ~13 hours |
| Slump MED | ~8 hours |
| Slump FAST | ~4 hours |

---

## Tech

Single HTML file — Chart.js 4.4.3 via CDN for the chart, everything else is vanilla JavaScript. No framework, no build step, no server required.

---

## Development

```bash
# clone
git clone https://github.com/r3g0ry/Fireglass.git
cd Fireglass

# open locally
open index.html   # macOS
# or just drag index.html into a browser

# deploy to GitHub Pages
git checkout gh-pages
git merge claude/glass-fire-progress-viz-Chpaa
git push
git checkout claude/glass-fire-progress-viz-Chpaa
```
