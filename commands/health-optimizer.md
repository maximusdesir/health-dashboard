Daily health optimization note. Pulls today's biometric data (WHOOP + Withings), cross-references with your scheduled training session, applies autoregulation logic, and saves a daily note to your vault.

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
> Roberts 2015; PMC11235606 meta-analysis 2023). The current protocol schedules cold on non-lift days only.
>
> **Disclaimer:** Self-tracking and decision-support only. Not medical advice. Consumer wearables carry
> measurement error — treat all outputs as relative signals, not clinical readings. Supplement names
> in this template are illustrative — do not adopt any compound without personalizing to your own needs
> and consulting a qualified clinician or registered dietitian.

---

## Setup

Run `date +%Y-%m-%d` and `date +%A` to get today's date and day of week.

Determine `VAULT_PATH`:
```bash
echo "${VAULT_PATH:-$HOME/vault}"
```

---

## Phase 1 — Parallel Data Pull

Run ALL of the following simultaneously.

### 1a. WHOOP — Today's Status

Call all at once:
- `whoop_recovery` — recovery score, HRV, RHR, contributors, baseline comparison
- `whoop_sleep` — last night: duration, stages (SWS%, REM%), sleep performance %, debt
- `whoop_strain` — today's strain target range and current strain
- `whoop_cycle` — today's full physiological cycle summary
- `whoop_sleep_need` — current sleep debt in hours

### 1b. Withings — Body Composition

Call:
- `get_measures` — latest weight + body fat % (last 3 days for context)

### 1c. Training Schedule

Based on today's day of week, read the corresponding training file from `$VAULT_PATH`.

Look for day files in this order:
1. Check `$VAULT_PATH/6 Training/` or a similar training folder for day-specific files
2. Fall back to reading your weekly schedule file if individual day files don't exist

If no training files found, note "no vault training data — using manual schedule" and proceed.

---

## Phase 2 — Autoregulation + Optimization

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
| > 60 min | Consider shifting training later if schedule allows; cap intensity one tier below gate |

### C. Session Plan

Write out today's modified session:
- List each exercise with adjusted sets × rep range
- Note any substitutions or dropped movements
- Flag anything that should be treated as an RPE cap

### D. Nutrition Targets

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

### E. Recovery Stack for Tonight

Based on yesterday's sleep performance and today's training. Replace the illustrative supplement
names below with your own personalized stack.

> Cold: do not schedule within 4–6h of resistance training. Strong evidence that post-lift cold
> water immersion blunts hypertrophic adaptations (Roberts 2015; PMC11235606 meta-analysis 2023).
> Cold is appropriate on sprint, field, conditioning, and rest days.

---

## Phase 3 — Write Daily Note

Save the note to:
```
$VAULT_PATH/0 Inbox/Daily/YYYY-MM-DD.md
```

Use this exact template:

```markdown
---
date: YYYY-MM-DD
type: daily
tags: [daily, training, recovery]
recovery_score: [N]%
hrv: [N] ms
sleep_debt: [N] min
---

# WEEKDAY, Month Day

## Recovery
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

## Nutrition

**Load class:** [Heavy / Moderate / Light]
**Target:** ~N kcal · Ng protein · Ng carbs · Ng fat

**Peri-workout:**
- Pre (30–45 min): [carbs + caffeine if applicable]
- Post (within 2–3h, ideally soon after): [protein + carbs]
  # Total daily protein distribution matters more than a narrow post-workout window

---

## Recovery Stack Tonight

| Protocol | Target |
|---|---|
| Last food | [your cutoff] |
| Supplements | [your evening stack] |
| Lights out | N:NN PM |
| NSDR | [yes/no, timing] |
| Cold | [yes/no] # Not within 4–6h of resistance training |

---

## Today's Directive

[One sentence: the single most important thing to execute today — training, recovery, or learning.]
```

---

## Phase 4 — Report Back

After saving the note, tell the user:
- File path saved
- Recovery gate (Green/Yellow/Red) and what it means for today's session
- Today's directive in one line
