# Question List: Answers and Recommendations

Based on careful reasoning about the research goals and stimulus pool structure.
See [QUESTION_LIST.md](QUESTION_LIST.md) for the questions.

---

## Image Pool Overview

The full pool in `all_imageList_str.js` contains **4,914 images** from three distinct sources:

| Source | Count | Character |
|--------|------:|-----------|
| **COCO** | 1,999 | Diverse real-world photos, scene-level, often complex and contextually rich |
| **ImageNet** | 1,915 | Object-centric, often cleaner compositions, less contextual |
| **Named scenes** | 1,000 | 334 specific scene categories (e.g., bakery, bedroom, alley), mostly 4 images per category |

These sources differ systematically in likely ambiguity levels, visual complexity, and aesthetic properties. There is no need to use all 4,914 images — the right subset depends entirely on the research question.

---

## Q1. How many images per participant?

### Recommendation: **50–80 main images per participant** (pilot at 50 first).

### Reasoning

**Practical ceiling**: with a ~1-hour session and ~1 min/image (naming + typing + 3 sliders), after reserving ~10 min for consent, instructions, practice, and debrief, you have roughly **50 minutes of task time**, allowing at most **50–80 images per participant**.

**Statistical floor**: below ~30–40 images per participant, within-subject estimates (e.g., each participant's mean pleasure or mean naming RT) become unreliable. Fifty is a reasonable minimum for stable individual-level means.

**Diminishing returns with fatigue**: typing makes this task heavier than slider-only tasks. Expect rating drift and slower typing after trial 50–60. Monitor this in the pilot by plotting mean RT and rating variance by trial position.

**Conclusion**: 50 images is the right pilot anchor. Scale to 80 only if the pilot shows no fatigue signal in later trials.

---

## Q2. Should every participant have the same set of stimuli?

### Recommendation: **Yes, use the same focal image set for all participants** — but first select a smaller focal set from the pool.

### Reasoning

The primary goal appears to be understanding the relationship between **ambiguity and aesthetic pleasure at the image level**. This means you need reliable **per-image** averages: each image should receive multiple independent ratings.

With 50 images per participant, there are two main approaches:

| Approach | Description | Implication |
|----------|-------------|-------------|
| **Same set for all** | Everyone rates the same 50–200 images | Each image gets N ratings (N = sample size); maximum per-image reliability |
| **Different sets, no overlap** | Each participant sees unique images | Each image gets 1 rating; no reliability estimate possible |
| **Different sets, partial overlap** | Counterbalanced or random subsampling from a focal set | Each image gets several ratings; more total images covered |

Given that you do not need to cover all 4,914 images, the most efficient design is:

1. **Curate a focal image set** of 100–300 images that spans the ambiguity dimension well (see Q3 for how to select these).
2. **Have all participants rate the same focal set**, split into sessions or counterbalanced blocks if the set is larger than 50–80 images per participant.

This maximises per-image reliability and keeps analysis straightforward. If the focal set is 100 images and you run each participant on 50 (a randomly assigned half), every image still gets ratings from half of all participants — enough for reliable means with reasonable N.

---

## Q3. How to sample stimuli: balanced across image subsets, or focused on one subset?

### Recommendation: **Select a balanced focal set across all three image sources** — but weight selection toward maximising variance in ambiguity, not equal counts per source.

### Reasoning

The three image sources differ meaningfully and non-randomly:

- **Named scene images** (334 categories × ~4 images): high within-category similarity, likely lower ambiguity on average (clear category structure). Useful for anchoring the low-ambiguity end of the scale.
- **COCO images**: diverse, contextually rich, scene-level. Likely higher variance in ambiguity. Many images may be genuinely ambiguous at the scene or object level.
- **ImageNet images**: object-centric. Ambiguity may be lower for common objects but higher for unusual or abstract ones.

For studying ambiguity × pleasure, **variance in ambiguity is essential**. A focal set that only draws from one source risks being range-restricted (e.g., all low-ambiguity named scenes).

**Suggested selection strategy**:

1. **Use model-derived ambiguity estimates for principled sampling.** A computational model (e.g., a vision model trained or prompted to predict ambiguity, or a proxy such as classification entropy, image complexity, or feature-space uncertainty) can score all 4,914 images without any human data. Use these scores to stratify the pool by ambiguity level and sample images that span the full range (low, medium, high) in roughly equal proportions. This avoids the risk of accidentally selecting a range-restricted set and makes the sampling procedure reproducible and documentable.
2. If model-based scores are not yet available, sample from all three sources to maximise the chance of obtaining variance in ambiguity:
   - e.g., ~60–80 COCO + ~60–80 ImageNet + ~40–60 named scenes = ~160–200 focal images.
3. Within named scenes, favour categories that are likely to have within-category ambiguity variance (e.g., avoid very prototypical, unambiguous categories like "bedroom" if your pilot confirms those are all rated low-ambiguity).
4. Avoid systematic biases: check that the focal set is roughly balanced for outdoor/indoor, natural/man-made, and scene/object-centric images to ensure generalisability of the ambiguity–pleasure relationship.

### What to avoid

- Sampling only from named scenes → likely under-represents high-ambiguity images.
- Using the full pre-set chunks → chunk boundaries are arbitrary and may not serve a principled stimulus selection goal.
- Covering all 4,914 images → unnecessary; increases participant burden without proportional scientific gain.

---

## Proposed Workflow

1. **Define a focal image set** (~100–200 images) using the selection strategy above.
2. **Set participants to all 50–80 of the same images** per session (random order); if focal set > 80, split into counterbalanced halves.
3. **Pilot with ~20 participants**: check timing, fatigue, ambiguity variance in obtained ratings, and inter-rater reliability.
4. **Adjust focal set** if ambiguity range is too narrow, then freeze before confirmatory collection.

---

## Cross-reference

- Implementation details: [`index.html`](index.html)
- Metric gaps and improvements: [`METRICS_IMPROVEMENTS.md`](METRICS_IMPROVEMENTS.md)
- TBD parameters: [`TBD_HYPERPARAMETERS.md`](TBD_HYPERPARAMETERS.md)
