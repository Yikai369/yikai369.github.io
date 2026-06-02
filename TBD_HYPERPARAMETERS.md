# TBD Hyperparameters

This document lists parameters that should be **fixed before confirmatory data collection** (or explicitly versioned if changed mid-study). For each item: **what to decide**, **how to decide it**, and **where it lives in code** (mostly [`index.html`](index.html)).

---

## Checklist (short form)

### Stimulus design
- [ ] Define focal image set (size, source balance, ambiguity range using model-derived estimates)
- [50] Decide images per participant (recommended: 50–80; pilot at 50)
- [Yes ] Decide whether all participants see the same set or different subsets

### Timing
- [ ] Naming-onset timeout (current: 5000 ms; tune from pilot RT distribution)
- [ ] Fixation duration (current: 250 ms)
- [ ] Ultrafast RT exclusion floor (current: 150 ms)

### Task and UI
- [2] Practice trial count (current: 2)
- [ ] Enable/disable browser and screen size checks (currently defined but not pushed to timeline)
- [x] Confirm slider start position and require-movement policy (random start per trial, require_movement=true)

### Participant wording (freeze before live collection)
- [ ] Naming-onset prompt (`W_NAMING_ONSET`)
- [ ] Category input prompt (`W_CATEGORY_INPUT`)
- [ ] Rating prompts and scale anchors (`W_RATING_PLEASURE`, `W_RATING_AMBIGUITY`, `W_RATING_CONFIDENCE`)
- [ ] Consent compensation and credit wording (currently contains placeholder "X dollars")
- [ ] Prolific completion code (currently placeholder `XXXXXXX`)

### QC and exclusions (predefine before analysis)
- [ ] Max per-participant timeout rate
- [ ] Max per-participant straightline rate
- [ ] Minimum label quality rule
- [ ] Trial completeness definition

### Study-level
- [ ] Target sample size N
- [ ] Stopping rule (fixed N vs sequential)
- [ ] Confirm subject ID source (random vs Prolific PID)

---

---

## 1. Design and timing

| Parameter | What to fix | How to determine | Current default (check code) |
|-----------|-------------|------------------|------------------------------|
| **Main trials per participant** | Total images in the assigned chunk(s) or subsample | Pilot: completion time, fatigue (rating drift by trial index), dropout; power for primary model | Driven by `chunkList[chunk_index]` length; `chunk_index` is hardcoded in `getRandomChunk()` |
| **Practice trial count** | N practice images | Pilot: error/timeout on first main trials; keep minimal | First **2** entries of `imageNames` as `practice_stimuli` |
| **Naming-onset timeout** (`trial_duration`) | Max wait for SPACE | Pilot RT distribution; set near tail where valid responses thin (e.g. 90–95th percentile) | **5000 ms** on naming-onset keyboard trial |
| **Ultrafast RT floor** | Flag threshold (ms) | Literature + pilot false-positive rate | **150 ms** in `flag_rt_too_fast` |
| **Fixation duration** | ms | Keep short unless you need stronger reset | **250 ms** |
| **Post-trial gap** | ms after instruction screens | Reduce anticipatory rhythm if needed | **1000 ms** after task-start instructions only |
| **Slider range** | min/max | Match analysis and anchors | **1–7** for all rating sliders |
| **Slider start** | initial handle position | Avoid midpoint bias vs end anchors | **4** (middle of 1–7) |
| **Require slider movement** | boolean | Reduces default-middle responding | **true** (`require_movement`) |
| **Chunk assignment** | Which chunk(s), randomization across participants | Balance stimuli and avoid always-chunk-0 confound | Currently **fixed** `randomIndex = 0` in `getRandomChunk()` |
| **Main trial order** | shuffle vs fixed | Randomize unless order is a factor | **`randomize_order: true`** on main procedure |
| **Browser / screen gates** | min width/height, mobile exclusion | Device matrix from pilot | `800×600` defined; **phonecheck / widthcheck are not pushed to timeline**—decide if they should run |

---

## 2. Sampling and study metadata

| Parameter | What to fix | How to determine | Current default |
|-----------|-------------|------------------|-----------------|
| **Target N** | Participants | Power / simulation on primary endpoint + expected exclusions | TBD |
| **Stopping rule** | Fixed N vs sequential | Prereg | TBD |
| **`experiment_version` string** | Version tag in data | Bump on any material change to task or wording | `"ambiguity_naming_v2"` in `addProperties` |
| **Firebase `study` tag** | Saved payload label | Distinct from other studies in same DB | `"ambiguity_rating_naming_v2"` |
| **Subject ID source** | Random vs Prolific PID | IRB + deduplication policy | Random 8-char; Prolific URL vars **commented out** |

---

## 3. Quality control and exclusion (predefine before analysis)

| Parameter | What to fix | How to determine | Current / notes |
|-----------|-------------|------------------|-----------------|
| **Max timeout rate per participant** | e.g. exclude if > X% | Pilot distribution | TBD |
| **Max straightline rate** | Same rating value across constructs | Pilot; note current flag only checks last 3 ratings in one trial | TBD |
| **Min label length / content rules** | Exclude or flag | Pilot nonsense rate; `flag_label_too_short` exists for length < 2 | Partial |
| **Trial completeness rule** | Required rows per `trial_uid` | Define in analysis script | TBD (not yet a saved column) |
| **Save failure handling** | Retry, copy, contact | UX + data loss risk | Alert only; no retry loop yet |

---

## 4. Participant-facing wording as hyperparameters

Treat every string participants read as a **versioned hyperparameter**: small wording changes can shift behavior (speed–accuracy tradeoff, interpretation of “category,” affect before ratings). Freeze text with the same rigor as timing.

Below: **ID** (for prereg), **purpose**, **what is still TBD**, **current text or location** in [`index.html`](index.html).

### 4.1 Global / frame

| ID | Purpose | TBD / decisions | Current (summary) |
|----|---------|-----------------|---------------------|
| **W_PAGE_TITLE** | Browser tab | Exact title | `<title>My experiment</title>` |
| **W_PRELOAD** | Expectations during load | Tone, time estimate | “The experiment is loading. This may take a few minutes.” |
| **W_MOBILE_EXCLUDE** | If phone check is enabled | Exact exclusion copy | “You must use a desktop/laptop computer…” |
| **W_WELCOME** | First impression, do-not-close | Compensation mention? | “Welcome!” + consent next + do not close |
| **W_CONSENT_FULL** | Legal and ethical content | IRB-approved text; compensation amount; credit vs money; debrief promise | Long HTML block in `consent.message`; contains placeholder “X dollars” and mixed credit language—**needs IRB pass** |

### 4.2 Task instructions (before practice / main)

| ID | Purpose | TBD / decisions | Current (summary) |
|----|---------|-----------------|---------------------|
| **W_INSTR_OVERVIEW** | High-level task | “Category” vs “object name” vs “one or two words” | Images center; SPACE when name comes to mind; type name; sliders; practice mentioned |
| **W_INSTR_SEQUENCE** | Step order and timeout | Whether to mention timeout duration (5 s) | Numbered steps 1–3; “image will continue automatically” if no name—**duration not stated** |
| **W_BUTTON_CONTINUE** | Button labels | Localization | “Continue” / “Start” / “I agree” / “End” |

### 4.3 Per-trial prompts (core construct wording)

| ID | Purpose | TBD / decisions | Current (summary) |
|----|---------|-----------------|---------------------|
| **W_NAMING_ONSET** | Speeded naming intention | “Category” definition; key (SPACE only); warn accidental presses | “Press SPACE as soon as a category/name comes to mind.” |
| **W_CATEGORY_INPUT** | Free label constraint | One word vs phrase; language; examples | “Type the category/name that best matches the image:” + required text field |
| **W_RATING_PLEASURE** | Affective construct | Hedonic vs liking vs beauty | “How **pleasant** is this image?” |
| **W_RATING_AMBIGUITY** | Ambiguity construct | Interpretive vs perceptual ambiguity | “How **ambiguous** is this image?” |
| **W_RATING_CONFIDENCE** | Metacognitive referent | Confidence in *name* vs *category* vs *memory* | “How confident are you in the category name you provided?” |
| **W_SLIDER_ANCHORS_PLEASURE** | Scale endpoints | Valence wording | “Very unpleasant” … “Very pleasant” (1–7 labels at ends) |
| **W_SLIDER_ANCHORS_AMBIGUITY** | Scale endpoints | | “Not at all ambiguous” … “Very ambiguous” |
| **W_SLIDER_ANCHORS_CONFIDENCE** | Scale endpoints | | “Not confident at all” … “Very confident” |

### 4.4 Practice feedback

| ID | Purpose | TBD / decisions | Current (summary) |
|----|---------|-----------------|---------------------|
| **W_PRACTICE_FEEDBACK** | Learning without revealing answers | How much detail; encouragement vs neutral | SPACE captured / timed out; typed accepted / missing; reminder to complete all steps |

### 4.5 Post-task and exit

| ID | Purpose | TBD / decisions | Current (summary) |
|----|---------|-----------------|---------------------|
| **W_EXP_END** | Transition to demographics | | Experiment completed; continue questions; do not close |
| **W_DEMOGRAPHICS** | Fields and labels | Which fields; free text vs constrained; “Prolific ID” vs SONA | Form includes Prolific ID, gender, age, mother tongue, ethnicity, art training |
| **W_DEBRIEF** | Scientific purpose | Match actual task (naming + ratings) | Still emphasizes ambiguity ratings and pleasure relationship—**may need update** if primary claims shift |
| **W_SAVE** | Wait behavior | | “Saving data, please stay…” |
| **W_PROLIFIC_EXIT** | Completion URL | Real completion code vs `XXXXXXX` | Link contains placeholder `XXXXXXX`—**must replace** before live Prolific |

### 4.6 Error / edge messages

| ID | Purpose | TBD / decisions | Current |
|----|---------|-----------------|---------|
| **W_SAVE_FAIL** | Data loss mitigation | Retry UI, contact, copy JSON | `alert("Data save failed...")` |

---

## 5. How to lock hyperparameters (recommended workflow)

1. **Draft** all wording IDs in a table (this file) and circulate for IRB / advisor review.
2. **Pilot** (small N): log timing distributions, comprehension issues, and open-ended feedback on instructions.
3. **Revise** text and timing only in a controlled way; bump **`experiment_version`** when anything material changes.
4. **Freeze** a one-page “run sheet” (numbers + final strings) and treat it as the analysis contract.
5. **Analyze** with pre-registered exclusion rules tied to stored flags, not ad hoc tuning on the confirmatory set.

---

## 6. Cross-reference

- Implementation details and metric gaps: [`METRICS_IMPROVEMENTS.md`](METRICS_IMPROVEMENTS.md)
- Executable task logic: [`index.html`](index.html)
