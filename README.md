# 🛳 KXVN Projects

> **One VPS. Nine services. One agent that runs the show.**

🌐 **[kxvn.io](https://kxvn.io)** — front door.
🤖 **Hermit** — the AI agent that ties it all together.
🎬 **P.I.R.A.T.E.** — the media stack in the basement.

Hosted on a single Linux VPS behind Nginx + Tailscale, with everything orchestrated by an always-on AI agent so nothing stays broken for long.

---

### 🖥️ The Box

| | |
|---|---|
| Hostname | `Hermit` |
| Kernel | Linux 7.x generic |
| CPU | Intel Xeon Gold 6152 — 16 vCPU @ 2.10 GHz, single NUMA domain |
| Memory | 32 GB DDR4 ECC · 11 GB swap |
| Local disk | Boot only. Every byte of media lives in object storage. |
| Network | Single public IP · Tailscale overlay for ops · direct egress for streaming (no Cloudflare proxy — large files + WebSockets) |
| Tunables | Hermit restarts the rclone mount + the affected containers when the FUSE bind goes stale (it does, sometimes) |

> *The whole stack is reads-against-warm-object-storage. Adding boxes adds SPOFs.*

---

## ⚙️ System Acronyms

> Every project here ships with a corporate-sounding acronym so it sounds like a Fortune 500 product, not a homelab. 😏

| System | Acronym | Stands For | What It Actually Is |
|---|---|---|---|
| **HERMIT** | `H.E.R.M.I.T.` | **H**ome **E**fficient **R**esearch **M**anagement & **I**nformation **T**ransit | The core AI gateway, automation, and management layer tying the whole stack together. |
| **PIRATE** | `P.I.R.A.T.E.` | **P**erfectly **I**ntegrated **R**epository for **A**ll **T**he **E**ntertainment | The media stack — Sonarr, Radarr, SABnzbd, Jellyfin, Prowlarr, qBittorrent, Jellyseerr. "Perfectly legal." 🤫 |

*Composed by an insufferably truthful machine.* :3

---

> 🎬 *[GIF SLOT — drop a quick screen-record of "Hermit, grab Daredevil" → show appears in Jellyfin here]*

---

## 🌟 Featured

| Project | Description | Stack |
|---|---|---|
| [**kxvn.io**](https://kxvn.io) | Click-to-enter splash page with audio, live Discord presence via Lanyard, Spotify now-playing, avatar decorations, mute controls, and Minecraft-style typography. The public front door for the whole stack. | Vanilla HTML/CSS/JS · Lanyard API · Nginx |
| **Hermes** | AI agent framework that runs as one core across CLI, a multi-platform gateway (Discord, Telegram, WhatsApp, SMS, web dashboard), and ad-hoc shell sessions. Long-term memory via a personal knowledge graph, code awareness via a code-structure index, both connected through MCP. | Python · MCP · Ollama (local + cloud) · systemd |

> 🎬 *[GIF SLOT — drop the "bot replies on Discord / Telegram / SMS / Voice" demo here]*

---

## 🛠 Apps & Tools

| Project | What it does | Stack |
|---|---|---|
| **Jellyfin** | Personal media library with a full automation pipeline. Tracks wanted TV shows and movies, grabs releases from Usenet, sorts them into the right folders, renames them, and refreshes the library automatically. Hardware-accelerated transcoding when available, software fallback otherwise. Friends and family request new content through Jellyseerr (a self-hosted Jellyfin front-end with TMDB-driven search, request approval, and Radarr/Sonarr integration). | Jellyfin · Sonarr (v4) · Radarr (v6) · SABnzbd · Prowlarr · qBittorrent · Jellyseerr · Bazarr · Unpackerr · custom ffmpeg images · Docker · B2 (rclone FUSE) |
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

### 🧠 How it actually thinks

Every service on the box is something Hermes can reach. Send it a Discord message saying *"grab Daredevil: Born Again"* and it:

1. Asks Jellyseerr if it's already requested — if yes, just confirms.
2. Asks Sonarr if it's already in the library (and which seasons).
3. If not, fires a Sonarr search, lets Prowlarr hit the indexers, picks the best release under the [release rules](#-apps--tools).
4. Watches the queue, the storage, the FUSE mount, the webhook back to Jellyseerr — and fixes each silent break in turn.

Hermit doesn't touch the quality profiles or the user's watch history. It owns the boring middle 95% — mounts, scrapes, queue monitoring, library-ID drift, status mismatches.

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
