---
name: rezrov_down
description: rezrov (Google Cloud server, 34.68.90.3) is DOWN as of 2026-04-18. Do not factor it into workflows, recommendations, or sequence design. Does not serve Rutzetta map, roads.html, rutzfull.html, or anything else right now. All agents should treat it as unavailable until R says otherwise.
type: project
originSessionId: 9517c474-68dc-4cfc-8cba-f8f363948c0c
---
# rezrov — DOWN

**Status as of 2026-04-18:** DOWN. Do not reach for it.

R has stated: "rezrov is down for now, don't even factor it in."

**What this means for every agent:**

- Do not propose deploying anything to rezrov
- Do not assume map portal, roads.html, rutzfull.html, Marker OCR pipeline, nginx, Node/Express, pm2, SSH endpoints are reachable
- Do not suggest "we could host this on rezrov"
- Do not include rezrov in sequence infrastructure designs (Watchman/Herald/Searcher/Spotter/Sifter all run on Anthropic cloud, not on R's server — unrelated)
- Do not link to http://34.68.90.3 or any rezrov-backed URL as a live target
- When reasoning about workflows, act as if rezrov did not exist

**Why:** R said so. No further diagnosis requested. Likely tied to the Google Cloud free-credit downgrade timing documented in grip.md, or to unrelated cloud ops — not the point. The point is: **remove rezrov from the active stack in your planning until R restores it.**

**When R brings it back:** update this file's status line to "UP" with the date. Until then, every mention of rezrov should be historical or future-conditional ("when rezrov is back up, …").

**Corollary:** the Grip sequences (Watchman/Herald) designed today run entirely on Anthropic's scheduled-agent cloud and a GitHub repo — zero rezrov dependency. Design is rezrov-agnostic on purpose.
