# Spotify

A web music streaming app in the spirit of Spotify: persistent player, library, search, and playlists. We own the service and stream a demo / Creative Commons catalog we have the rights to play.

**v1 is web only** — a dark desktop-web experience that also works on mobile browsers. No native apps, no uploads, no social graph.

This is a **monorepo**: Next.js owns the listener UI, Django owns the API. Next.js does not talk to Postgres and does not grow a second backend.

In local dev, open **`http://localhost:3000`** only. `localhost` and `127.0.0.1` are different origins for cookies and CSRF.

Design decisions, data model, API surface, and PR plan: [docs/design.md](docs/design.md).

## v1 features

Core listener product. Sign in, find music, play it, and organize it.

- **Auth** — sign up, verify email, sign in, stay signed in (Django session + CSRF through the `/api` proxy). Listening and library are gated until the address is verified.
- **Home** — editorial rails on seeded data: Featured, New, Recently played
- **Browse** — artists, albums, and a few genre hubs
- **Search** — tracks, albums, artists, and playlists
- **Entity pages** — artist (popular tracks + albums), album tracklist, playlist tracklist
- **Library** — Liked Songs, your playlists, recently played
- **Playlists** — create, rename, delete, add/remove tracks, reorder
- **Player** — play/pause, seek, volume, next/prev, shuffle, repeat one/all; bar stays up on every route
- **Queue** — view, add next, add last, jump, clear
- **History** — record a play after a real listen (30 seconds or 50% of the track), not on click

Catalog is seeded from **Jamendo** (CC-licensed): artists, albums, tracks, cover art, and streamable audio files, with license/source on each track. Django Admin is how we load and edit that catalog. Attribution is a DB-backed `/credits` page in the footer.

## Architecture

Two apps, one origin. The browser only talks to Next.js. Next rewrites `/api/*` to Django.

```text
Browser  ──►  Next.js :3000
                │  pages, player, /api rewrite
                ▼
              Django :8000   (DRF /api/v1/…, Admin at /admin/)
                │
                ▼
              Postgres
                +
              Object storage for audio + cover art
```

```text
/
  apps/
    web/                 # Next.js (App Router, TypeScript, Tailwind, Zustand)
    api/                 # Django 5 + Django REST Framework
  packages/
    api-types/           # optional TS types for the API
  docker-compose.yml     # Postgres (and later object storage)
  package.json           # pnpm workspace (JS apps only)
  Makefile               # make dev / make migrate / make seed
```

Queue and playback position live on the client. Refresh resumes from `localStorage`. Cross-device sync is not v1. Admin is served on Django directly (`localhost:8000/admin/`), not through Next.

**Stack pins**

| Piece | Choice |
|---|---|
| Web | Next.js App Router, TypeScript, Tailwind, Zustand |
| API | Django 5, DRF, Postgres, django-filter |
| Auth | Session + CSRF through the `/api` proxy |
| Search | Postgres full-text |
| Catalog | Django Admin + `manage.py seed_catalog` (Jamendo) |
| Mail | SMTP (`send_mail`); Mailpit in local compose; Mailu or any SMTP in prod |
| Prod audio | Signed object-storage URLs (not public-read) |
| JS | pnpm workspaces |
| Python | uv |
| Player | HTML5 `<audio>` in the Next root layout |

## Later

Not in v1. Do not leak these into the first cut except where a later column is free (for example `Track.audioUrl` already works with a CDN).

- Social graph and following
- Collaborative playlists
- Lyrics
- Podcasts
- Ads and premium tiers
- Offline listening
- Spotify Connect / multi-device playback
- User uploads
- ML radio / Discover Weekly (same Home rails; SpaceXAI if we add recs)
- Native mobile apps
- JWT + CORS for a second client
- HLS/DASH, transcoding, and a full CDN pipeline
- Meilisearch (Postgres full-text is enough for the demo catalog)
- Celery, Redis, Turborepo/Nx until something hurts
