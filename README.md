# Coy

**Live Demo:** [https://coy-discoverr-tab.vercel.app](https://coy-discoverr-tab.vercel.app)

Coy is a mobile-first, privacy-first social discovery and matching application built with React, TypeScript, Tailwind CSS, Express, and SQLite. Every interaction upholds Coy's core privacy rule: **nobody ever learns who liked them unless the interest is mutual.**

---

## Core Features

- **Home & Discover:** Clean 4-tab bottom bar (`Home` blank placeholder tab, default `Discover` tab, `Matches`, and `Profile`). `Discover` provides a swipe deck of 32 seeded profiles with horizontal drag/buttons/keyboard **Nope** & **Like**, upward **Fav (Super Like)**, full profile detail sheets, and a **Filter** section (age range, distance, verified-only). Swiped profiles never reappear.
- **Matches & Messaging:** Mutual-only matches view (starting with 0 matches until the user mutually likes a seeded admirer) with `All` / `Favs` filter, live search, real-time 1-on-1 chat (with contextual replies from matched profiles), read receipts, unmatching, and block/report safety tools.
- **Profile & Blue Check Camera Verification:** Edit up to 3 photos, bio, role/major, location, interests, and Q&A prompts, preview your live card, and capture a live front-camera verification selfie (`POST /api/profile/verify`) for a verified badge.

---

## Tech Stack

- **Frontend:** React 19, TypeScript, Tailwind CSS v4, Motion (`motion/react`), Lucide icons, Vite
- **Backend:** Express (`server.ts` / `lib/api-app.ts`) with REST routes and SSE event streaming
- **Database:** SQLite via `better-sqlite3` (`profiles`, `swipes`, `preferences`, `messages`, `blocks`, `reports`)
- **Testing:** Vitest (in-memory SQLite + on-disk persistence suite)

---

## Running Locally

```bash
# Install dependencies
npm install

# Start development server on port 3000
npm run dev

# Reset & reseed the 32 profiles (0 initial matches, 10 incoming likes)
npm run reset

# Run backend & privacy test suite
npm test

# Typecheck and build for production
npm run lint && npm run build
npm start
```

---

## API Overview

- **Discovery & Filters:** `GET /api/feed`, `POST /api/swipe`, `GET/PUT /api/preferences`
- **Matches & Chat:** `GET /api/matches`, `DELETE /api/matches/:targetId`, `GET /api/matches/:targetId/messages`, `POST /api/matches/:targetId/messages`, `POST /api/matches/:targetId/read`, `GET /api/events`
- **Profile, Verification & Safety:** `GET/PATCH /api/profile/me`, `POST /api/profile/verify`, `POST /api/users/:targetId/block`, `POST /api/users/:targetId/report`

---

## Deployment Notes

- **Vercel (`vercel.json` + `api/index.ts`):** Routes `/api/*` to the Express serverless function and stores SQLite in `/tmp/coy.db` (ephemeral across cold starts/instances).
- **Persistent Server (Render, Railway, Fly.io, or VPS):** Run `npm run build && npm start` for full on-disk `data/coy.db` persistence and continuous SSE streaming.
