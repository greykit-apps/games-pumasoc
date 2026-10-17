# PumaSOC

**Seven game days on the SOC floor.** A retro cyberpunk Security Operations Center management game in the browser. Defend a client estate against nation-state and criminal threat actors — APT28, Lazarus Group, Volt Typhoon, LockBit and others — each with its own skill rating, escalating through campaign phases.

**Play:** [greykit.com/games/pumasoc](https://greykit.com/games/pumasoc/)

![PumaSOC mid-game — incidents, SIEM feed, and operations panel](preview.png)

## How it plays

Threats probe the estate continuously. Detection Coverage decides whether each one is caught on arrival; a detected threat becomes an incident, and an assigned analyst works it through **TRIAGE → CONTAIN → ERADICATE → RECOVER**. Undetected threats dwell and breach unless found, and incidents left unassigned breach too. Budget arrives each cycle and buys analysts, training, threat hunts, detection rules, vulnerability scans and patches, SOAR playbooks, intel feeds, and partners: red teams, awareness training, an MSSP or a consultant. Every few game days a crisis lands — a DDoS, a tool outage, a zero-day, an insider, or a budget audit.

Survive seven game days with fewer than three breaches. Three difficulties, Analyst, Engineer and Hunter, set the starting budget, staff and detection, and how fast threats breach.

![Difficulty selection — Analyst, Engineer, and Hunter profiles](screenshots/difficulty-select.png)

## Keys

| Key | Action |
|---|---|
| `Space` | Pause / resume |
| `1`–`4` | Game speed (1×–4×) |
| `?` | How to play |
| `M` | Mute / unmute |

## Saves

The game auto-saves every ~10 seconds and on leaving the page, in the browser's `localStorage` under `pumasoc_save`, and picks up where it left off. **NEW GAME** in the header starts again from the difficulty screen.

## Credits

Economy and cost mechanics: [docs/economy.md](docs/economy.md).

By Matthew "Shan" Romanek, with Claude. Built by [greykit.com](https://greykit.com/).

Made for my own play. Nothing here is offered: no support, no updates, no licence.
