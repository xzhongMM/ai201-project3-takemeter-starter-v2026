# TakeMeter — fictional running-forum exercise

> **Dataset disclosure:** This is a practice build from the repository's fictional running forum. The first 60 rows in `labels.csv` come from the provided fictional practice set; the other 140 are synthetic examples authored for this exercise. None are represented as public community posts. Use `data/practice_labels.csv` if you want to run the supplied examples alone.

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

**Counts per label:**

| Label | Count | Share |
|---|---:|---:|
| `analysis` | 67 | 33.5% |
| `hot_take` | 67 | 33.5% |
| `reaction` | 66 | 33.0% |
| **Total** | **200** | **100%** |

**Three hard cases**

1. *The post:* “My log has 8 recovery sessions at a 6% improvement. The slower days followed poor sleep, so I would fix recovery before changing the plan.” *Could have been:* `analysis` or `hot_take`. *I chose:* `analysis`, because the measured trend supports the recommendation. *(synthetic)*
2. *The post:* “Just finished race pacing and my legs are toast. I need a minute before I decide whether that was fun.” *Could have been:* `reaction` or `hot_take`. *I chose:* `reaction`, because it describes a personal, immediate experience. *(synthetic)*
3. *The post:* “Across 3 races, one fewer stop happened when I started conservatively. The pattern suggests pacing matters more than the course profile.” *Could have been:* `analysis` or `hot_take`. *I chose:* `analysis`, because the comparison is offered as evidence for the conclusion. *(synthetic)*

## The Training Run

**Base model:** `distilbert-base-uncased`.

**Settings:** The notebook defaults are 3 epochs, learning rate `2e-5`, batch size 16, max length 128, and seed 42.

**Anything I changed from the defaults, and why:** None.

**Split sizes:** 139 train / 31 validation / 30 test using seed 42. Test counts were 10 `analysis`, 10 `hot_take`, and 10 `reaction`.

**Results:** CPU (`torch 2.15.0`), test accuracy **0.900**, macro F1 **0.898**. Per-label F1: `analysis` **0.842**, `hot_take` **0.952**, `reaction` **0.900**. Confidence accuracy for the most confident third was **1.000** and the least confident third was **0.700**. The criteria based on accuracy and per-label F1 were met on this one split. Treat these as a practice result only: the examples are synthetic and highly patterned, and no three-seed robustness run was done.

## How I Used AI

**Moment 1**

- *What I asked for:* To turn the assignment prompt into a concrete project checklist.
- *What came back:* A reminder to use the fictional running-forum theme already present in the repository.
- *What I changed:* I kept the result explicitly marked as synthetic instead of claiming the examples came from a public community.

**Moment 2**

- *What I asked for:* Help generating additional examples and boundary cases related to the sample taxonomy.
- *What came back:* Synthetic running discussion posts in the three label styles, plus eight examples near the label boundaries.
- *What I changed:* I included the generated posts only as synthetic examples and recorded their origin in the CSV note column. Their label was assigned as part of authoring, not by pre-labeling real collected posts.

**Pre-labelling disclosure:** No AI pre-labeling of collected posts was used. The original 60 rows retain the labels provided with the practice dataset; the 140 additional rows are synthetic and were generated with labels for this exercise. No human cold pass over 20 public posts was performed, so that part of the course workflow remains unmet.
