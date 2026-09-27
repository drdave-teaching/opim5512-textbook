# Module 5 Hub — Text Analytics Modeling (Fall 2026)

:::{admonition} ⚠️ Work in progress
:class: warning
This book is under active development for Fall 2026 - pages may change as the course evolves.
:::

Two complete toolkits for text, and the judgment to choose between them.

> **Turn words into columns you can explain — or hand the document to a pretrained model and get a summary, a label, or a 1×384 vector. Know which one your stakeholder needs.**

## The pieces

| What | Where | Why |
|---|---|---|
| 🎥 **The 12 videos** (~1h05m) | HuskyCT (Module 5) · [what each one covers](videos.md) | watch in order |
| ✅ **Skills sheet** | [what you can do now](skills.md) | check yourself off after the videos |
| 📓 **The chapter** | [Chapter 5](../m5_text/index.md) | the narrative version |

## The two blocks

**Block 1 · Classical text analytics** (videos 1-5) — NLTK preprocessing (lowercase → strip → stop words → tokenize → stem), then **bag of words** and **TF-IDF**, then a classifier you can crack open with permutation importance and name the exact words driving it.

**Block 2 · Pretrained transformers** (videos 6-12) — HuggingFace for summarization, zero-shot labeling, sentiment and emotion; then **embeddings**: every document becomes a fixed 1×384 vector, which you can cluster with k-means, auto-label with TF-IDF, and visualize with PCA and UMAP.

## The two datasets

| Dataset | Used for |
|---|---|
| **NOAA Storm Events 2019** | hail vs. flash flood classification from narrative text |
| **BillSum** (California legislation) | summarization, zero-shot, sentiment, embeddings, topic modeling |

## The assignment

- **A10 — Food Recalls.** Predict FDA recall severity (Class 1/2/3) from `Product Description` + `Reason for Recall`. This deliberately fuses Module 5 with **Module 2**: the classes are imbalanced, and the job is to **beat a majority-class baseline**. Prepare the data **two ways** (TF-IDF/BoW *and* padded text sequences), use a 90/10 split with `random_state=42` so everyone's numbers are comparable, and post your best F1 on the discussion board.
  - EDA is worth 20 points and wants **five varied plots** — *not five word clouds*. Dedupe product descriptions and handle the dirty rows.

:::{admonition} Why not just ask an LLM?
:class: tip
Sometimes you should. But Dave's argument for block 1 is worth being able to make yourself: an LLM is *"a little hands-off, and you don't necessarily understand how it operated."* With BoW/TF-IDF you can run permutation importance and tell a stakeholder exactly which words moved the prediction. Plenty of regulated industries require that.
:::

## Where this ends

Module 5 completes the set — Module 2 gave you honest evaluation and explanation, Module 3 put a model in production, Module 4 handled time, and Module 5 handles text. What's left is the final project.
