# qodex-grip

Shared context + log repo for **the Grip's sequences**. Mirror pattern of `qodex-pi`.

## Sequences

| Name | Cadence | Writes to | Scope |
|---|---|---|---|
| **the Watchman** | Daily 6:17am GT (12:17 UTC) | `WATCHMAN.md` | brew/mas outdated + Tier-1 CVE/version deltas |
| **the Herald** | Sundays 7:00am GT (13:00 UTC) | `HERALD.md` | Company news, EOL, M1 migration, watchlist maturity, workflow changes |

## Context

- `memory/installed_raifive.md` — current raiFive install snapshot (synced from local)
- `memory/installed_iphone.md` — manual fonGusta app list
- `memory/grip_past.md` — uninstalled apps + reasons (no zombie revivals)
- `memory/grip_future.md` — watchlist
- `memory/grip.md` — machines, storage, M1 migration checklist
- `memory/grip-sequences.md` — sequence design + Tier-1 list
- `memory/rezrov_down.md` — rezrov is DOWN, agents treat as unavailable

## Output consumers

- **Daily Planet** (`~/codects/streams/planet/planet.html`) — reads `WATCHMAN.md` + `HERALD.md` and renders an Apps desk section
- R directly — `cat WATCHMAN.md | tail` or `cat HERALD.md | tail`

## Design ref

Full design in `memory/grip-sequences.md`. Tier-1 priority list (~30 apps deep-checked daily) lives there.
