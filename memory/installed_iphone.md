---
name: installed_iphone
description: Manually maintained list of apps installed on fonGusta (iPhone 16 Pro, iOS 26.x). Grip is the keeper; R updates when apps are added/removed. No auto-introspection path from raiFive to iPhone app list exists (Apple sandboxing). Gold hook injects this on every SessionStart.
type: reference
originSessionId: 9517c474-68dc-4cfc-8cba-f8f363948c0c
---
# Installed on fonGusta — manual list

_Last updated: 2026-04-18_

Grip maintains this file. R updates it when installing/removing apps. Auto-refresh is not possible from raiFive — iOS does not expose the installed-apps list over USB/WiFi sync without an MDM profile or jailbreak. Options for future automation: iMazing's "Apps" tab can export a list; Apple Configurator; Shortcuts "Get Device Details" (limited). For now, manual is cleaner than wrong.

## Apps

<!-- Fill in — one per line or grouped. Format is intentionally loose so R can dump quickly. -->

### Photo / camera
- Photos (stock)
- Camera (stock)
- Halide
- Scaniverse
- Immich (client)

### Maps / geo
- Maps (stock)
- Google Maps
- Organic Maps
- Gaia GPS
- Go Map!!
- OsmAnd

### Mail / messaging
- Mail (stock)
- WhatsApp
- Telegram
- Signal
- Messenger
- Spark

### Terminal / dev
- Blink Shell (evaluating)
- Termius
- Working Copy
- a-Shell
- Pythonista
- Scriptable

### Audio / music
- Music (stock)
- Suno
- Dolby On
- VoiceRecord

### Productivity
- Obsidian
- Notion
- Drafts
- Shortcuts (stock)

### Utilities
- Syncthing (SyncTrain)
- iMazing Profile Editor
- Mullvad VPN
- ProtonMail/ProtonVPN

### Travel / local
- Google Translate
- DeepL
- Gems (language)

<!-- PLACEHOLDER LIST — R to verify and expand. This file supersedes assumptions; if an app isn't here, assume NOT installed until confirmed. -->

## How to refresh

- **iMazing** → Device → Apps → Export list (CSV or clipboard)
- **Shortcuts** → build a "Snapshot apps" action (limited — only shows user-visible apps)
- **Manual** — open iPhone, scroll, dictate. R's call.

## Rules

- When recommending any iPhone app, check this list first.
- If not listed, phrase as "worth trying" not "you should get" — acknowledge the list may be incomplete.
- If an app is confirmed installed but missing here, R adds it on the spot.
