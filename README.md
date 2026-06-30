# KXVN Projects

Personal projects, services, and infrastructure built by KXVN.

Featured hub: kxvn.io — personal landing page and entry point to everything below.

Self-hosted on a Linux VPS behind Nginx + Cloudflare, accessed remotely over Tailscale.

---

## Featured

### kxvn.io — Personal landing page
Click-to-enter splash with audio, live Discord presence via Lanyard, Spotify now-playing, avatar decorations, mute controls, Minecraft-style typography. The public front door for the whole stack.
**Stack:** Vanilla HTML/CSS/JS · Lanyard API · Nginx

### Hermes — Personal AI agent
AI agent framework that runs as one core across CLI, a multi-platform gateway (Discord, Telegram, WhatsApp, SMS, web dashboard), and ad-hoc shell sessions. Plugs into a personal knowledge graph and a code-structure index via MCP so the agent has long-term memory and can read its own code.
**Stack:** Python · MCP · Ollama (local + cloud) · systemd

---

## Apps & Tools

### MediaV2 — Personal streaming platform
Movies and TV with TMDB metadata, watch history, AI-powered search, Chromecast support, AniList integration, custom subtitles, skip-intro, and an admin dashboard. Installs as a PWA.
**Stack:** React · VidNest · TMDB API · AniList API · pm2 + systemd

### Jellyfin — Personal media library + automation stack
Self-hosted media server with a full automation pipeline: tracks wanted TV shows and movies, grabs releases from Usenet, sorts them into the right folders, renames them, and refreshes the library automatically. Hardware-accelerated transcoding when available, software fallback otherwise.
**Stack:** Jellyfin · Sonarr · Radarr · SABnzbd · Prowlarr · qBittorrent · Bazarr · Unpackerr · custom ffmpeg images · Docker

### CDN — Self-hosted file hosting
File upload, share, and preview with drag-and-drop injection at the nginx edge. Built on a popular self-hosted uploader, restyled and re-engineered for the KXVN stack.
**Stack:** Zipline v4 · Docker · Nginx

### PDF Editor — Self-hosted PDF toolkit
About 50 PDF tools: merge, split, rotate, compress, OCR, convert, sign, redact, compare. Login-gated with built-in auth, rebranded end-to-end.
**Stack:** Stirling-PDF (Docker) · Nginx

### KXVN Trades — LLM-driven trading research
Natural-language prompts trigger multi-agent debate over market data, then return a paper-trade plan. Paper-only by design — no broker keys, no real-money execution. Rebranded from an upstream project.
**Stack:** FastAPI · React 19 · Vite · Tailwind · ECharts · Ollama Cloud · Nginx

### Collectr-Price — Pokémon TCG price lookup
Card price scraper and calculator. Search by name, pulls pricing from Collectr, shows grade and rarity, links out to listings.
**Stack:** Python

### Discord Bot — Universal bot starter
Message + slash commands + HTTP app API. Includes AI chat integration, gambling mini-games, reminders, Spotify controls, polls, home-automation hooks, and a live stats dashboard.
**Stack:** Node.js · Discord.js · pm2

### IPTV Live — Stream 9,000+ free channels
Browse live TV by country and category, HLS playback, backend Express proxy for CORS-free streaming. Local-only by design.
**Stack:** React 19 · Vite · hls.js · Express · iptv-org API

### Homeserver — Infrastructure & deployment templates
Docker Compose templates, Nginx site configs, firewall rules, lockdown scripts, an inventory of what's running, and runbooks for migrations.
**Stack:** Docker · Nginx · Bash

---

## AI Layer (private)

| Service | What it does |
|---|---|
| Hermes Agent | Gateway: Discord, Telegram, WhatsApp, SMS, web |
| GBrain | Personal knowledge graph (PGLite, embedded embeddings) |
| CodeGraph | Code structure index, queryable via MCP |
| Ollama (local) | Model server on the host, plus cloud fallback |

---

Source code for every project is private. This repo is the public showcase.
