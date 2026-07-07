# Chapter 3 — Webscraping & Operational ML

:::{admonition} 🔗 Notebooks for this chapter
:class: seealso dropdown
Open in Colab and **Runtime → Run all** — data loads from a stable link, nothing to upload.

- **FINAL GCP Guide Dec8 2025** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/FINAL_GCP_Guide_Dec8_2025.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/FINAL_GCP_Guide_Dec8_2025.ipynb)
- **M3 2 Beautiful Soup Model Updates** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/M3_2_BeautifulSoup_ModelUpdates.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/M3_2_BeautifulSoup_ModelUpdates.ipynb)
- **M3 3 Gen AI ETL** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/M3_3_GenAI_ETL.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/M3_3_GenAI_ETL.ipynb)
- **My1st Agent Write LPs** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/My1stAgent_WriteLPs.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/My1stAgent_WriteLPs.ipynb)
- **My2nd Agent Brute Force** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/My2ndAgent_BruteForce.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/My2ndAgent_BruteForce.ipynb)
- **Scraping Craigs List For Cars** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/ScrapingCraigsList_ForCars.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module3/ScrapingCraigsList_ForCars.ipynb)
:::


This is the chapter that turns "a model in a notebook" into something that **looks and behaves like a production system** — automated, cloud-based, and continuously updating. You'll collect data at scale, transform messy raw text into clean features (two ways — old-school and AI), train a model that **runs on a schedule in the cloud**, and sync its outputs back to GitHub. By the end you'll have built an end-to-end pipeline, not a proof-of-concept.

The running project: scrape used-car listings, extract structured fields (make, model, year, mileage, price), and predict price — a deliberately "numeric-ish boring" dataset, because boring is what production usually looks like.

## 3.1 Deploying to the cloud with GitHub Actions

Your code shouldn't run only on your laptop. **GitHub Actions** is a CI/CD service that runs your code automatically on **Google Cloud Platform** whenever you push — no manual intervention, fully reproducible. The course scaffold (the `myscrapers` repo) deploys a set of **Cloud Functions** and the milestone is getting **five green check marks**: five workflows that authenticate to GCP and deploy successfully.

```{admonition} The deployment gotchas everyone hits
:class: warning
Two failures account for most lost hours: **(1)** a typo in a GitHub repository **variable** (e.g. `WORKLOAD_IDENTITY_PROVIDE` missing the trailing `R`) breaks authentication — double-check every variable name; **(2)** a strict bash setting (`set -euo pipefail`) makes the Cloud Shell **close instantly** when any command fails, so a small typo nukes your session. Build slowly, verify each step (`gcloud functions list`, `gcloud scheduler jobs list`), and confirm your functions are **ACTIVE** before moving on.
```

Once deployed, **Cloud Scheduler** triggers the functions on a **cron schedule** (e.g. hourly), so the pipeline keeps running and the dataset keeps growing while you sleep. That's the difference between a script and a *system*.

## 3.2 ETL with regular expressions

The scraper dumps **unstructured raw text** — a car listing is free-form prose. **ETL** (extract, transform, load) turns it into a structured row, and the old-school tool is the **regular expression (regex)**: a pattern that finds and captures substrings. You write one pattern per field — price, year, make/model, mileage:

```python
import re
PRICE_RE = re.compile(r'\$\s*([0-9][0-9,]+)')          # $12,500 -> 12500
YEAR_RE  = re.compile(r'\b(19|20)\d{2}\b')              # a 4-digit year
# parse a listing into a dict of fields
def parse_listing(text):
    price = PRICE_RE.search(text)
    return {'price': int(price.group(1).replace(',', '')) if price else None,
            'year':  int(YEAR_RE.search(text).group(0)) if YEAR_RE.search(text) else None}
```

Regex is **fast and deterministic** — perfect for well-structured fields like price, mileage, and year. Its weakness is messy, inconsistent wording: "AWD," "all wheel drive," "4WD," and "4x4" all mean the same thing, and a regex has to enumerate every variant. That's exactly where the next tool shines.

## 3.3 ETL with generative AI

Modern ETL hands the messy text to a **large language model**. The course uses **Gemini (Vertex AI)** with a defined **schema**: you tell the model exactly which fields to return (and *only* those), and it reads the listing the way a human would — inferring `transmission`, `color`, `condition`, `city`, even when the wording is inconsistent. You deploy a separate `materialize-llm` function so the GenAI pipeline runs **alongside** the regex one, and you compare them.

```{admonition} RegEx vs. GenAI — use both
:class: tip
- **RegEx** wins on **simple, predictable patterns** (price, year, mileage): fast, cheap, deterministic, no API call.
- **GenAI** wins on **vague, description-heavy text** (condition, free-text features, normalizing "AWD/4x4"): it uses context and returns far fewer missing values — at the cost of speed and an API bill.
The professional move is a **hybrid**: regex the easy fields, LLM the hard ones. **Prompt engineering** — tightening the schema and instructions so the model returns clean, consistent JSON — is the new feature-engineering.
```

## 3.4 Closing the loop

Finally, the model fits on a schedule in the cloud and its outputs — predictions, plots, a growing CSV — get **synced back to GitHub** so the results are visible to the world. The capstone is a **model-trending** notebook: clone the repo, read the accumulated results, and chart how the model's **error and interpretability change over time** as new data arrives. Companies want to know you can *productionize and trend* a model — and now you can.

```{admonition} A note on what's current
:class: note
Cloud platforms, scraping targets, and the GCP→GitHub sync step evolve fast — and some get blocked or deprecated. The **architecture** in this chapter is the durable lesson (scrape → ETL → schedule → sync → trend); expect to swap individual components (the scraper, the sync mechanism) as the tooling changes.
```

## Wrap-up

```{admonition} Key takeaways
:class: tip
- **GitHub Actions** deploys code to **GCP** automatically; **Cloud Scheduler** runs it on a cron so the pipeline self-updates.
- **ETL** turns scraped raw text into structured rows; **regex** is fast/deterministic for clean fields.
- **GenAI (Gemini) ETL** with a tight **schema + prompt engineering** handles messy, description-heavy fields — use a **hybrid** of both.
- Close the loop: **schedule** the model, **sync** outputs to GitHub, and **trend** error/interpretability over time — that's a production system.
```


---

## 📌 Lecture key points

*Distilled takeaways from the video lectures behind this chapter — click each to expand.*


:::{admonition} GCP Pt 1 — Overview
:class: note dropdown
- Goal: deploy code to **Google Cloud Platform** so it runs without your laptop.
- **GitHub Actions** drives deployment automatically on push.
- Anchored on the **`myscrapers`** starter repo.
- The milestone is **five green check marks** (five workflows deploy).
- Sets up an automated, cloud-based pipeline.
:::

:::{admonition} GCP Pt 2 — Setting up GCP for our project
:class: note dropdown
- Configure GCP project, service accounts, and **GitHub Actions variables**.
- Variables must be set for **YOUR** repo, not Dave's.
- Stay close to the guide; permissions are exacting.
- Authenticate via workload identity.
- Foundation for deployment.
:::

:::{admonition} GCP Pt 3 — Green check marks!
:class: note dropdown
- Success = **5 green checks** in GitHub Actions (workflows authenticate + deploy).
- Common bug: a **typo in a variable name** (e.g., missing `R` in `WORKLOAD_IDENTITY_PROVIDER`).
- Verify with `gcloud functions list` (functions ACTIVE).
- Share successes/failures on the discussion board.
- Proof your code is live on GCP.
:::

:::{admonition} GCP Pt 4 — Testing your functions
:class: note dropdown
- **Cloud Scheduler** triggers functions on an **hourly cron**.
- Test endpoints with `curl`; confirm `"status":"ok"`.
- Strict bash (`set -euo pipefail`) closes the shell on any error — build carefully.
- Functions: scraper, extractor-per-listing, extractor-llm-poc, materialize, train.
- The pipeline self-updates while you sleep.
:::

:::{admonition} Intro to ETL with RegEx
:class: note dropdown
- **ETL** = turn scraped **unstructured text** into structured rows.
- **Regular expressions** find and capture substrings.
- Extract make/model/year/mileage/price from car listings.
- Fast and **deterministic**.
- Foundation for the RegEx ETL videos.
:::

:::{admonition} RegEx for PRICE_RE
:class: note dropdown
- Build a regex to capture **price** (\$12,500 → 12500).
- Patterns: `^`, `\s*` (flexible spaces), capture groups, optional separators.
- `re.I` (case-insensitive), `re.M` (multiline).
- Strip commas, cast to int.
- Store the field in the parsed dict.
:::

:::{admonition} YEAR_RE and MAKE_MODEL_RE
:class: note dropdown
- Capture **year** (4-digit) and **make/model**.
- Exclude false matches (e.g., "Contact Information" ≠ make/model).
- Anchor patterns to where the field appears.
- Consistent dictionary keys feed downstream materialization.
- Regex shines on predictable fields.
:::

:::{admonition} Three flavors of mileage
:class: note dropdown
- Mileage appears in **multiple formats** — write patterns for each.
- Normalize variants to one clean field.
- Demonstrates regex flexibility and its limits.
- Test against real messy listings.
- Capture-and-normalize pattern.
:::

:::{admonition} How to improve the ETL RegEx starter code
:class: note dropdown
- Customize the starter `parse_listings()` with **new fields** (color, condition, transmission, fuel).
- Deploy a **versioned `materialize-v2`** so the schema can evolve safely.
- Update `.yml` workflows + output directories so new fields flow through.
- Don't break the running pipeline as you extend it.
- Document what you built and why (A06).
:::

:::{admonition} Push yourself — customize outputs / review the DT model
:class: note dropdown
- The pipeline fits a **decision-tree** model on a schedule.
- Customize outputs (predictions, plots, CSV).
- Review the model and iterate.
- Connect ETL → modeling → outputs.
- Operational ML end-to-end.
:::

:::{admonition} Intro to GenAI (Vertex) and GCP
:class: note dropdown
- Use **Gemini (Vertex AI)** to extract features from messy text.
- Define a **schema** so the model returns **only** required fields.
- LLM reads context like a human (infers transmission/color/condition).
- Deploy alongside the regex pipeline.
- The modern ETL approach.
:::

:::{admonition} Overview / Interesting aspects of the extractor-llm-poc
:class: note dropdown
- Walk the **extractor-llm-poc** Cloud Function (LLM extraction proof-of-concept).
- Compares LLM JSON output to the raw text.
- Improves extraction when raw text is too messy for regex.
- Returns strict JSON via the response schema.
- The GenAI half of the hybrid pipeline.
:::

:::{admonition} Three cool ways to enhance/update the extractor-LLM-POC
:class: note dropdown
- Add features (color/city/state/zip) by **expanding the schema**.
- **Prompt engineering** = tighten instructions for cleaner, consistent JSON.
- Normalize messy variants ("AWD/4WD/4x4").
- GenAI returns **fewer missing values** than regex on vague fields.
- Iterate on prompts like feature engineering.
:::

:::{admonition} Updating the extractor-llm-poc to predict transmission / materialize to CSV
:class: note dropdown
- Add **transmission** to the schema; deploy a separate **`materialize-master-llm`**.
- Materialize JSONL → one **CSV** that keeps updating.
- Run LLM pipeline **alongside** the regex one and compare.
- Update functions + workflows together (or new fields won't appear).
- A07 deliverable.
:::

:::{admonition} Materialize the LLM jsonl data (add a jsonl column)
:class: note dropdown
- **Materialization** aggregates per-listing JSONL into a master CSV.
- Add a column for the jsonl/LLM fields.
- Scheduled materialize function in Cloud Run.
- Proof: CSV grows with new listings.
- Connects extraction to a usable dataset.
:::

:::{admonition} Diffs and wrapping up / Got stuck / Oops forgot transmission
:class: note dropdown
- **Diffs**: review what changed between versions.
- **Got stuck / oops**: real debugging (forgot to wire transmission through end-to-end).
- Updating one function often requires updating downstream ones.
- Output directories must change or old listings keep empty new fields.
- Honest look at iterative pipeline development.
:::

:::{admonition} Sync-data with GitHub Actions
:class: note dropdown
- Push private **GCP/Cloud Storage outputs** to GitHub via a **GitHub Actions cron** (no GCP needed).
- Fires ~45+ min past the hour (2–15 min late) — "good enough" academically.
- Makes results shareable with the world.
- Feeds the **A08 model-trending** notebook.
- *(Status: this sync step is no longer taught — architecture is the durable lesson.)*
:::
