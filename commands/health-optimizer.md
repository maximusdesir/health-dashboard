Daily recap + look-ahead note. Recaps **yesterday** (training executed, deep-work hours, recovery outcome, regulation adherence) and plans **today** across three domains — physical training, deep work / learning, and recovery regulation (NSDR, naps, supplements, light/lights-out). Pulls biometric data (WHOOP + Withings), cross-references your scheduled session and learning goals, applies autoregulation logic, and saves a daily note to your vault. Run it in the morning.

Requires: WHOOP MCP, Withings MCP. Optional: Hevy MCP.
Vault path: set `VAULT_PATH` env var, or defaults to `~/vault`.

> **Evidence & Confidence**
> Recovery gate thresholds are WHOOP vendor-defined product bands — not externally validated physiology.
> Volume adjustments (−20%/−40%) are practical heuristics; no published study derives these specific numbers.
> Mg Glycinate and L-Theanine+caffeine have Moderate RCT support. Glycine 3g has small-n RCT support.
> Apigenin has preclinical and chamomile-extract data only — no human RCT exists at the 50mg supplement dose.
> NSDR reduces fatigue perception and stress; it does not biochemically repay sleep debt.
> PBM (red light): moderate evidence for targeted pre/post-exercise muscle application; AM facial
> and whole-body protocols have weaker support.
> Cold water within 4–6h of resistance training attenuates hypertrophy signaling (strong evidence —
> Roberts 2015; PMC11235606 meta-analysis 2023). Schedule cold on non-lift days only.
>
> **Disclaimer:** Self-tracking and decision-support only. Not medical advice. Consumer wearables carry
> measurement error — treat all outputs as relative signals, not clinical readings. Supplement names
> in this template are illustrative — do not adopt any compound without personalizing to your own needs
> and consulting a qualified clinician or registered dietitian.

---

## Setup

Run these to anchor the recap (yesterday) and the plan (today):
```bash
date +%Y-%m-%d                    # TODAY
date +%A                          # today's weekday
date -d yesterday +%Y-%m-%d       # YESTERDAY
date -d yesterday +%A             # yesterday's weekday
```

Determine `VAULT_PATH`:
```bash
echo "${VAULT_PATH:-$HOME/vault}"
```

The **recap** covers YESTERDAY. The **plan** covers TODAY.

---

## Phase 1 — Parallel Data Pull

Run ALL of the following simultaneously. Do not wait for one before starting the others.

### 1a. WHOOP — Today's Status (for the plan)

Call all at once:
- `whoop_recovery` — this morning's recovery score, HRV, RHR, contributors, baseline comparison
- `whoop_sleep` — last night: duration, stages (SWS%, REM%), sleep performance %, debt
- `whoop_strain` — today's strain target range and current strain
- `whoop_sleep_need` — current sleep debt in hours

### 1b. WHOOP — Yesterday (for the recap)

- `whoop_day` date=`<YESTERDAY>` — yesterday's recovery, sleep, and strain achieved
- `whoop_workouts` — last 2 days, to confirm whether yesterday's session actually happened and at what strain

### 1c. Withings — Body Composition

Call:
- `get_measures` — latest weight + body fat % (last 3 days for context)

### 1d. Vault — Yesterday's Notes + Today's Plan Files

Read **yesterday's** notes (recap) and **today's** session/goals files (plan), all resolved from `$VAULT_PATH`.

**Yesterday's recap notes** (in `$VAULT_PATH/0 Inbox/Daily/`):
- `<YESTERDAY>.md` — yesterday's daily note: what was the planned session, stack, directive? Compare against what actually happened.
- A deep-work / study log for yesterday if you keep one (e.g. `<YESTERDAY> Deep Work.md`) — topics, logged duration, active recall, gaps.

**Today's training file:** based on today's day of week, read the corresponding training file from `$VAULT_PATH`.
1. Check `$VAULT_PATH/6 Training/` (or a similar training folder) for a file containing today's day name.
2. Fall back to a weekly schedule file if individual day files don't exist.
3. If none found, note "no vault training data — using manual schedule" and proceed.

**Today's timing/schedule file (optional):** if you keep per-day schedule notes (wake, work shift, nap, meal/supplement times, lights-out), read today's. These times vary by day — pull them from the note rather than assuming a fixed daily rhythm.

**Learning goals (for the learning plan):** read your goals file (look for `Goals*` / `goals*` in your life/personal folder) and any deep-work protocol note. If you have no learning goals configured, skip the Learning Plan section.

### 1e. Deep-Work Hours — Yesterday

If you track deep-work time with a tool (timer app, NFC tracker, exported CSV), pull yesterday's total — this is the authoritative duration. Use your study log (1d) for *what* was covered; use the tracker for *how long*. If you have no tracker, fall back to the logged duration in yesterday's note.

---

## Phase 2 — Recap of Yesterday

Reconstruct what actually happened and grade adherence. Keep it tight — this is a quick morning review, not the weekly audit.

### A. Training executed
- Did yesterday's planned session happen? Cross-check the daily note's plan vs `whoop_workouts` and the study log.
- Strain achieved vs WHOOP target for the day. Hit, over, or under?

### B. Deep work
- Tracked hours yesterday vs the day's intent. From the study log, what was covered and which goal did it serve?
- If a tracker number and a logged number both exist and diverge by >10 min, flag it in one line.
- Any active-recall gaps logged yesterday worth carrying into today?

### C. Recovery outcome
- This morning's recovery score is the *result* of yesterday's load + last night's sleep. Did yesterday's choices (late night, high strain, skipped nap, caffeine timing) show up in the number?

### D. Regulation adherence
- From yesterday's note: supplements taken on time? Nap taken if planned? Lights-out hit? Caffeine cutoff respected?
- Flag every deviation in one line each. State the miss and its likely cost — no lecture.

---

## Phase 3 — Plan Today

### A. Recovery Gate

> WHOOP recovery bands are proprietary product thresholds — use as a consistent relative guide.
> The −20%/−40% adjustments are practical heuristics, not evidence-derived numbers. Adjust to
> match your own training history and response.

| WHOOP Recovery | Action |
|---|---|
| ≥ 67% (Green) | Train as programmed — full volume, full intensity |
| 34–66% (Yellow) | −20% volume (drop one set per compound), keep all working weights # heuristic |
| < 33% (Red) | Compounds only, −40% sets, no max-effort work — or swap to active recovery # heuristic |

### B. Sleep Debt Modifier

> These tiers are practical prompts, not evidence-derived thresholds. NSDR reduces perceived
> fatigue — it does not biochemically repay sleep debt.

| Sleep Debt | Modifier |
|---|---|
| < 30 min | No change |
| 30–60 min | Optional: 15-min NSDR before training to reduce perceived fatigue |
| > 60 min | Consider shifting training later if schedule allows; cap intensity one tier below gate. Add a planned nap / extra NSDR block. |

### C. Session Plan

Write out today's modified session:
- List each exercise with adjusted sets × rep range
- Note any substitutions or dropped movements
- Flag anything that should be treated as an RPE cap

### D. Learning Plan

If you have learning goals (1d), map today to them. Be specific — name the next concrete unit, not "study."
- **Today's deep-work target:** which goal(s) to advance, and the next unit (e.g. next course module, next chapter, next problem set).
- **Carry-over:** any active-recall gap from yesterday's recap to re-test today.
- **Time block:** where the deep-work block fits around today's work/training timing. Protect at least one block.
- If a goal has had zero attention in recent daily notes, surface it as today's priority.

### E. Nutrition Targets

Classify today's load (based on session type + recovery gate result):

| Load | Calories | Protein | Carbs | Fat |
|---|---|---|---|---|
| Heavy (Green gate, full session) | Per your plan | Per your plan | Per your plan | Per your plan |
| Moderate (Yellow gate or light session) | −200–300 kcal | Unchanged | −30–50g | Unchanged |
| Light / Off (Red gate or rest day) | −400–500 kcal | Unchanged | −60–80g | Unchanged |

Fill in the actual numbers from your nutrition protocol.

> Post-workout nutrition: total daily protein intake and distribution every 3–4h matter more than
> a narrow post-workout window. The "45-minute anabolic window" is largely unsupported by current
> ISSN positions (2017). Eat soon after training when practical — the urgency is overstated.

### F. Recovery + Regulation Stack for Today

Based on yesterday's sleep performance, today's training, and the recovery gate. Replace the
illustrative supplement names with your own personalized stack.

- **Regulation matched to the data:** if recovery is low or sleep debt > 60 min, proactively schedule
  extra regulation — a planned nap, an added NSDR block, an earlier lights-out, or a lower strain
  ceiling. Match the intervention to the deficit the data shows.
- **Evening supplements:** your wind-down stack, timed relative to lights-out.
- **Pre-bed protein** (e.g. casein) if part of your daily floor.
- **Lights-out** target, adjusted ±15 min for sleep debt.
- **Sauna / cold:** per your protocol.
  - Cold: do not schedule within 4–6h of resistance training. Strong evidence that post-lift cold
    water immersion blunts hypertrophic adaptations (Roberts 2015; PMC11235606 meta-analysis 2023).
    Cold is appropriate on sprint, field, conditioning, and rest days.
- **Caffeine cutoff:** per your protocol — flag any planned dose past your cutoff.
- **Red light:** AM and/or post-train per your protocol.
- Flag any protocol deviations baked into today's plan.

---

## Phase 4 — Write Daily Note

Save the note to:
```
$VAULT_PATH/0 Inbox/Daily/YYYY-MM-DD.md
```

Use this exact template:

```markdown
---
date: YYYY-MM-DD
type: daily
tags: [daily, training, recovery, learning]
recovery_score: [N]%
hrv: [N] ms
sleep_debt: [N] min
---

# WEEKDAY, Month Day

## Yesterday — Recap

| Domain | Result |
|---|---|
| Training | [planned session — done / missed / modified] · strain N vs target N |
| Deep work | Nh Nm · [topic → goal] · [gaps: …] |
| Recovery driver | [what in yesterday's load/sleep shows up in today's score] |
| Regulation | [supplements / nap / lights-out / caffeine — hits & misses] |

> [One sentence: the single lesson from yesterday to apply today.]

---

## Today — Recovery
**Score:** N% ([Green/Yellow/Red]) · **HRV:** N ms ([+/-N%] vs baseline) · **RHR:** N bpm
**Sleep last night:** Nh Nm · **Performance:** N% · **SWS:** N% · **REM:** N%
**Sleep debt:** N min

> [One sentence: what the recovery data means for today's performance]

---

## Today's Session — [Session Name]

**Gate:** [Green / Yellow / Red] → [full / −20% volume / compounds only]

| Exercise | Sets × Reps | Notes |
|---|---|---|
| Exercise 1 | N × N–N | [any cues or adjustments] |
| Exercise 2 | N × N–N | |
| ... | | |

[If Red gate: "Swap to: 20-min walk + mobility + NSDR"]

---

## Learning

**Block:** [time window around work/training]
| Goal | Next unit today | Carry-over |
|---|---|---|
| | | |

---

## Nutrition

**Load class:** [Heavy / Moderate / Light]
**Target:** ~N kcal · Ng protein · Ng carbs · Ng fat

**Peri-workout:**
- Pre (30–45 min): [carbs + caffeine if applicable]
- Post (within 2–3h, ideally soon after): [protein + carbs]
  # Total daily protein distribution matters more than a narrow post-workout window

---

## Recovery + Regulation Stack Today

| Protocol | Target |
|---|---|
| Last food | [your cutoff] |
| Nap | [planned / extra if low recovery / —] |
| NSDR | [yes/no, timing — add one if debt > 60 min] |
| Supplements | [your evening stack] |
| Pre-bed protein | [yes/no] |
| Sauna | [yes/no] |
| Cold | [yes/no] # Not within 4–6h of resistance training |
| Lights out | N:NN PM |

---

## Today's Directive

[One sentence: the single most important thing to execute today — training, recovery, or learning.]
```

---

## Phase 5 — Report Back

After saving the note, tell the user:
- File path saved
- Yesterday's one-line lesson
- Recovery gate (Green/Yellow/Red) and what it means for today's session
- Today's learning target
- Today's directive in one line
