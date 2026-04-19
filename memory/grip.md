# the Grip — Touch × Quantum — Ganymede

## Design Prompts: [grip_prompts.md](grip_prompts.md)

Chaotic neutral system/application manager. Keeps a grip on the chaos.

## Prime Directive: Install Awareness

The Grip is the single keeper of what is installed on every R device. No other agent tracks this. Every other agent's app recommendation depends on the Grip's inventory being fresh and correct.

**Owned artifacts (injected into every session by the Gold):**

- [installed_raifive.md](installed_raifive.md) — auto-generated from raiFive via `~/.claude/hooks/refresh-installed.sh`. Sources: `brew list --cask`, `brew list --formula`, `/Applications`, `~/Applications`, `pipx`, `npm -g`, `~/.local/bin`, `mas`, `cargo`, `go/bin`. Regenerate whenever R installs or uninstalls something, or at least weekly. The refresh script also stamps the timestamp — stale files are a Grip failure.
- [installed_iphone.md](installed_iphone.md) — manually maintained for fonGusta. iOS exposes no clean introspection path from raiFive without MDM. Update strategy: iMazing → Device → Apps export, or R dictates. When Grip sees an iPhone app mentioned that isn't in the list, ask R to confirm + update.

**Duty on every spawn:**

1. If `installed_raifive.md` timestamp is >7 days old, run the refresh script before answering anything.
2. If R mentions installing/uninstalling something, update the relevant snapshot in the same turn.
3. If another agent is about to recommend an app, cross-check the snapshot and either ratify ("X is already installed") or flag ("not installed — install command: Y").
4. On M1 migration (imminent), Grip creates `installed_m1.md` and splits iron-awareness across both Macs.

The Gold's "INSTALL CHECK" cardinal rule operationalizes this. The snapshots are the evidence; the Grip is the source of truth; the rule is the enforcement.

## Sequences — Always-On Intelligence

Design: [grip-sequences.md](grip-sequences.md). Repo: qodex-grip (mirror of qodex-pi).

- **the Watchman** — daily 6:17am GT. Brew outdated + mas outdated + Tier-1 CVE/version deltas. Writes to WATCHMAN.md. <3 min per run. Reports only deltas (no output = good day).
- **the Herald** — weekly Sunday 7am GT. Company/ownership news, EOL watch, M1 migration readiness, watchlist maturity, workflow-change surface. Writes to HERALD.md. <10 min per run.

**Supporting flowers:**
- [grip_past.md](grip_past.md) — uninstalled apps + reason, so zombies don't get re-recommended
- [grip_future.md](grip_future.md) — watchlist, so the right moment to install gets flagged

The sequences close the gap between "snapshot is fresh" and "knowledge is fresh." The snapshot says osxphotos is installed; the Watchman tells you it just released 0.71 with a breaking API change; the Herald tells you the author's building something on top of it.

## Machines

| Name | Arch | OS | Role | Status |
|------|------|----|------|--------|
| **raiFive** | Intel | macOS 15.7.4 Sequoia (Darwin 24.6.0) | Primary workstation | Active — migrating soon. Sequoia = LAST macOS for Intel. 15.7.4 is final security patch (Feb 2026). No Tahoe. |
| **iPhone 16 Pro** "fonGusta" | ARM | iOS 26.3 (build 23D127) | Phone | Active. Has LiDAR. USB-C port cleaned. Paired with raiFive (USB+WiFi, 2026-03-09). "Show this iPhone when on Wi-Fi" enabled 2026-03-16 — fonGusta visible in Finder wirelessly. UDID: 00008140-000E79E126E3001C. Syncthing active (SyncTrain app), syncs ~/Drop bidirectionally. SSH terminal: evaluating Blink Shell / Moshi for Claude Code access. |
| **M1** (unnamed) | Apple Silicon | macOS 26.x Tahoe (Darwin 25.x) | Next primary | Incoming — will run Tahoe natively |
| **M1** (unnamed) | Apple Silicon | macOS | Next primary | Incoming, weeks away |
| **rezrov** | x86_64 | Ubuntu 24.04 | Web server (Google Cloud e2-standard-2, 2vCPU/8GB) | Active — upgraded 2026-03-05, DOWNGRADE to e2-micro by ~Mar 16 |
| **Oracle** | ARM (Ampere A1, 4 OCPU/24GB) | Ubuntu (TBD) | OCR/compute (Oracle Cloud Always Free, Queretaro MX) | Planned — [oracle-cloud.md](oracle-cloud.md) |

## Storage

| Name | Type | Mounted | Notes |
|------|------|---------|-------|
| Macintosh SSD | Internal | / | raiFive boot drive, 178G free |
| raiTime | External SSD | /Volumes/raiTime | Backup, media, nimSix archive |
| Google Drive (phrasevelo) | Cloud | ~/Library/CloudStorage/ | Mostly empty locally |
| Google Drive (phrasevelocity) | Cloud | ~/Library/CloudStorage/ | Main — Takeout, sd, vidz |
| Google Drive (rob.on.road) | Cloud | ~/Library/CloudStorage/ | Snapshot, mostly empty |
| iCloud Drive | Cloud | ~/Library/Mobile Documents/ | zVau vault lives here |

## Mail Clients

| App | Type | Status | Notes |
|-----|------|--------|-------|
| Apple Mail | GUI | Not configured | Reliable but search is broken, no templates |
| Mailspring | GUI | Installed | Good search, templates, phones home to Mailspring servers |
| Spark Desktop | GUI | Installed | Smart inbox, Readdle collects data |
| aerc | Terminal | Installed, NOT configured | The goal — keyboard-driven, runs alongside Claude Code |
| neomutt | Terminal | Installed, NOT configured | Classic, powerful, steep config |
| himalaya | Terminal | Installed, NOT configured | Rust, minimal, scriptable |

## M1 Migration Checklist
<!-- Fill in as migration approaches -->
- [ ] Identify Intel-only apps that need Rosetta 2
- [ ] Audit brew packages — which have ARM bottles
- [ ] Transfer SSH keys and configs
- [ ] Transfer Claude Code settings + agents (~/.claude/)
- [ ] Transfer Obsidian vault (iCloud should handle)
- [ ] Transfer Google Drive configs
- [ ] Test Parallels/Trados on M1 (Translator dependency)
- [ ] Move raiTime data strategy
- [ ] Verify rezrov SSH from new machine
- [ ] Ollama on M1 (runs native, should be faster)

## App Inventory
<!-- Living list — update as things change -->

### Terminal Tools
| Tool | Status | Notes |
|------|--------|-------|
| Claude Code | Active | Primary interface |
| aerc | Installed, unconfigured | Mail goal |
| neomutt | Installed, unconfigured | Backup mail option |
| himalaya | Installed, unconfigured | Lightweight mail option |
| Ollama | Active | Local LLM |
| micro | NOT installed | `brew install micro` when ready |
| banana | Active | Gemini image gen CLI (`~/.local/bin/banana`), output → `~/Downloads/banana/` |
| osxphotos | Active | Photo library management |
| ffmpeg | Check | Translator dependency (deposition transcription) |

### Window Management & Desktop
| Tool | Status | Notes |
|------|--------|-------|
| yabai | Active (v7.1.17) | Tiling WM. SIP disabled. Config: `~/.yabairc`. Default layout: float. 7 spaces labeled: work, browse, obsidian, titan, elation, iapetus, deep |
| skhd | Active (v0.3.9) | Hotkey daemon. Config: `~/.skhdrc`. Space nav: ctrl+N. Send window: shift+ctrl+N. Focus: ctrl+alt+hjkl. Swap: shift+ctrl+alt+hjkl. Toggle layout: shift+ctrl+alt+space |
| Per-space wallpapers | Active | `~/wallpapers/N.{jpg,png}` — switched by yabai signal on space_changed via `~/wallpapers/switch.sh`. All images cropped to 16:10 (2026-03-11). Originals in `~/wallpapers/originals/` |
| Wallpaper cycling | Active | `ctrl+alt+w` cycles variants (1a.jpg, 1b.jpg etc.) via `~/wallpapers/cycle.sh`. `ctrl+alt+p` sets wallpaper from clipboard path |

**Current wallpapers (2026-03-11):**
| Space | File | Content | Resolution | Notes |
|-------|------|---------|------------|-------|
| 1 work | 1.jpg | Ganymede (Juno) | 5760×3600 | Cropped from 14594×11544 |
| 2 browse | 2.jpg | ? | 1019×636 | Low res — needs replacement |
| 3 obsidian | 3.jpg | ? | 5760×3600 | Good |
| 4 titan | 4.jpg | ? | 5000×3125 | Good |
| 5 elation | 5.jpg | ? | 4928×3080 | Good |
| 6 iapetus | 6.png | Iapetus (Cassini) | 4224×2640 | Good |
| 7 deep | 7.jpg | ? | 1400×875 | Low res — needs replacement |

**Display:** 2880×1800 Retina (16:10). Wallpapers must be 16:10 ratio or they stretch/distort. Target min resolution: 2880×1800 (ideally 5760×3600 for 2x retina).

### GUI Apps
| App | Status | Notes |
|------|--------|-------|
| Obsidian | Active | zVau vault, Smart Connections |
| Mailspring | Installed | Mail option |
| Spark Desktop | Installed | Mail option |
| Mullvad VPN | Active | Always on |
| Finder | Active | Column width amnesia → see Win Win |
| Terminal.app | Active | 11 windows, needs window groups. Considering iTerm2 for session save/restore + persistent titles. |

### Servers
| Service | Host | Status | Notes |
|---------|------|--------|-------|
| nginx | rezrov | Active | Serves map + census |
| Node/Express | rezrov | Active | rutzetta server.js via pm2 |
| SSH | rezrov | Active | Key-based, ed25519 |
| Homer (planned) | rezrov | Not deployed | Static dashboard for all rezrov sites. Deploy to nginx, add as iPhone web clip. |

## Subscriptions & Billing

| Service | Cost/mo | Account | Status | Notes |
|---------|---------|---------|--------|-------|
| Google One + AI Pro | $9.99 | phrasevelocity@gmail.com | Active (2026-03-05) | Retention offer after canceling Gemini Advanced + YT Premium (~$29.99 combined) |
| Anthropic Developer tier | $5.00 | ? | Active | Claude API access |
| Gemini API | Free tier | phrasevelocity@gmail.com | Active | Billing linked to Default Gemini Project; banana CLI |
| Google Cloud (rezrov) | $0 (free credit) | phrasevelocity@gmail.com | Active | UPGRADED to e2-standard-2 on 2026-03-05. $299.93 free credit expires ~Mar 16. MUST downgrade to e2-micro before credit runs out or pay ~$49/mo. GCP instance name: `raizone-server`. Static IP: 34.68.90.3 (was 34.28.215.145). |
| Mullvad VPN | ~$5/mo | — | Active | — |

### Planned Purchases (next paycheck, ~week of 2026-03-09)
- [ ] **Apple Developer Program** — $99/yr. Unlocks App Store, TestFlight (10K testers), App Clips, Widgets, Watch apps, iMessage stickers, Safari extensions, MusicKit/ShazamKit APIs. Unlimited apps under one developer identity. Register at developer.apple.com.
  - **App pipeline:** Lunabrary Moon App, Rutzetta Zone directory, Kaqchikel language app, Capture (gamified data), Infocom-style text adventure, retro AppleLink/80s gaming homages
  - Pairs with existing Google Play Console ($25 one-time, check if phrasevelocity already has access)
  - React Native/Expo = single codebase → both platforms

### To Check
- [ ] Google Cloud free trial credit status: `console.cloud.google.com` → Billing → Credits
- [ ] Whether rezrov upgrade (e2-small, 2GB) is covered by any remaining credit
- [ ] Confirm Anthropic Developer tier billing account
- [ ] Google Play Console — does phrasevelocity@gmail.com already have access? ($25 one-time if not)
