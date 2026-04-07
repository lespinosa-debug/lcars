# LCARS — Claude Code Context
## LCARS ESPINOSA COMMAND v3 — Frontend

This is Luis Espinosa's personal LCARS (Star Trek-themed) command interface.
A single-file HTML/CSS/JS app deployed via GitHub Pages.

---

## Repo & Deploy

- **GitHub**: `lespinosa-debug/lcars`
- **Live URL**: `https://lespinosa-debug.github.io/lcars`
- **Platform**: GitHub Pages (CDN — changes take 2–5 min to propagate after push)
- **Structure**: Single file — everything lives in `index.html`

### Standard deploy command
```bash
cd ~/lcars && git add index.html && git commit -m "<message>" && git push --force
```

> Always use `--force` for frontend pushes (GitHub Pages quirk with this repo)

### Verify deploy (don't trust screenshots — check network requests)
After push, load the live site and inspect the `/api/calendar` network request URL
to confirm the new iCal URLs are present — CDN cache can lag visually.

---

## Project Structure

```
lcars/
├── index.html    ← entire app: HTML + CSS + JS, all in one file
└── CLAUDE.md     ← you are here
```

---

## Key Sections in index.html

| Section | What it does |
|---------|-------------|
| `<style id="lcars-icons">` | LCARS SVG icon system |
| `<svg style="display:none">` | SVG symbol library (delta, moon, cal, etc.) |
| CSS variables | Theme system: `--g` (gold), `--r` (red), `--b` (blue), etc. |
| Theme buttons | DAWN / DAY / TNG / DUSK / NIGHT — dissolve transitions |
| `loadCalendar()` | Fetches iCal feeds via backend proxy, renders events |
| `fetchWeather()` | Pulls weather for Coral Springs FL |
| `pollStore()` | Polls `/api/store` every 30s for live nuggets/tasks |
| `loadBriefing()` | Pulls latest Claude briefing from backend |
| `updateDayTracker()` | Day progress bar 06:00–23:00 |
| `pingAPI()` | Wakes Render backend on load |
| `replaceEmojis()` | Swaps emoji for LCARS SVG icons |

---

## Calendar Configuration

Located in `loadCalendar()` → `const URLS = [...]`

```javascript
const URLS=[
  'https://calendar.google.com/calendar/ical/lespinosa%40frostflorida.com/private-1bb59553e99af6e8fc4f4d2074182626/basic.ics',  // 0 orange  FROST WORK
  'webcal://p135-caldav.icloud.com/published/2/MTAzNTE5Mzk4NDIxMDM1MRH7oMYq2vnBm9PGGEMy-6_OjOARgb-NBp5vXUkh4D1m',             // 1 red     LCARS
  // 'URL_HERE',  // 2 purple  PERSONAL
  // 'URL_HERE',  // 3 blue    SKYE & LUIS
  // 'URL_HERE',  // 4 green   SCHOOL
  // 'URL_HERE',  // 5 yellow  FAMILY
].filter(Boolean);
```

### Color index map
| Index | Color | Calendar |
|-------|-------|---------|
| 0 | Orange `#FF9900` | FROST WORK |
| 1 | Red `#FF3333` | LCARS |
| 2 | Purple `#AA44FF` | PERSONAL |
| 3 | Blue `#4499FF` | SKYE & LUIS |
| 4 | Green `#00FF88` | SCHOOL |
| 5 | Yellow `#FFCC66` | FAMILY |

---

## Backend Connection

```javascript
const API_URL = 'https://lcars-api.onrender.com';
```

All API calls go through this — calendar proxy, store, briefing, SMS webhook.

---

## Theme System

Five themes controlled by CSS class on `<body>`:
- `dawn` — warm orange/amber
- `day` — bright gold/white  
- `tng` — classic TNG blue/grey
- `dusk` — deep orange/purple
- `night` — dark purple/black

Theme persists via `localStorage`. Dissolve transition on switch.

---

## LCARS Design Rules

- **Font**: `Antonio` (Google Fonts) — all caps, wide letter spacing
- **Colors**: Always use CSS variables, never raw hex in JS
- **Icons**: Always use the SVG symbol system (`ico()` helper), never emoji in rendered UI
- **Borders**: LCARS style — thick left bars, rounded corners on panels
- **Animations**: Subtle — blink for live indicators, dissolve for theme switches
- **Mobile**: Optimized for iPhone — `viewport-fit=cover`, 2x font scaling, no user-scale

---

## Owner Context

**Luis Espinosa (LRE)** — Video Engineer / Creative Director, Coral Springs FL
- Work: Frost Productions (`lespinosa@frostflorida.com`)
- Personal: `luislre@gmail.com`
- SMS: `+19545399989`
- Backend SMS/AI bridge: `(954) 539-9989` (Twilio)

### Terminology
- "LCARS" = this interface AND the iCloud calendar (index 1)
- "Frost Work" = work calendar (index 0)
- "Gigs" = freelance video engineering jobs
- "Nuggets" = quick intelligence/idea captures stored in backend
- "Anchors" = protected daily routines (school drop-off 06:45, kids bedtime 19:30)

---

## Key Rules

- **One file** — never split into separate CSS/JS files
- **No frameworks** — vanilla JS only, no React/Vue/etc.
- **Always `--force` push** — required for GitHub Pages on this repo
- **Test after deploy** — check network requests, not just visual
- **Preserve all existing features** when making changes — don't remove working code
- **Match the aesthetic** — every new element should look LCARS-native
- When adding calendar URLs, uncomment the placeholder at the correct index
- The webcal:// protocol must stay as-is in the frontend URLS array — backend handles conversion
