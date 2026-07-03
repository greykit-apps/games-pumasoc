# PUMA SOC — Economy & Cost Mechanics

> Design reference for all budget, income, spending, and scoring systems.
> All values sourced from `index.html` constants and runtime logic.
> Regenerated 2026-07-03 after the bug-fix and balance pass; supersedes the earlier
> version, which described a removed per-cycle fee model.

---

## 1. Starting Position

| Difficulty | Budget   | Income/cycle | Analysts | Coverage | Starting vulns (H/M/L) | Vuln score |
|------------|----------|--------------|----------|----------|------------------------|------------|
| Analyst    | $800,000 | $75,000      | 3        | 45%      | 4 / 4 / 4              | ≈46        |
| Engineer   | $500,000 | $50,000      | 2        | 25%      | 6 / 6 / 5              | ≈64        |
| Hunter     | $300,000 | $30,000      | 1        | 10%      | 8 / 8 / 6              | ≈82        |

Budget carries over between weeks — there is no reset.

## 2. Income

- **Budget allocation:** fixed amount every **360 game-minutes** (6 game-hours).
- **Week decay:** the allocation is multiplied at each week transition —
  ×0.85 (→W2), ×0.80 (→W3), ×0.75 (→W4). Engineer: $50k → $42.5k → $34k → $25.5k.
- **Incident bonuses:** each completed IR pays **$5k–$20k × the same decay factor**
  (minimum $1k), credited the moment RECOVER finishes. Because rewards decay with
  income, late-week incident volume cannot out-earn the shrinking budget line.

## 3. Analyst Workforce

- **Hire:** $75,000, maximum **8** analysts.
- Analyst activities require **2+ total analysts** and one free slot:

| Activity          | Cost | Duration | Effect |
|-------------------|------|----------|--------|
| Send to Training  | $50k | 24h      | +10% IR speed for all analysts, permanent (max +30%) |
| Threat Hunt       | $60k | 12h      | Surfaces 1–3 currently-undetected threats as incidents |
| Tabletop Exercise | $25k | 8h       | +15% IR speed for 48h after completion |
| Patch Sprint      | $25k | 8h       | Patches every **discovered LOW** vuln at no extra cost |
| OSINT Research    | $40k | 12h      | Loads 3 charges — next 3 spawns auto-detected |

## 4. Detection & MTTD

- **Rule upgrades:** $25k / $30k / $35k / $40k / $50k per level (0→5), 12 ATT&CK categories.
  Each level: **+10% detection chance for that category's threats**, **+5%÷12 to global
  coverage** (i.e., the coverage stat is the 12-category average), **−8 min MTTD**.
- **MTTD is live gameplay**, not just a stat: undetected threats are surfaced by an
  exponential process whose **mean time = current MTTD** (base 240 min, floor 30).
  Undetected threats breach after 720/600/480 game-min (Analyst/Engineer/Hunter);
  campaign-phase-3 actors halve that window. At base MTTD the breach chance per
  undetected threat is ~5–14%; a tuned program pushes it near zero.
- **Startup grace (week 1 only):** the undetected-breach window is ×1.5 on day 1
  and ×1.25 on day 2, then full pressure from day 3. First threat spawns after
  240/300/180 game-min (Analyst/Engineer/Hunter) — Engineer gets a full real-time
  minute of setup; Hunter gets no breathing room by design.
- **Intel feeds** (7-day subscriptions from purchase):

| Feed (week unlocked)           | Cost  | Detection | MTTD    | Vuln bonus |
|--------------------------------|-------|-----------|---------|------------|
| OSINT Aggregator (W1)          | $30k  | +5%       | −15 min | —          |
| IOC Feed Premium (W1)          | $55k  | +8%       | −25 min | —          |
| Nation-State Intel (W1)        | $80k  | +12%      | −40 min | —          |
| Dark Web Monitor (W2)          | $100k | +12%      | −35 min | —          |
| Zero-Day Exchange (W2)         | $120k | +8%       | —       | −25        |
| Threat Graph Analytics (W3)    | $140k | +15%      | −40 min | —          |
| Adversary Profile Database (W3)| $160k | +15%      | −40 min | —          |
| Predictive Threat Engine (W4)  | $175k | +15%      | −50 min | —          |
| Insider Threat Monitor (W4)    | $190k | +12%      | −30 min | −20        |

## 5. Vulnerability Management

- **Scan:** $15,000, takes 2 game-hours, reveals 60–80% of undiscovered vulns.
- **Patch costs:** CRIT $15–25k · HIGH $8–12k · MED $3–6k · LOW $1–3k.
  "Patch Critical" covers CRIT+HIGH; bulk patching applies a **15% discount**.
- **Vuln score** = 10 + 15×CRIT + 7×HIGH + 2×MED over **all unpatched vulns,
  discovered or not** (cap 100). Scanning never raises the score — it only reveals
  what you can patch. Intel-feed and consultant vuln bonuses subtract from the
  *effective* score used for display and spawn rate.
- **Spawn pressure:** spawn interval divisor includes `1 + (effectiveScore − 50)/100`
  — score 80 ⇒ ~1.3× threats, score 20 ⇒ ~0.7×.
- **New CVEs:** 1–3 per day; week transitions add 6/7/9 more with CRIT chance
  25%/35%/45% (W2/W3/W4).

## 6. SOAR

- Platform $100,000 (one-time, persists), playbooks $20,000 per level,
  2 levels per category: −25%/−50% TRIAGE+CONTAIN time.
- Playbooks decay 1 level at each week transition.

## 7. Partners

### Security Awareness Training — 7-day terms
Initial Access is weighted to ~27% of all spawns, so the intercept is meaningful.

| Tier               | Startup | Renewal | Intercept | Coverage |
|--------------------|---------|---------|-----------|----------|
| Phishing Simulation| $20k    | $15k    | 25%       | —        |
| Awareness Program  | $50k    | $30k    | 40%       | +5%      |
| Human Risk Mgt.    | $100k   | $40k    | 60%       | +10%     |

### MSSP — 7-day contracts

| Tier            | Startup | Renewal | Benefits |
|-----------------|---------|---------|----------|
| Alert Monitoring| $100k   | $10k    | Auto-assigns free analysts to new incidents |
| Co-Managed SOC  | $225k   | $25k    | + auto lessons-learned sign-off, +10% IR speed |
| Fully Managed   | $350k   | $50k    | + one overflow incident worked at 60% speed, +15% IR speed |

### Red Team — one-shot engagements

| Engagement                  | Cost  | Duration | Suppression target |
|-----------------------------|-------|----------|--------------------|
| Phishing Simulation         | $30k  | 12h      | Initial Access |
| External Network Pentest    | $50k  | 24h      | Initial Access / Discovery |
| Internal Network Assessment | $75k  | 48h      | Lateral Movement / Priv Esc / Cred Access |
| Cloud Security Review       | $100k | 36h      | Persistence / Priv Esc |
| Full Red Team Engagement    | $150k | 72h      | Your weakest-rule category (72h suppression) |

On completion: **+2–6% permanent coverage** (total red-team bonus capped at +10%),
1–4 hidden vulns discovered, and 48h suppression (72h for the full engagement) —
suppressed categories auto-detect on spawn.

### Consultants — $75k, 7 game-days

| Consultant     | Bonus |
|----------------|-------|
| Cassandra Voss | +8% coverage, −20 min MTTD |
| Marcus Webb    | +3% coverage, −20 effective vuln score |
| Priya Nair     | +5% coverage, −10 min MTTD, −10 effective vuln score |

Consultants also deliver a daily prioritised recommendation. They are cancelled
if the budget is zero at an income tick.

## 8. Lessons Learned

When IR completes, the analyst is freed and the incident bonus paid **immediately**.
A 2-hour sign-off window then offers an optional **+1% IR speed** for that technique
category (stacks to +25% per category). Letting it lapse costs nothing.

## 9. Week Transitions

| Item              | Effect |
|-------------------|--------|
| Budget            | Carries over |
| Income            | ×0.85 / ×0.80 / ×0.75 |
| Detection rules   | −1 level (→W2), −2 (→W3, →W4) per category |
| SOAR playbooks    | −1 level per category |
| Spawn multiplier  | ×1.15 |
| New CVEs          | 6 / 7 / 9, higher severity mix each week |
| Intel/consultant/MSSP/SAT | Unaffected — they expire on their own 7-day clocks |
| Closed incidents  | Pruned from history (keeps saves small) |

## 10. Random Events

First event fires on day 2–4, then every **1.5–3 game-days**:

| Event                  | Effect |
|------------------------|--------|
| DDoS Barrage           | −20% coverage for 8h |
| Tool Outage            | −15% coverage for 4h |
| Zero-Day In the Wild   | Immediate skill-5 threat (auto-detected) |
| Insider Threat         | Insider incident spawns directly |
| Emergency Budget Audit | Seizes **max($20k, 10% of current budget)** |

## 11. Scoring

Breaches are a hard floor: 3 = **F**, 2 = **D**, 1 = **C** (per week; campaign
total of 3 ends the run). Zero-breach grades:

- **S** — coverage ≥75%, MTTD ≤60, unpatched vulns ≤3, open incidents ≤2
- **A** — coverage ≥50%, MTTD ≤120, unpatched vulns ≤8, open incidents ≤5
- **B** — everything else

"Unpatched" counts **all** unpatched vulns, discovered or not; breached and
pending-sign-off incidents don't count as open. Budget does not affect the grade —
money is the means, not the end.

## 12. Budget Display Thresholds

🟢 ≥ $150k healthy · 🟡 $50k–$149k caution · 🔴 < $50k critical
