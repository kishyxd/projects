# 🛳 KXVN Projects

> **One VPS. Nine services. One agent that runs the show.**

🌐 **[kxvn.io](https://kxvn.io)** — front door · 📚 **[docs.kxvn.io](https://docs.kxvn.io)** — guides · 📖 **[comics.kxvn.io](https://comics.kxvn.io)** — comics & manga · 🤖 **Hermit** — the agent behind it all

An always-on, self-hosted stack running on a single Linux VPS behind Nginx and Tailscale. Hermit keeps the services connected, watches the boring stuff, and fixes what it can before it becomes a problem.

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

> Every part of the stack carries an over-engineered enterprise acronym, because a homelab with a codename feels 10% more legitimate. 😏

| System | Acronym | Stands For | What It Actually Is |
|---|---|---|---|
| **KXVN** | `K.X.V.N.` | **K**ishy's e**X**tended **V**irtual **N**etwork | The whole stack — every domain, container, and service hanging off one point. |
| **HERMIT** | `H.E.R.M.I.T.` | **H**omelab **E**xecutive **R**esident — **M**ulti-platform **I**ntelligent **T**eammate | The AI agent & assistant — one core across every gateway, watching queues, running tasks, fixing what breaks. |
| **VAULT** | `V.A.U.L.T.` | **V**ideo **A**cquisition & **U**nified **L**ibrary **T**ransport | The media stack — Sonarr, Radarr, SABnzbd, Jellyfin, Prowlarr, qBittorrent, Jellyseerr. It fetches, it stores, it serves. |
| **SHELF** | `S.H.E.L.F.` | **S**elf-hosted **H**osting for **E**ntertainment, **L**iterature & **F**iles | The comics stack — Kavita, the signup bridge, and the download pipeline. It is literally a shelf. |

*Composed by an insufferably truthful machine.* :3

---

## 🌟 Featured

| Project | Description | Stack |
|---|---|---|
| [**kxvn.io**](https://kxvn.io) | Click-to-enter splash page with audio, live Discord presence via Lanyard, Spotify now-playing, avatar decorations, mute controls, and Minecraft-style typography. The public front door for the whole stack. | Vanilla HTML/CSS/JS · Lanyard API · Nginx |
| **Hermes** | AI agent framework that runs as one core across CLI, a multi-platform gateway (Discord, Telegram, WhatsApp, SMS, web dashboard), and ad-hoc shell sessions. Long-term memory via a personal knowledge graph, code awareness via a code-structure index, both connected through MCP. | Python · MCP · Ollama (local + cloud) · systemd |

## 🛰️ Live Service Map (today)

> Every container that actually answers the door. Grouped by what they do, not where their repo lives.

### 🎬 Media — V.A.U.L.T.

| Container | What it does | Stack |
|---|---|---|
| **Sonarr** | TV show brain. Tracks wanted episodes, picks best release, hands off to downloaders. Per-episode first, season packs only as fallback. | Sonarr v4 · Docker |
| **Radarr** | Movie twin of Sonarr. Same automation, no season logic. | Radarr v6 · Docker |
| **Prowlarr** | Single pane of glass over every indexer. One config change syncs to Sonarr + Radarr. | Prowlarr · Docker |
| **SABnzbd** | Usenet client. Does the PAR2/RAR work and hands clean files to Sonarr/Radarr. | SABnzbd · Docker |
| **qBittorrent** | Torrent client. Sidecar for grabs SABnzbd won't find. | qBittorrent · Docker |
| **Jellyfin** | Synchronized companion playback server. Provides backup web access and the proper native clients for phones, TVs, computers, and media centers. | Jellyfin · Docker |
| **Jellyseerr** | Friend-facing request UI. TMDB-backed search + approval flow + auto-webhook back to "Available". | Jellyseerr · v3.4.1 · Docker |
| **Tdarr** | Distributed transcode farm. One server + one worker, ready when a heavy batch comes through. | Tdarr · Docker |

Everything media-side talks to the **Wasabi hot bucket** (jellyv2kxvn) via `rclone FUSE` mounted at `/home/ai/mnt/kxvn-b2`. Local disk only sees boot files and `~/.cache/rclone`. The whole media pipeline is **V.A.U.L.T.** — Video Acquisition & Unified Library Transport.

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
   ┌──────────── V.A.U.L.T. ────────────────┼──────────── Productivity ────────────┐
   │                                     ▼                                        │
Sonarr/Radarr ──► Prowlarr ──► SABnzbd / qBittorrent ──► /downloads/ → Sonarr/Radarr ──► import
   │                                                                      │
   ▼                                                                      ▼
Jellyfin ◄──── webhook ◄──── Jellyseerr                            Tdarr (transcode)
   │
   ▼  streams
You watching the show
```

Hermit sits on the same wire as every one of these. A "grab Daredevil" message becomes: **Jellyseerr check → Sonarr search → queue watch → mount bounce → NFO fixup → status reap** — all without you touching the keyboard again. FTV Media is the user-facing media service; Silo and Jellyfin share accounts, passwords, libraries, and watch history.


## 🌐 Public entry points

The public-facing links:

| Public link | What it does |
|---|---|
| [kxvn.io](https://kxvn.io) | Public landing page and front door. |
| [ftv.kxvn.io](https://ftv.kxvn.io) | FTV — the main media player. |
| [kxvn.io/quicklinks](https://kxvn.io/quicklinks/) | **FTV Quick Stream** — public direct-watch links, tap any to start watching. |
| [kxvn.io/cquicklinks](https://kxvn.io/cquicklinks/) | FTV Quick Stream (copy mode) — tap to copy a link to your clipboard. |
| [comics.kxvn.io](https://comics.kxvn.io) | Kavita — comics & manga reader ([signup](https://comics.kxvn.io/signup) for an account). |
| [docs.kxvn.io](https://docs.kxvn.io) | Public documentation hub, including the FTV Media guide. |

Admin panels, service endpoints, and implementation details are kept out of this public README. Use the documentation hub for current user guidance.

## Recent changes

- **2026-09-30** — **KXVN Comics** launched (`comics.kxvn.io`): self-hosted Kavita comics + manga reader on the laptop with 9 series live (Invincible Compendium ×3, Jujutsu Kaisen ×31 volumes, Wolverine Old Man Logan, Spidey TPBs, Batman 1940, Akira, Sonic). Self-serve signup page at `/signup` (email → on-page activation link, no SMTP needed, Discord webhook pings on each signup), OPDS feed for iOS Panels, Mylar3 for comic automation, and an agent-driven acquisition pipeline — send Hermit a getcomics.org link or a manga title in any chat and it downloads, shelves, and scans automatically.
- **2026-09-18** — **FTV Quick Stream** launched: one-tap direct-watch pages for movies and episodes (`kxvn.io/<movie>/`, `kxvn.io/<series>/<S>/<E>/`), auto-generated from TMDB. Public listing at `kxvn.io/quicklinks` (+ clipboard-copy variant at `kxvn.io/cquicklinks`), both rebranded "FTV Quick Stream", collapsible per-season tabs, in-app-browser warning. Admin panel (behind auth) adds: hide/show toggles with optimistic UI, TMDB search with episode picker (add movies/shows by title or TMDB/IMDb id), TMDB info links to verify titles, and permanent delete mode (trash icon → multi-select → confirm). Bot's `/kxvn-list` merged into `/kxvn` with an autocomplete `type` option.
- **2026-08-13** — Discord bot got a `/media` and `/stat` dashboard (movies · series · episodes + size on disk + last 3 added). Auto-delete-after-30s on every command except `/media` and `/stat` so the bot doesn't litter channels.
- **2026-08-13** — `/collections` now shows live "X/Y in library" counts in search results (was just listing total movies).
- **2026-08-15** — FTV Media account documentation synchronized across Silo, Jellyfin, Jellyseerr, Discord, and this showcase. Jellyseerr uses Jellyfin authentication; Silo and Jellyfin password changes mirror in both directions, and watch history stays synchronized.
- **2026-08-15** — Public-link cleanup: this README now exposes only `kxvn.io` and `docs.kxvn.io`. Service-specific links belong in the user documentation, not the public project showcase.
- **2026-08-12** — Unified notifications: rolled-up/completion semantics over per-event spam.
- **2026-08-12** — Sonarr/Radarr minimumAge set to 500 days (skip ancient encodes).
- **2026-08-12** — Radarr Anime profile created (mirrors Sonarr anime structure).
- **2026-08-12** — Jellyseerr updated to v3.4.1.


---

## 📦 Repos

| Repo | Visibility | Notes |
|---|---|---|
| `kishyxd/projects` | Public | This showcase |
| `kishyxd/services` | Private | Glance, plausible, filebrowser, ntfy, pairdrop, etc. |
| `kishyxd/homeserver` | Private | Infra templates + runbooks |
| `kishyxd/pdf` | Private | PDF editor project |
| `kishyxd/kxvn-site` | Private | The kxvn.io landing page source |
| `kishyxd/discordbot` | Private | The KXVN Discord bot + slash commands (12 visible, 24 hidden) |
| `kishyxd/iptv-live` | Private | IPTV playlist proxy + search |
| `kishyxd/collectr-price` | Private | Pokémon TCG price tracker |
| `kishyxd/mediav2` | Private | Media discovery PWA |

🔒 *Source code for every project is private. This repo is the public showcase.*

See the public project hub at **[kxvn.io](https://kxvn.io)** or read the current guides at **[docs.kxvn.io](https://docs.kxvn.io)**.
