# Module 2 Hub — Tuning Models & Explainable AI (Fall 2026)

:::{admonition} ⚠️ Work in progress
:class: warning
This book is under active development for Fall 2026 - pages may change as the course evolves.
:::

Everything for Module 2 in one place. Module 1 taught you to fit a model. Module 2 teaches you to fit one you can **defend**. One arc runs through all of it:

> **Resample the training partition only. Report a distribution, not a lucky number. And be able to say which features mattered — and which way they pushed.**

## The pieces

| What | Where | Why |
|---|---|---|
| 🎥 **The 19 videos** (~2h05m) | HuskyCT (Module 2) · [what each one covers](videos.md) | watch in order, run the notebook alongside |
| ✅ **Skills sheet** | [what you can do now](skills.md) | check yourself off after the videos |
| 📓 **The notebooks** | [Chapter 2](../m2_tuning_xai/index.md) | every video drives one of these |
| ✍️ **By-hand checks** | [opim-math worksheets](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512) | confusion matrix and metrics by hand |

## The three blocks

**Block 1 · Imbalanced data and cross-validation** (videos 1-8) — why a 90%-accurate model can have zero skill, three different fixes (undersample, oversample, SMOTE/SMOTENC), and the evaluation strategies that give you a *distribution* of error metrics instead of one number.

**Block 2 · Spot-checking and tuning** (videos 9-14) — run a horse race between cheap models, wrap preprocessing and model together in a `Pipeline`, then hand the winner to `GridSearchCV`. Learn to count how many models you're about to fit *before* you hit run.

**Block 3 · Explainable AI** (videos 15-19) — permutation importance (which features matter), partial dependence (which way they push), and autoML with TPOT.

## The two assignments

- **A03 — imbalanced data and cross-validation.** Resample a genuinely imbalanced dataset, keep your test partition honest, and report error metrics as a **mean, standard deviation, and confidence interval** across repeats. A single accuracy number is not an answer here.
- **A04 — tuning and explainability.** Tune a model properly with cross-validation, then explain it: repeated permutation importance and partial dependence plots, read out loud in plain English.

:::{admonition} The single most common way students invalidate their own results
:class: warning
Applying SMOTE (or any resampling) **before** the train/test split. Copies of the same row end up on both sides, your test score is inflated, and the model falls apart in production. Split first. Always.
:::

## Where this goes next

Module 3 puts all of this in production: the tuned model, the importance plot, and the PDP become artifacts that a scheduled cloud job regenerates every hour — and the A08 midterm asks you to **trend** them over time.
