# SlugRun ⚾

Half marathon HQ for Slugga. Garmin data in, coaching out.
A-race: **Gold Coast Half Marathon — Sat 4 July 2026, 6:15am**. Then Melbourne, 11 Oct.

## What's here

- `index.html` — the whole app (Dashboard / Coach Sluggo / Profile). All CSS/JS inlined.
- `manifest.json` + `icon-*.png` — makes it installable on a phone home screen like a real app.

## Deploy (GitHub Pages — free, no domain needed)

```bash
cd slugrun
git init
git add .
git commit -m "SlugRun v0.1"
git branch -M main
git remote add origin https://github.com/jvanders33/slugrun.git
git push -u origin main
```

Then on github.com: repo → **Settings → Pages → Source: Deploy from a branch →
main / (root) → Save**. Two minutes later the app is live at:

```
https://jvanders33.github.io/slugrun/
```

Every future update is just `git add . && git commit -m "update" && git push` —
the live site refreshes automatically.

## Getting it on Slugga's phone

Send him the URL. Then:

- **iPhone:** open in Safari → Share button → **Add to Home Screen**
- **Android:** open in Chrome → ⋮ menu → **Add to Home screen / Install app**

It installs with the bat icon, opens full-screen with no browser bars, and
looks like a native app. Updates ship automatically when you push.

## Current state: v0.1 (mock data)

The app runs on a `MOCK` object inside `index.html` that mirrors the **exact
payload** the Garmin sync backend will produce. When the backend lands, flip one line:

```js
const CONFIG = {
  apiBase: "https://slugrun.yourdomain.com",  // currently null = mock mode
};
```

With `apiBase` set, the app pulls `GET /api/summary` for the dashboard and
POSTs chat to `POST /api/coach` (Claude server-side, with Slugga's real runs,
splits, HR and sleep in context).

## Backend (next build)

1. **Sync worker** — Node.js cron, `garmin-connect` npm lib, pulls activities +
   wellness + sleep into SQLite every 30 min.
2. **API** — Express: `/api/summary`, `/api/coach` (Anthropic API), `/api/history`.
3. **Training plan** — structured plan data; coach compares planned vs actual.

Garmin credentials live in `garmin.config.json` on the server — never in the
frontend, never in the repo.

Slow is smooth. Smooth is fast.
