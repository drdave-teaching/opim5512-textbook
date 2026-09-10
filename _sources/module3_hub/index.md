# Module 3 Hub — Webscraping & Operational ML (Fall 2026)

:::{admonition} ⚠️ Work in progress
:class: warning
This book is under active development for Fall 2026 - pages may change as the course evolves.
:::

This is **the production module**. Your code stops living on your laptop and starts running in the cloud on a schedule, whether you're awake or not.

> **Clone the starter repo, deploy five cloud functions, scrape real listings every hour, extract structured data twice — old-school RegEx and new-school Gemini — retrain a model, and publish the results.**

## The pieces

| What | Where | Why |
|---|---|---|
| 🎥 **The 20 videos** (~2h10m) | HuskyCT (Module 3) · [what each one covers](videos.md) | *do-as-I-do* — watch with a second screen |
| ✅ **Skills sheet** | [what you can do now](skills.md) | check yourself off after the videos |
| 📓 **The chapter** | [Chapter 3](../m3_operational_ml/index.md) | the narrative version |
| 🗂️ **The starter repo** | `myscrapers` (import it, don't fork it) | everything you deploy comes from here |

## The four blocks

**Block 1 · Getting deployed on GCP** (videos 1-4) — import the repo, create the project, run the setup block, add six repo variables, and deploy from GitHub Actions until you have **five green check marks**. Then test each function and watch your bucket fill with real data.

**Block 2 · Old-school ETL with RegEx** (videos 5-10) — turn raw ad text into structured fields with regular expressions: transparent, rules-based, explainable to anyone, and about **50% accurate on make/model** — which is the assignment.

**Block 3 · GenAI for ETL** (videos 11-19) — do the same job with Gemini on Vertex AI, constrained by a **schema** so the output is consistent enough to concatenate and model. Includes an unedited debugging session that ends unresolved, and the video where the fix finally lands.

**Block 4 · Getting your results out** (video 20) — sync predictions from a private bucket to a public GitHub repo so your work can be seen and graded.

## The pipeline, on the clock

Everything is scheduled by cron in the deploy YAMLs. Know this table:

| Time | What runs |
|---|---|
| **:00** | scrape listings |
| **:10** | RegEx extractor → `jsonl` |
| **:12** | LLM extractor → `json_llm` |
| **:15** | materialize → one CSV |
| **:20** | train the model, write `preds/` |
| **:45** | sync results to GitHub |

That chain is why adding one field is never a one-file change.

## The assignments

- **A05 — deploy it.** Get the five green check marks and prove the scraper is filling your bucket.
- **A06 — improve the RegEx ETL.** Add fields, fix `make`/`model`, and deploy your change **safely** (clone the function to `_v2`; don't break the running one).
- **A07 — customize the LLM extractor.** Add a field to the Gemini call and get it all the way through to the materialized CSV. Remember there are **four places** a new field has to appear.
- **A08 — the midterm.** Extend the pipeline, tune the model with Module 2's tools, sync **predictions plus permutation importance plus your top-3 PDPs**, and build a model-trending notebook.

:::{admonition} The hardest part is the permissions
:class: tip
Dave says it outright, and he means it. The setup block is long because service accounts and the workload identity pool have to be exactly right. Go one line at a time, and when something turns red, read the error — video 3 shows him doing exactly that and fixing it live.
:::
