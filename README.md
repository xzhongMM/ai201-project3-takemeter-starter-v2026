# TakeMeter — fictional running-forum exercise

> **Dataset disclosure:** This is a practice build from the repository's fictional running forum. The first 60 rows in `labels.csv` come from the provided fictional practice set; 140 additional rows were generated for the first run and 20 more synthetic `analysis` examples were added in the improvement run. None are represented as public community posts. Use `data/practice_labels.csv` if you want to run only the supplied examples.

For environment setup and troubleshooting, see [RUNNING.md](RUNNING.md). Run the notebook from this repository with its `.venv` kernel.

## What This Does

This project classifies short posts in a fictional running discussion forum. It uses three labels: `analysis` for arguments supported by checkable facts, `hot_take` for unsupported confident general claims, and `reaction` for immediate personal responses. The dataset is balanced across the labels to make the practice run easier to interpret.

### Reading notes and candidate distinctions

I reviewed 40 posts from the 60-post fictional practice set. Three distinctions appeared: some posts make a checkable comparison and use it to support a point; some make broad confident claims without evidence; and some mainly report a recent personal feeling. A useful boundary is whether a post is about the writer's own immediate experience (`reaction`) or makes a general claim (`hot_take`). These observations are from the supplied fictional set, not a public community.

## Label Taxonomy

### `analysis`

**Definition:** Makes an argument backed by a specific, checkable fact that does real work in the argument.

**Example 1:** “Everyone says the new course is faster but the results don't show it. Median finish was 3:58 last year on the old route and 3:56 this year. Two minutes across 4,000 runners is noise, not a faster course.”

**Example 2:** “Looked at my last three build-ups. The one where I capped long runs at 2 hours had my best race by 4 minutes, despite the lowest weekly mileage of the three. Time on feet past 2 hours was costing me the next four days.”

### `hot_take`

**Definition:** States a confident general claim without offering meaningful support.

**Example 1:** “Super shoes have ruined amateur racing. Times mean nothing now and everyone knows it.”

**Example 2:** “Heart rate training is a fad that sells watches. Run by feel like people did for eighty years.”

### `reaction`

**Definition:** Reports an immediate personal response to something that just happened, emphasizing feeling rather than an argument.

**Example 1:** “Just watched someone in a full costume run past me at mile 20 and I've never felt worse about myself.”

**Example 2:** “That last kilometre was the best I've felt in a race in years. Still buzzing.”

### The hardest boundary

**Which two labels:** `hot_take` and `reaction`.

**The decision rule I used every time:** If the post mainly reports the writer's own just-happened experience and feeling, label it `reaction`; if it makes a general claim about runners or training, label it `hot_take`, even when the writer sounds emotional.

### Boundary stress test

These eight synthetic posts were written to probe the label edges. Applying the definitions gives a single label to each:

| Synthetic boundary post | Label | Rule applied |
|---|---|---|
| “My last four long runs averaged 18 km, and the two with a faster finish had lower heart-rate drift. That supports adding a controlled finish.” | `analysis` | Checkable comparison supports the recommendation. |
| “Long runs should always finish fast. Easy finishes are a waste of the day.” | `hot_take` | General confident claim without evidence. |
| “Just finished my first 18 km and I'm still buzzing.” | `reaction` | Immediate personal experience and feeling. |
| “That was the best long run of the year.” | `hot_take` | Broad evaluation, not a just-happened personal feeling report. |
| “I ran 18 km this morning; my average pace was 5:40/km, so I should stop calling every run slow.” | `analysis` | The pace is checkable and supports the point. |
| “I ran 18 km this morning and I'm exhausted, but weirdly proud.” | `reaction` | Personal immediate feeling is the focus. |
| “Carbon shoes are overrated; people would be faster if they trained properly.” | `hot_take` | Unsupported general assertion. |
| “I tried the plated pair today and felt clumsy the whole way home.” | `reaction` | First-person immediate experience, with no general claim. |

## The Dataset

**Where the posts came from:** The fictional running-forum practice dataset included with this repository, plus 140 synthetic examples written for this exercise. These are not collected public posts, so this dataset does not meet the assignment's public-community collection requirement.

**How I labelled them:** The included 60 practice examples retain their supplied labels. The added examples were authored with the label definitions in mind; they were not cold-labeled or independently reviewed. No AI pre-labeling was used.

**Final counts per label after the one improvement:**

| Label | Count | Share |
|---|---:|---:|
| `analysis` | 87 | 39.5% |
| `hot_take` | 67 | 30.5% |
| `reaction` | 66 | 30.0% |
| **Total** | **220** | **100%** |

**Three hard cases**

1. *The post:* “My log has 8 recovery sessions at a 6% improvement. The slower days followed poor sleep, so I would fix recovery before changing the plan.” *Could have been:* `analysis` or `hot_take`. *I chose:* `analysis`, because the measured trend supports the recommendation. *(synthetic)*
2. *The post:* “Just finished race pacing and my legs are toast. I need a minute before I decide whether that was fun.” *Could have been:* `reaction` or `hot_take`. *I chose:* `reaction`, because it describes a personal, immediate experience. *(synthetic)*
3. *The post:* “Across 3 races, one fewer stop happened when I started conservatively. The pattern suggests pacing matters more than the course profile.” *Could have been:* `analysis` or `hot_take`. *I chose:* `analysis`, because the comparison is offered as evidence for the conclusion. *(synthetic)*

## The Training Run

**Base model:** `distilbert-base-uncased`.

**Settings:** The notebook defaults are 3 epochs, learning rate `2e-5`, batch size 16, max length 128, and seed 42.

**Anything I changed from the defaults, and why:** None.

**Split sizes:** 139 train / 31 validation / 30 test using seed 42. Test counts were 10 `analysis`, 10 `hot_take`, and 10 `reaction`.

**Results before the improvement:** CPU (`torch 2.15.0`), test accuracy **0.900**, macro F1 **0.898**. Per-label F1: `analysis` **0.842**, `hot_take` **0.952**, `reaction` **0.900**. Confidence accuracy for the most confident third was **1.000** and the least confident third was **0.700**. These scores are on the original 200-row dataset; the three-seed logs below give the repeated results. The examples are synthetic and highly patterned, so the high scores are a practice result rather than evidence about public posts.

## How I Used AI

**Moment 1**

- *What I asked for:* To turn the assignment prompt into a concrete project checklist.
- *What came back:* A reminder to use the fictional running-forum theme already present in the repository.
- *What I changed:* I kept the result explicitly marked as synthetic instead of claiming the examples came from a public community.

**Moment 2**

- *What I asked for:* Help generating additional examples and boundary cases related to the sample taxonomy, then applying the staff taxonomy to the staff posts.
- *What came back:* Synthetic running discussion posts in the three label styles, boundary cases, and a proposed label for each of the 30 staff posts.
- *What I changed:* I recorded generated examples as synthetic and kept the staff labels in `my_staff_labels.csv`; I did not invent staff answers or an agreement score.

**Pre-labelling disclosure:** No AI pre-labeling of collected posts was used. The original 60 rows retain the labels provided with the practice dataset; 160 additional rows are synthetic and were generated with labels for this exercise. No human cold pass over 20 public posts was performed, so that part of the course workflow remains unmet.

## Unit 6 — Baseline and diagnosis

**Prediction, written before the baseline run:** I expect the fine-tuned model to beat the zero-shot baseline overall, especially on `reaction`, because it has seen examples that distinguish immediate first-person feeling from general claims.

### Baseline vs. trained model

Both columns below use the same 30 posts in `test_split.csv`. The trained column is the seed 42 result from the 200-row dataset.

| Measure | Baseline | Trained | Difference |
|---|---:|---:|---:|
| Overall accuracy | 0.700 | 0.900 | +0.200 |
| Macro F1 | 0.667 | 0.898 | +0.232 |
| F1 — `analysis` | 0.818 | 0.842 | +0.024 |
| F1 — `hot_take` | 0.720 | 0.952 | +0.232 |
| F1 — `reaction` | 0.462 | 0.900 | +0.438 |

The definitions baseline scored **0.700** accuracy; using bare label names scored **0.567**, a **0.133** drop. The definitions helped the zero-shot model on this split.

### Three-seed results before the change

The data had 200 rows (67 `analysis`, 67 `hot_take`, 66 `reaction`). All runs used CPU, the same model and settings, and seeds 42, 7, and 2024.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---:|---:|---:|---|
| 1. Overall accuracy | ≥ 0.60 | 0.900 | 1.000 | 1.000 | MET |
| 2. Every label F1 | ≥ 0.50 | min 0.842 | min 1.000 | min 1.000 | MET |
| 3. Largest label share | ≤ 40% | 33.5% | 33.5% | 33.5% | MET |
| 4. Blind second-pass agreement | ≥ 80% | — | — | — | MISSED — not measured |
| 5. Accuracy in most-confident third | ≥ 0.70 | 1.000 | 1.000 | 1.000 | MET |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---:|---:|---:|---:|
| Overall accuracy | 0.900 | 1.000 | 1.000 | 0.100 |
| Macro F1 | 0.898 | 1.000 | 1.000 | 0.102 |
| F1 — `analysis` | 0.842 | 1.000 | 1.000 | 0.158 |
| F1 — `hot_take` | 0.952 | 1.000 | 1.000 | 0.048 |
| F1 — `reaction` | 0.900 | 1.000 | 1.000 | 0.100 |

Seed 42 confusion matrix (rows are the true labels):

| true \\ predicted | analysis | hot_take | reaction |
|---|---:|---:|---:|
| **analysis** | 8 | 1 | 1 |
| **hot_take** | 0 | 10 | 0 |
| **reaction** | 1 | 0 | 9 |

There was one `analysis` post predicted as `hot_take` and none in the reverse direction; with only 10 test posts per label, this is too little evidence to diagnose a strong directional bias. Accuracy moved by 0.100 across seeds, consistent with a small test split making the estimate jumpy.

### Staff-taxonomy check

`my_staff_labels.csv` contains 30 exact-text, AI-generated draft labels under the supplied staff definitions; they should be reviewed before submission. `data/staff_labels.csv` is not present, so `agreement.py` and adjudication could not be completed. I did not create a substitute answer key or fabricate an agreement rate.

### One change and three-seed results after

The weakest and most variable before-run label was `analysis` (F1 0.842–1.000). I added **20 varied synthetic `analysis` examples**—20 rather than 30 so the class stayed below my 40% balance target. This was the only dataset change; the model settings stayed the same. The after dataset has 220 rows, with the largest label at 39.5%.

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---:|---:|---:|---|
| 1. Overall accuracy | ≥ 0.60 | 0.970 | 0.970 | 1.000 | MET |
| 2. Every label F1 | ≥ 0.50 | min 0.947 | min 0.947 | min 1.000 | MET |
| 3. Largest label share | ≤ 40% | 39.5% | 39.5% | 39.5% | MET |
| 4. Blind second-pass agreement | ≥ 80% | — | — | — | MISSED — not measured |
| 5. Accuracy in most-confident third | ≥ 0.70 | 1.000 | 1.000 | 1.000 | MET |

| Measure | Seed 42 | Seed 7 | Seed 2024 | Spread |
|---|---:|---:|---:|---:|
| Overall accuracy | 0.970 | 0.970 | 1.000 | 0.030 |
| Macro F1 | 0.967 | 0.970 | 1.000 | 0.033 |
| F1 — `analysis` | 1.000 | 0.963 | 1.000 | 0.037 |
| F1 — `hot_take` | 0.947 | 0.947 | 1.000 | 0.053 |
| F1 — `reaction` | 0.952 | 1.000 | 1.000 | 0.048 |

The seed 42 after-run matrix was:

| true \\ predicted | analysis | hot_take | reaction |
|---|---:|---:|---:|
| **analysis** | 13 | 0 | 0 |
| **hot_take** | 0 | 9 | 1 |
| **reaction** | 0 | 0 | 10 |

The added examples improved the measured `analysis` F1 floor from 0.842 to 0.963 and narrowed accuracy spread from 0.100 to 0.030. The split changed with the dataset, so this is encouraging evidence on the synthetic corpus, not a controlled claim about public posts.

### What's still broken

- **Blind agreement criterion:** Not measured. A separate 20-post blind second pass is still needed; no independent cold pass was done for the synthetic rows.
- **Public source requirement:** The dataset is fictional/synthetic, by design. To satisfy the course requirement, replace it with at least 200 posts from a public community, cold-label 20, and repeat the evaluation.
- **Staff agreement report:** Waiting for `data/staff_labels.csv`; after receiving it, run `python agreement.py` and write a reasoned call for every disagreement.

The labels were meant to capture whether a post argues from evidence, asserts a general view without support, or reports an immediate personal feeling. The model scored highly on the generated examples, but their repeated wording gives it shortcuts that may not transfer to real forum posts; the low zero-shot `reaction` F1 and the before-run errors also show that separating feeling from a general claim remains harder than recognizing template-like arguments.
