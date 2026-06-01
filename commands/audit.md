Weekly personal audit: deep work pace, physical performance, body composition, and schedule alignment. Outputs a direct coaching report saved to your vault.

Requires: WHOOP MCP, Withings MCP, Google Calendar MCP. Optional: Hevy MCP.
Vault path: set `VAULT_PATH` env var, or defaults to `~/vault`.

---

## Setup

Run `date +%Y-%m-%d` to get today's date.
Audit window: the 7 calendar days ending today (inclusive).

Determine `VAULT_PATH`:
```bash
echo "${VAULT_PATH:-$HOME/vault}"
```

---

## Phase 1 — Parallel Data Pull

Run ALL of the following simultaneously. Do not wait for one before starting the others.

### 1a. Vault — Goals & Protocols

1. Read `$VAULT_PATH` for a goals file (look for files named `Goals*`, `goals*`, or similar in the life/personal folder)
2. Read any training, recovery, nutrition, and deep work protocol files
3. Find daily notes from the last 7 days:
   ```bash
   find "$VAULT_PATH/0 Inbox/Daily" -name "*.md" -mtime -8 2>/dev/null | sort
   ```
   Read every file returned. Extract: session topics, durations, active recall, gaps, tasks completed.

### 1b. WHOOP — Training, Sleep & Recovery

Call all of these:
- `whoop_calendar` — 7-day recovery/sleep/strain grid (pass today's date)
- `whoop_recovery` — today's deep-dive (HRV, RHR, contributors, baselines)
- `whoop_sleep` — today's deep-dive (stages, debt, performance, consistency)
- `whoop_strain` — today's deep-dive (strain target, zone distribution)
- `whoop_workouts` — last 7 days of workouts (sport, strain, duration, HR zones)
- `whoop_sleep_need` — current sleep debt
- `whoop_performance_assessment` — fitness tier and VO2 max proxy
- `whoop_trend` metric=`HRV` — week vs monthly baseline
- `whoop_trend` metric=`RECOVERY` — week vs monthly baseline
- `whoop_trend` metric=`SLEEP_PERFORMANCE` — week trend
- `whoop_trend` metric=`DAY_STRAIN` — week vs monthly average
- `whoop_trend` metric=`RHR` — week trend
- `whoop_coach_ask` question=`"Based on my last 7 days, am I overtraining, undertraining, or on track? What is the single most important thing I need to address right now?"`

### 1c. Withings — Body Composition & Activity

Call:
- `get_measures` — weight and body fat % for the last 14 days
- `get_activity` — daily steps and active calories for the last 7 days
- `get_sleep_summary` — last 7 days sleep summary (cross-reference with WHOOP)
- `get_user_goals` — Withings targets if set

### 1d. Google Calendar — Schedule

Call:
- `list_calendars` — get all calendar IDs
- `list_events` — past 7 days across all calendars
- `list_events` — next 7 days across all calendars

### 1e. Hevy — Strength Training Log

If Hevy MCP tools are available:
- Fetch workout history for the last 7 days
- Fetch recent PRs or exercise volume trends
- Fetch routine/program info

If unavailable: note "Hevy: not connected" and use WHOOP workout data as the sole training source.

---

## Phase 2 — Analysis

### A. Learning Audit

If the user has learning goals (CS courses, books, certifications):
- Map deep work sessions to their goals
- Assess pace: is the current rate sufficient to hit the deadline?
- Flag any goal with zero sessions this week
- Note recurring weak spots from active recall

### B. Physical Audit

**Training load:**
- How many workouts vs. the programmed floor?
- Strain distribution — enough high-intensity to drive adaptation?
- Key lift progression (if Hevy available)

**Recovery:**
- 7-day HRV trend: direction and % vs monthly baseline
- Day-by-day recovery scores: green/yellow/red count
- RHR trend: rising = accumulating fatigue
- Sleep debt from `whoop_sleep_need`

**Sleep:**
- Average nightly duration vs. 7.5h target
- Sleep performance % and consistency score
- Any nights under 7h — name them

**Body composition — WHOOP↔Withings framework:**

| Pattern | Signal |
|---|---|
| High strain + weight UP | Water retention / inflammation — not fat gain |
| Low strain + weight UP | Potential fat gain — flag nutrition audit |
| Declining HRV + weight dropping fast | Muscle loss risk — check protein/calories |
| BF% flat despite consistent training | Nutrition gap — not in deficit or protein low |
| BF% dropping + HRV stable/rising | Optimal recomp — confirm and reinforce |

### C. Schedule Audit

**Past week:** events that consumed significant time, impact on training/sleep floor
**Upcoming week:** Heavy / Moderate / Light classification; days to protect

### D. Cross-Domain Synthesis

Flag these interactions if present:
1. Heavy schedule → poor sleep → low HRV → missed training or degraded deep work
2. High training + insufficient deep work = imbalanced week
3. Declining HRV + rising weight + high strain = overreaching
4. Good recovery + no high-intensity = undertraining
5. Learning ahead, physical lagging = energy misalignment

---

## Phase 3 — Coaching Report

**Tone: direct, demanding, zero sugarcoating.** State failures plainly. Give concrete numbers. Every recommendation tied to a specific data point.

Save to: `$VAULT_PATH/0 Inbox/Weekly/YYYY-MM-DD Weekly Audit.md`

```markdown
---
title: Weekly Audit — YYYY-MM-DD
date: YYYY-MM-DD
type: audit
tags: [audit, weekly]
---

# Weekly Audit — Month Day, Year

## Verdict

[One blunt sentence: on track, slipping, or behind.]

| Domain | Status |
|---|---|
| Learning | 🟢 / 🟡 / 🔴 |
| Training | 🟢 / 🟡 / 🔴 |
| Recovery | 🟢 / 🟡 / 🔴 |
| Schedule | 🟢 / 🟡 / 🔴 |

---

## Learning

[Sessions, hours, pace per goal, gaps flagged]

---

## Physical

### Training
[Workouts, strain, lift progression]

### Recovery
[HRV trend, recovery scores, RHR, sleep debt]

### Sleep
[Avg duration, performance %, nights under 7h]

### Body Composition
[14-day weight + BF% trend, WHOOP↔Withings flag if applicable]

### Fitness Tier
[Performance assessment result, WHOOP coach key insight]

---

## Schedule

[Past week load, upcoming week classification, conflicts to protect]

---

## Directives

[6–8 numbered, specific, non-negotiable orders for the coming week. Each tied to a data point.]

1.
2.
3.
4.
5.
6.
```

---

## Phase 4 — Save Report

Filename: `YYYY-MM-DD Weekly Audit.md`
Save to: `$VAULT_PATH/0 Inbox/Weekly/`
Confirm the saved path.
