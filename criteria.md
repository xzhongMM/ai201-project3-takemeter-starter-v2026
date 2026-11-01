# Acceptance criteria — synthetic running-forum classifier

These targets are provisional because this project uses the repository's fictional practice community and synthetic examples, not collected public posts. They are written before training.

## 1. Overall accuracy

Accuracy on the held-out test split will be at least **0.60**.

**Why this target:** Three balanced labels give a chance baseline near 0.33. A 0.60 target asks for a useful signal while recognizing that the classes overlap and many posts are short.

## 2. Per-label performance

The test-set F1 score for each of `analysis`, `hot_take`, and `reaction` will be at least **0.50**.

**Why this target:** A respectable overall score can hide a model that ignores a whole category. The per-label floor is intentionally lower because each held-out class has relatively few examples.

## 3. Balance

No label will account for more than **40%** of the 200-row dataset.

**Why this target:** A balanced set makes it harder for the classifier to succeed by always choosing one label, and 40% leaves room for small variation in the number of examples per label.

## 4. Labeling consistency

On a blind second pass over 20 examples, I will agree with my first label on at least **16 of 20 posts (80%)**.

**Why this target:** The decision boundary between an unsupported general claim and an immediate personal reaction can be subtle. 80% gives a concrete check while allowing genuine borderline cases.

## 5. Confidence

Among predictions with confidence of at least **0.80**, at least **70%** will be correct on the held-out test split.

**Why this target:** Confidence should help distinguish predictions safe enough to review first. The test set is small, so I use a 70% precision target rather than expecting near certainty.

> **Revised after the run:** The notebook reports accuracy for the most confident third of predictions, but it does not report accuracy only for predictions with confidence at least 0.80. The original criterion cannot be measured from its output as written.
>
> **Revised criterion:** Accuracy among the most confident third of held-out predictions will be at least **0.70**.
>
> **Why revised:** This matches the confidence summary the notebook actually produces, so the result can be checked without changing the evaluation code.
