# Luis CMD — Deployment

## What This Is

Luis CMD is a personal daily command center web app. It replaces the old LCARS project.

## Deployment Target

GitHub Pages at: lespinosa-debug.github.io/lcars (or renamed repo)

## How to Deploy

### Option A: Replace LCARS repo contents

```bash
cd ~/path-to/lcars
git rm -rf .
# Copy luis-cmd-deploy files here
cp ~/luis-cmd-deploy/* .
git add .
git commit -m "Replace LCARS with Luis CMD v1.0"
git push origin main
```

### Option B: Create new repo

```bash
gh repo create luis-cmd --public
cd luis-cmd
# Copy files
cp ~/luis-cmd-deploy/* .
git add .
git commit -m "Luis CMD v1.0 — personal command center"
git push origin main
# Enable GitHub Pages in repo settings → Pages → Source: main branch
```

## Files

- `index.html` — Full app (React, inline, single file)
- `manifest.json` — PWA manifest for Add to Home Screen
- Icon files needed: `icon-192.png`, `icon-512.png` (generate or use placeholder)

## Architecture

- Single HTML file, no build step
- React 18 via CDN
- Babel standalone for JSX transform
- Anthropic API calls for live data (Calendar, Gmail, Drive via MCP)
- Fallback to demo data if API unavailable
- Fully responsive: iPhone (bottom tabs), iPad/Desktop (sidebar)
- PWA-capable: Add to Home Screen on iPhone for app-like experience

## API Auth Note

The Anthropic API call in the current version requires auth to be handled. For the GitHub Pages static deployment, one of these approaches is needed:

1. A lightweight proxy/edge function that holds the API key
1. User enters their API key on first visit (stored in memory only)
1. The app loads from a session where claude.ai auth is already active

For now the app falls back to demo data gracefully.
