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

## 🛰️ Live Service Map (today)

> Every container that actually answers the door. Grouped by what they do, not where their repo lives.

### 🎬 Media — P.I.R.A.T.E.

| Container | What it does | Stack |
|---|---|---|
| **Sonarr** | TV show brain. Tracks wanted episodes, picks best release, hands off to downloaders. Per-episode first, season packs only as fallback. | Sonarr v4 · Docker |
| **Radarr** | Movie twin of Sonarr. Same automation, no season logic. | Radarr v6 · Docker |
| **Prowlarr** | Single pane of glass over every indexer. One config change syncs to Sonarr + Radarr. | Prowlarr · Docker |
| **SABnzbd** | Usenet client. Does the PAR2/RAR work and hands clean files to Sonarr/Radarr. | SABnzbd · Docker |
| **qBittorrent** | Torrent client. Sidecar for grabs SABnzbd won't find. | qBittorrent · Docker |
| **Jellyfin** | The playback server. Streams straight to phone, browser, TV. Hardware-accel transcoding when available. | Jellyfin · custom ffmpeg · Docker |
| **Jellyseerr** | Friend-facing request UI. TMDB-backed search + approval flow + auto-webhook back to "Available". | Jellyseerr · Docker |
| **Tdarr** | Distributed transcode farm. One server + one worker, ready when a heavy batch comes through. | Tdarr · Docker |

Everything media-side talks to the **Wasabi hot bucket** (jellyv2kxvn) via `rclone FUSE` mounted at `/home/ai/mnt/kxvn-b2`. Local disk only sees boot files and `~/.cache/rclone`.

### 🧰 Productivity & Web

| Container | Domain | What it does |
|---|---|---|
| **Zipline (CDN)** | `<cdn>` | Self-hosted file host with drag-and-drop upload and shareable links. Restyled, no upstream branding. |
| **Stirling-PDF** | `<pdf>` | 50+ PDF tools — merge, split, OCR, redact, sign. Login-gated, fully rebranded. |
| **KXVN Trades** | `<trading>` | Paper-mode LLM trading research. Multi-agent debate returns a plan, never executes. |
| **kxvn.io landing** | `kxvn.io` | Splash page — audio gate, Lanyard Discord presence, Spotify now-playing, mute controls. |
| **docs / graphs / master-control** | various | Internal dashboards and admin tools. Tailscale-only. |

### 👀 Operations & Observability

| Container | What it does |
|---|---|
| **Uptime Kuma** | 24/7 health monitor across every public service. Sends pings to Discord on incident. |
| **Portainer** | Docker management UI. Backup plan when Hermes is asleep. |
| **Glances** | System-wide host metrics: CPU, RAM, disk, network, container breakdown. |
| **ntfy** | Self-hosted push notifications. The webhook target every cron'd job, watcher, and watchdog uses. |

### 📡 Fun & Quick Tools

| Container | What it does |
|---|---|
| **Pairdrop** | Local-only, zero-trust AirDrop substitute. Drag a file across browsers on the same LAN. |
| **Filebrowser** | Web UI for the working directory on the box. Quick drag, download, upload. |
| **Glance** | Personal home-dashboard. RSS, weather, GitHub, weather, one panel at a time. |
| **Spotify Now Bridge** | Tells the splash page what you're listening to. |
| **Rustdesk (hbbs + hbbr)** | Self-hosted remote desktop. Your own TeamViewer with no third party in the loop. |
| **LiveKit** | Voice/video server used by Voice-channel sessions in Hermes. |

### 🏠 Physical Layer

| Device | What it does |
|---|---|
| **Home Assistant** | Smart-home hub. Lights, switches, scenes, the PC power plug, all behind one fabric. |
| **PC (windows box)** | Upload station — runs SAB/Stirling/Rustdesk clients, ships bandwidth in and out of the box. |

### 🗺️ How it all threads together

```
                          You (Discord · Telegram · SMS · Voice · Web)
                                       │
                                       ▼
                            ┌─────── Hermit ────────┐
                            │       (agent)        │
                            │   - watches queues   │
                            │   - fixes FUSE bind  │
                            │   - alerts via ntfy  │
                            └────────────┬─────────┘
                                         │
   ┌──────────── P.I.R.A.T.E. ────────────┼──────────── Productivity ────────────┐
   │                                     ▼                                        │
Sonarr/Radarr ──► Prowlarr ──► SABnzbd / qBittorrent ──► /downloads/ → Sonarr/Radarr ──► import
   │                                                                      │
   ▼                                                                      ▼
Jellyfin  ◄──── webhook ◄──── Jellyseerr                            Tdarr (transcode)
   │
   ▼  streams
You watching the show
```

Hermit sits on the same wire as every one of these. A "grab Daredevil" message becomes: **Jellyseerr check → Sonarr search → queue watch → mount bounce → NFO fixup → status reap** — all without you touching the keyboard again.

---

## 📦 Repos

| Repo | Visibility | Notes |
|---|---|---|
| `kishyxd/projects` | Public | This showcase |
| `kishyxd/services` | Private | Glance, plausible, filebrowser, ntfy, pairdrop, etc. |
| `kishyxd/homeserver` | Private | Infra templates + runbooks |
| `kishyxd/pdf` | Private | PDF editor project |
| `kishyxd/kxvn-site` | Private | The kxvn.io landing page source |

🔒 *Source code for every project is private. This repo is the public showcase.*

See them all live at **[kxvn.io](https://kxvn.io)**.
