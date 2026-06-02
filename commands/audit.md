Weekly personal audit: deep work pace, physical performance, body composition, and schedule alignment. Outputs a direct coaching report saved to your vault.

Requires: WHOOP MCP, Withings MCP, Google Calendar MCP. Optional: Hevy MCP.
Vault path: set `VAULT_PATH` env var, or defaults to `~/vault`.

> **Evidence & Confidence**
> HRV-guided autoregulation has moderate-to-strong meta-analytic support as a general approach.
> WHOOP recovery score is a proprietary composite — not independently validated for performance
> prediction. WHOOP HRV (PPG-derived) has acceptable but not clinical-grade accuracy vs ECG.
> Withings consumer BIA body fat % has ±1–3% day-to-day variation from hydration alone — use
> ≥4-week morning-fasted trends, not single readings. The WHOOP↔Withings framework below is
> heuristic pattern-matching from noisy signals; treat it as a prompt for investigation, not a
> conclusion. HRV absolute targets are highly individual — track your own trend, not a fixed number.
>
> **Disclaimer:** Self-tracking and decision-support only. Not medical advice. Consumer wearables
> carry measurement error. Consult a clinician for symptoms, injuries, or health concerns.

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
- RHR trend: rising RHR across multiple days signals accumulating fatigue or illness
- Sleep debt from `whoop_sleep_need`

**Sleep:**
- Average nightly duration vs. 7.5h target
- Sleep performance % and consistency score
- Any nights under 7h — name them

**Body composition — WHOOP↔Withings heuristic framework:**

Treat each pattern as a prompt for investigation, not a conclusion. Withings BIA has ±1–3% day-to-day
variation from hydration alone. Use ≥4-week morning-fasted trends. Do not act on single-week BF%
changes of <2%.

| Pattern | Signal |
|---|---|
| High strain + weight UP | Probable water retention / glycogen loading — do not panic cut; monitor 5–7 days |
| Low strain + weight UP | Possible fat accumulation OR hydration artifact — act only if trend persists ≥2 weeks |
| Declining HRV + weight dropping fast | Prompt: check protein/calorie adequacy AND other stressors (illness, stress, poor sleep) |
| BF% flat despite consistent training | Possible nutrition gap OR BIA noise — verify with ≥4-week trend before acting |
| BF% dropping + HRV stable/rising | Positive recomp signal — reinforce; confirm over ≥4 weeks, morning-fasted, same conditions |

> Standardize Withings measurements: every morning before food/fluid, after voiding, same conditions.

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

**Tone: direct, specific, no hedging.** Give concrete numbers. Every recommendation tied to a data point. Praise only when genuinely earned and only briefly.

For every 🔴 domain: state the gap precisely, then add one line: **Next action:** [the single most important step to close it this week].

Save to: `$VAULT_PATH/0 Inbox/Weekly/YYYY-MM-DD Weekly Audit.md`

> **Configure your goals before using this template.** Replace every `[YOUR_*]` placeholder in the
> Goal Countdown section with your actual goals. See `README.md` → "Configure Your Targets."

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
[If 🔴: **Next action:** ...]

---

## Physical

### Training
[Workouts, strain, lift progression]
[If 🔴: **Next action:** ...]

### Recovery
[HRV trend, recovery scores, RHR, sleep debt]
[If 🔴: **Next action:** ...]

### Sleep
[Avg duration, performance %, nights under 7h]

### Body Composition
[14-day weight + BF% trend, WHOOP↔Withings flag if applicable]
> Note: single-week BF% shifts of <2% are within Withings BIA measurement noise.

### Fitness Tier
[Performance assessment result, WHOOP coach key insight]

---

## Schedule

[Past week load, upcoming week classification, conflicts to protect]

---

## Directives

[6–8 numbered, specific actions for the coming week. Each tied to a data point.]

1.
2.
3.
4.
5.
6.

---

## Goal Countdown

| Goal | Current | Target | Gap | On Track? |
|---|---|---|---|---|
| [YOUR LEARNING GOAL 1] | [X] | [Target] | [N] | ✅ / ⚠️ / ❌ |
| [YOUR LEARNING GOAL 2] | [X] | [Target] | [N] | ✅ / ⚠️ / ❌ |
| [PRIMARY LIFT 1] | [X lb/kg] | [YOUR_TARGET] | [N] | ✅ / ⚠️ / ❌ |
| [PRIMARY LIFT 2] | [X lb/kg] | [YOUR_TARGET] | [N] | ✅ / ⚠️ / ❌ |
| Weight / BF% | [X / Y%] | [YOUR_TARGET] | — | ✅ / ⚠️ / ❌ |
| VO2 Max (estimated) | [X] | [YOUR_TARGET] | [N] | ✅ / ⚠️ / ❌ |
| HRV (28d trend) | [X ms avg] | ↑ trend | — | ✅ / ⚠️ / ❌ |
| RHR | [X bpm] | [YOUR_TARGET] | [N] | ✅ / ⚠️ / ❌ |
| [ANY OTHER GOAL] | [X] | [Target] | [N] | ✅ / ⚠️ / ❌ |

> HRV: tracking your 28-day trend direction matters more than any absolute number. Cross-person
> HRV targets are not physiologically meaningful due to large individual variation.
> VO2 Max: WHOOP estimate is a proxy — not a validated VO2max measurement.
```

---

## Phase 4 — Save Report

Filename: `YYYY-MM-DD Weekly Audit.md`
Save to: `$VAULT_PATH/0 Inbox/Weekly/`
Confirm the saved path.
