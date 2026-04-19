# WATCHMAN — daily app delta log

The Watchman appends a dated section here each morning (6:17am GT). Each section reports only **deltas vs. yesterday** — no news is good news.

Format:
```
## YYYY-MM-DD
### Brew updates ready
- pkg a.b.c → x.y.z (changelog summary)

### Security
- CVE-YYYY-xxxxx affecting pkg (installed: v, fixed: v)

### Tier-1 news
- App: what changed (version, feature, breaking change, platform support)

### Cross-ref from past.md
- Dropped-app news (if any)
```

---

_First run pending trigger activation._
