Daily health optimization note. Pulls today's biometric data (WHOOP + Withings), cross-references with your scheduled training session, applies autoregulation logic, and saves a daily note to your vault.

Requires: WHOOP MCP, Withings MCP. Optional: Hevy MCP.
Vault path: set `VAULT_PATH` env var, or defaults to `~/vault`.

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

If no training files found, note "no vault training data — using manual schedule" and proceed with whatever schedule the user has described.

---

## Phase 2 — Autoregulation + Optimization

### A. Recovery Gate

Apply the recovery gate to today's programmed session:

| WHOOP Recovery | Action |
|---|---|
| ≥ 67% (Green) | Train as programmed — full volume, full intensity |
| 34–66% (Yellow) | −20% volume (drop one set per compound), keep all working weights |
| < 33% (Red) | Compounds only, −40% sets, no max-effort work — or swap to active recovery |

### B. Sleep Debt Modifier

| Sleep Debt | Modifier |
|---|---|
| < 30 min | No change |
| 30–60 min | Add 15-min NSDR before training if morning session |
| > 60 min | Flag: consider shifting training to later in day; reduce session intensity one tier |

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

Fill in the actual numbers from the user's nutrition protocol if available.

### E. Recovery Stack for Tonight

Based on yesterday's sleep performance and today's training:
- Supplement timing adjustments (note if magnesium should be higher, ashwagandha cycling day, etc.)
- Lights-out target (adjust ±15 min based on sleep debt)
- Flag any protocol deviations (late caffeine, skipped NSDR, etc.)

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
- Pre (30 min): [carbs + caffeine if applicable]
- Post (45 min): [protein + carbs]

---

## Recovery Stack Tonight

| Protocol | Target |
|---|---|
| Last food | 6:30 PM |
| Supplements | 7:30–8:00 PM — [list] |
| Lights out | N:NN PM |
| NSDR | [yes/no, timing] |

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
