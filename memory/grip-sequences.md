---
name: grip-sequences
description: Grip sequences — the Watchman (daily updates/security) and the Herald (weekly news/EOL/migration). Autonomous scheduled agents that continuously surveil all installed, past, and watchlisted apps across raiFive + fonGusta. Pattern mirrors pi-sequences (Searcher/Tender/Spotter/Sifter).
type: project
originSessionId: 9517c474-68dc-4cfc-8cba-f8f363948c0c
---
# Grip Sequences — Always-On App Intelligence

**Created:** 2026-04-18
**Repo:** https://github.com/phrasevelocity-alt/qodex-grip (to be created)
**Manage triggers:** https://claude.ai/code/scheduled
**Owner:** the Grip

## Why

The Grip's Prime Directive (install awareness) is only as good as its freshness. Snapshots tell you what's installed — they don't tell you when BetterTouchTool ships a breaking 5.0, when Readdle sells Spark, when Parallels drops Intel support, when a cask has a CVE. The sequences close that gap. They run whether or not R is in a session, write to a growing log, and the overnight harvest is there at breakfast.

## Scope — three app pools

1. **Current** — what's installed now. Source: `installed_raifive.md` + `installed_iphone.md`. Dozens of apps; too many to deep-scan daily → tiered approach.
2. **Past** — apps R has uninstalled. Source: `grip_past.md`. Track **why** they were dropped so the Watchman doesn't resurrect zombie recommendations.
3. **Future** — apps on the watchlist. Source: `grip_future.md`. Apps R is curious about but hasn't installed. The Herald monitors their maturity, price changes, new features, so a decision can be made when the moment comes.

## Sequence 1 — the Watchman (daily, fast, actionable)

- **Schedule:** Daily 6:17am GT (12:17 UTC) — staggered off Searcher/Spotter/Sifter
- **Model:** claude-sonnet-4-6
- **Duration target:** <3 min per run
- **Inputs:** installed_raifive.md, installed_iphone.md, grip_past.md
- **Tasks (in order, fail-soft):**
  1. Run `brew outdated --cask` + `brew outdated` — report deltas vs yesterday. Authoritative, cheap.
  2. Scan Mac App Store update queue (if `mas` installed, `mas outdated`).
  3. Tier-1 web check (top 30 daily-use apps — hand-curated list below): current stable version, release date, changelog summary, any breaking-change flags, any CVE/advisory in last 24h.
  4. Cross-reference past.md — if a past-app is mentioned in news (acquisition, rename, fork), surface it with the "uninstalled because X" context.
- **Output:** appends to `WATCHMAN.md` as dated section. Format:
  ```
  ## 2026-04-18
  ### Brew updates ready
  - bettertouchtool 4.932 → 5.001 (changelog: …)
  ### Security
  - CVE-2026-xxxx in openssl@3 (installed: 3.2.1, fixed: 3.2.3)
  ### Tier-1 news
  - Parallels Desktop 20.2 released — Intel macOS 15 still supported, 21 will be ARM-only
  ```
- **No output = good day.** The Watchman only reports deltas, not baseline state.

## Sequence 2 — the Herald (weekly, deeper, strategic)

- **Schedule:** Sundays 7:00am GT (13:00 UTC)
- **Model:** claude-sonnet-4-6
- **Duration target:** <10 min per run
- **Inputs:** installed_raifive.md, installed_iphone.md, grip_past.md, grip_future.md, grip.md (M1 migration checklist)
- **Tasks:**
  1. Company/ownership status for Tier-1 + Tier-2 apps — acquisitions, shutdowns, leadership changes, pricing model shifts.
  2. macOS Sequoia (Intel final) compatibility tracking — which apps' next major release drops Intel.
  3. M1 migration readiness — for each brew cask/formula, ARM bottle status; for each GUI, Apple Silicon native vs Rosetta; for each CLI, does it build on ARM.
  4. EOL watch — apps with no commits in 12+ months, dead GitHub repos, abandoned sites.
  5. Watchlist maturity — for each app in `grip_future.md`, is the moment right? (feature added, price dropped, competitor acquired).
  6. Workflow-change surface — major UI overhauls, keyboard shortcut changes, default-behavior shifts.
- **Output:** appends to `HERALD.md` with sectioned weekly report.

## Tier-1 daily-check list (hand-maintained, ~30 apps)

Edited here as priorities shift. These get the Watchman's full attention daily.

**Dev/terminal:** Claude Code, iTerm2, Obsidian, Cursor, Ollama, yabai, skhd
**Creative:** Affinity (2025 unified), Canva (web), DiffusionBee, Blender, Godot
**Writing/research:** Sublime Text, BBEdit, Perplexity, DeepL
**Comms:** Telegram, WhatsApp, Signal, Messenger, Spark, Mailspring
**System:** Parallels Desktop, Mullvad VPN, Syncthing, Raycast, BetterTouchTool, Tailscale
**Security-sensitive:** KeePassXC, Strongbox, LuLu
**Translator-critical:** Parallels (Trados), OmegaT
**Phone-critical:** Halide, Scaniverse, Suno, iMazing

## Repo structure (qodex-grip, to be created)

```
qodex-grip/
  WATCHMAN.md          ← daily deltas (append-only)
  HERALD.md            ← weekly reports (append-only)
  memory/
    installed_raifive.md      ← synced from local
    installed_iphone.md       ← synced from local
    grip_past.md              ← uninstalled apps + reason
    grip_future.md            ← watchlist
    grip.md                   ← full Grip context (machines, storage, M1 checklist)
  tier1.md             ← priority list (synced from this flower)
```

## Sync protocol

When local snapshots refresh, push to repo so remote agents see current state:
```bash
SRC=~/.claude/projects/-Users-raizone/memory
cd /tmp/qodex-grip && git pull
cp "$SRC"/installed_raifive.md memory/
cp "$SRC"/installed_iphone.md memory/
cp "$SRC"/grip_past.md memory/
cp "$SRC"/grip_future.md memory/
cp "$SRC"/grip.md memory/
git add -A && git commit -m "sync $(date +%F)" && git push
```

## Reading the harvest

Morning ritual — pull and read:
```bash
cd /tmp/qodex-grip && git pull
cat WATCHMAN.md | tail -60       # last few days of deltas
cat HERALD.md | tail -200        # last Sunday's report
```

## Status

- [x] qodex-grip repo created — https://github.com/phrasevelocity-alt/qodex-grip
- [x] grip_past.md seeded (empty frame)
- [x] grip_future.md seeded (empty frame)
- [x] Watchman trigger created — **trig_013e76DPUbcmti2A9gUnPToq** — first run 2026-04-19 12:17 UTC
- [x] Herald trigger created — **trig_01Xw6ttvh6dCN6gCeGxuVjCw** — first run 2026-04-19 13:00 UTC (tomorrow, Sunday)
- [x] First snapshots pushed to repo
- [x] Daily Planet wired (~/codects/streams/planet/generate.sh Apps Desk section)
- [ ] First run verified

## Trigger manifest

| Sequence | Trigger ID | First run | Cron (UTC) | Local (GT) |
|---|---|---|---|---|
| Watchman | `trig_013e76DPUbcmti2A9gUnPToq` | 2026-04-19 12:17 UTC | `17 12 * * *` | daily 6:17am |
| Herald | `trig_01Xw6ttvh6dCN6gCeGxuVjCw` | 2026-04-19 13:00 UTC | `0 13 * * 0` | Sundays 7:00am |

Manage at https://claude.ai/code/scheduled. Run-now or edit via the RemoteTrigger API.
