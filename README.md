# KXVN Projects

A showcase of the personal projects, services, and infrastructure I run on my own VPS.

🌐 **[kxvn.io](https://kxvn.io)** — personal landing page, the front door to everything below.

Self-hosted on a Linux VPS behind Nginx + Cloudflare, accessed remotely over Tailscale.

---

## 🌟 Featured

| Project | Description | Stack |
|---|---|---|
| [**kxvn.io**](https://kxvn.io) | Click-to-enter splash page with audio, live Discord presence via Lanyard, Spotify now-playing, avatar decorations, mute controls, and Minecraft-style typography. The public front door for the whole stack. | Vanilla HTML/CSS/JS · Lanyard API · Nginx |
| **Hermes** | AI agent framework that runs as one core across CLI, a multi-platform gateway (Discord, Telegram, WhatsApp, SMS, web dashboard), and ad-hoc shell sessions. Long-term memory via a personal knowledge graph, code awareness via a code-structure index, both connected through MCP. | Python · MCP · Ollama (local + cloud) · systemd |

---

## 🛠 Apps & Tools

| Project | What it does | Stack |
|---|---|---|
| **Jellyfin** | Personal media library with a full automation pipeline. Tracks wanted TV shows and movies, grabs releases from Usenet, sorts them into the right folders, renames them, and refreshes the library automatically. Hardware-accelerated transcoding when available, software fallback otherwise. | Jellyfin · Sonarr · Radarr · SABnzbd · Prowlarr · qBittorrent · Bazarr · Unpackerr · custom ffmpeg images · Docker |
| **MediaV2** | Streaming platform for movies and TV with TMDB metadata, watch history, AI-powered search, Chromecast support, AniList integration, custom subtitles, skip-intro, and an admin dashboard. Installs as a PWA. | React · VidNest · TMDB API · AniList API · pm2 + systemd |
| **CDN** | Self-hosted file hosting with upload, share, and preview. Drag-and-drop JS injection at the nginx edge for the snappy upload UX. Built on a popular self-hosted uploader, restyled and re-engineered for the KXVN stack. | Zipline v4 · Docker · Nginx |
| **PDF Editor** | About 50 PDF tools: merge, split, rotate, compress, OCR, convert, sign, redact, compare. Login-gated with built-in auth, rebranded end-to-end so it never shows the upstream name. | Stirling-PDF (Docker) · Nginx |
| **KXVN Trades** | LLM-driven trading research. Natural-language prompts trigger multi-agent debate over market data and return a paper-trade plan. Paper-only by design — no broker keys, no real-money execution. Rebranded from an upstream project. | FastAPI · React 19 · Vite · Tailwind · ECharts · Ollama Cloud · Nginx |
| **Collectr-Price** | Pokémon TCG price scraper and calculator. Search by name, pulls pricing from Collectr, shows grade and rarity, links out to listings. | Python |
| **Discord Bot** | Universal bot starter: message + slash commands + HTTP app API. AI chat integration, gambling mini-games, reminders, Spotify controls, polls, home-automation hooks, and a live stats dashboard. | Node.js · Discord.js · pm2 |
| **IPTV Live** | Browse 9,000+ free live TV channels by country and category, HLS playback in the browser, backend Express proxy for CORS-free streaming. Local-only by design — not exposed publicly. | React 19 · Vite · hls.js · Express · iptv-org API |
| **Homeserver** | Infrastructure and deployment templates: Docker Compose files, Nginx site configs, firewall rules, lockdown scripts, an inventory of what's running, and runbooks for migrations. | Docker · Nginx · Bash |

---

## 🤖 AI Layer (private)

| Service | Role |
|---|---|
| **Hermes Agent** | Multi-platform gateway: Discord, Telegram, WhatsApp, SMS, web |
| **GBrain** | Personal knowledge graph (PGLite, embedded embeddings) — Hermes's long-term memory |
| **CodeGraph** | Code structure index, queryable via MCP — lets the agent read its own code |
| **Ollama** | Local model server on the host, with cloud fallback for bigger jobs |

---

## 📦 Repos

| Repo | Visibility | Notes |
|---|---|---|
| `kishyxd/projects` | Public | This showcase |
| `kishyxd/services` | Private | Glance, plausible, filebrowser, ntfy, pairdrop, etc. |
| `kishyxd/homeserver` | Private | Infra templates + runbooks |
| `kishyxd/pdf` | Private | PDF editor project |
| `kishyxd/kxvn-site` | Private | The kxvn.io landing page source |

---

🔒 *Source code for every project is private. This repo is the public showcase.*

See them all live at **[kxvn.io](https://kxvn.io)**.
