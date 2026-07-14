# 🚀 Launch

> A personal hub linking out to the dashboards I actually keep open — F1, Tour de France, World Cup, and my stock portfolio — one shared control panel instead of four disconnected bookmarks.

**Live site:** https://mattt-lab.github.io/launch/

---

## What it does

A single page of "channel" tiles, each linking straight to one of the other apps. Every tile shows a live status pulled from that app's own data — not a static label — so at a glance you can see what's actually happening before you click in.

| Tile | Links to | Status shown |
|---|---|---|
| F1 Dashboard | `f1-dashboard.github.io` | Race week, or a countdown to the next Grand Prix |
| Tour de France | `tour-de-france` | Current stage number, rest day, or a countdown to the Grand Départ |
| World Cup 2026 | `world-cup` | Live match, today's stage, or a countdown to the next one |
| Portfolio Tracker | `portfolio-tracker` (Vercel) | Market open / after hours / pre-market / closed |

---

## Design

Built to read as an extension of the sites it links to, not a separate product: the same near-black background, system font, and uppercase mono labels that F1/TdF/World Cup already share. Each tile's top edge and status dot use that app's real accent color (F1 red, TdF yellow, World Cup green), plus indigo introduced here for Portfolio Tracker.

The background grid animates with a slow, small sine-wave ripple — canvas-drawn rather than CSS, since a real wave means displacing individual points along each line, not just panning a background-image. It's paused automatically for `prefers-reduced-motion` and whenever the tab isn't visible.

---

## How live status works

- **F1** — fetches the public Jolpica/Ergast API (the same one the F1 dashboard itself uses) and checks whether today falls within a race weekend window.
- **Tour de France** — fetches `route.json` straight from the TdF site (same `mattt-lab.github.io` domain, so no CORS setup needed) and matches today's date against the stage list.
- **World Cup** — fetches `matches.json` from the World Cup site: checks for a live match first, then today's fixtures, then the next upcoming one.
- **Portfolio Tracker** — computed locally from NYSE trading hours; no fetch involved.

If any fetch fails, that tile falls back to just the app's plain name rather than showing stale or broken status text.

---

## Tech stack

| Layer | Choice |
|---|---|
| Hosting | GitHub Pages (static) |
| Frontend | Vanilla HTML/CSS/JS — single file, no framework, no build step |
| Status data | Same-origin fetches to F1/TdF/World Cup's own committed JSON; local NYSE-hours calculation for Portfolio Tracker |
| Background | Canvas-animated grid, respects `prefers-reduced-motion` |

---

*Built for personal use — a jumping-off point for the apps at [mattt-lab](https://github.com/mattt-lab), not a general-purpose product.*
