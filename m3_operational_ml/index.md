# Chapter 3 — Webscraping & Operational ML

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
