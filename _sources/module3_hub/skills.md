# Module 3 Skills Sheet — What You Can Do Now

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

This is the module that changes what you can claim on a résumé. Before it, you could build a model. After it, you have **deployed a scheduled data pipeline in the cloud** and can point at it running. Go down the list and check them off.

---

## ☁️ Deploying on Google Cloud Platform
*Videos 1-4 · Repo: `myscrapers` · Assignment A05*

- ☐ Import a starter repo into your own account and rename it for your NetID
- ☐ Create a GCP project, attach billing, and find your **free credits** (including the Vertex AI grant)
- ☐ Run the Cloud Shell setup block after changing **only three values**: project ID, bucket name, GitHub repo
- ☐ Say what the setup actually created: **enabled APIs**, three **service accounts** (runtime, deployer, scheduler), and a **workload identity pool**
- ☐ Explain in one sentence why the identity pool exists (so GitHub and GCP can mint tokens for each other without you pasting a key anywhere)
- ☐ Add the six required **repo variables** in Settings → Secrets and variables → Actions → Variables
- ☐ Deploy five cloud functions from **GitHub Actions** and read the run status
- ☐ **Debug a red X**: open the failing run, read the error, apply the fix, re-run the workflow
- ☐ Escape a hung Cloud Shell command with **Ctrl+C** and re-run it without panicking
- ☐ Use **Test in Cloud Shell** on a Cloud Run service and confirm output landing in your bucket
- ☐ Navigate the bucket layout: `scrapes/` (raw) → `structured/` (parsed) → `datasets/` → `preds/`
- ☐ Read the **cron schedule** out of the deploy YAML and say what runs at :00, :10, :12, :15, :20, :45
- ☐ Tear it all down and redeploy from scratch as v2 — *and get faster each time*

## 🔤 ETL with regular expressions
*Videos 5-9 · Function: `extractor_per_listing` · Assignment A06*

- ☐ Define **ETL** and identify which cloud function does the T
- ☐ Read a regex pattern aloud in plain English, piece by piece
- ☐ Write a pattern with a **capture group**, a **word boundary**, and optional whitespace
- ☐ Explain why the price pattern tolerates a stray space after the `$` (real people type real ads)
- ☐ Post-process a match into a modelable value: strip commas, cast to `int`
- ☐ Use the **bronze / silver / gold** vocabulary correctly, and say why regex output is silver
- ☐ Build a **fallback chain** — try keyword form, then `80k miles` form (**× 1,000**), then plain `X miles` — with a `None` default
- ☐ Predict where a pattern will fail *before* running it (`'18 Jeep Wrangler`, an all-caps ad, `GREAT DEAL` as the make)
- ☐ **Grade your own extraction honestly** — open the CSV and say "the year column is excellent, make/model is about 50%"
- ☐ Add a new field to the extractor and name every downstream place that must change
- ☐ Deploy a change **safely**: clone the function to `_v2`, give it its own YAML and schedule, and leave the working version running
- ☐ Argue for regex over a model where it wins: transparent, rules-based, and explainable to anyone

## 🤖 GenAI for ETL
*Videos 11-19 · Function: `extractor_llm_poc` · Assignment A07*

- ☐ Enable **Vertex AI** and raise the function timeout for LLM calls
- ☐ Define a **response schema** (field names + data types) and explain why it's the thing that makes output usable
- ☐ Set **`temperature=0`** and say what it's for
- ☐ Decide **required vs. optional** fields deliberately, and explain the flattery failure mode — a model will invent a value to satisfy you
- ☐ Use **`nullable=true`** so "not present" is representable
- ☐ Write **retry logic** for resource-exhausted / server / timeout errors so one flaky call doesn't kill an hourly job
- ☐ Stamp every record with **provenance**: LLM provider, model name, run timestamp — and say why (audit, debugging, and re-running the archive on a better model later)
- ☐ Write your own post-processing (ints, stripped strings) instead of trusting the model's formatting
- ☐ Add a field end to end and hit **all four places**: schema, required list, **prompt instructions**, and the final record assembly
- ☐ Diagnose the specific failure where a required schema field errors because **the prompt never mentioned it**
- ☐ Build `materialize_llm` by copying `materialize_master`, changing the folder prefix, adding the column, and writing a new YAML
- ☐ Avoid the **name-collision bug**: a new YAML that reuses the old cloud-function name silently overwrites a working deployment
- ☐ Compare RegEx vs. LLM output on the **same listing** and defend which you'd ship
- ☐ Review your own change as a **diff** before deploying

## 📈 Operational modeling and delivery
*Videos 10, 20 · Assignment A08 (midterm)*

- ☐ Read the hourly training function: `scraped_at` → datetime, **everything before today trains, today is the holdout**
- ☐ Explain the pipeline inside it: imputer → one-hot encoder → decision tree
- ☐ Design the experiment worth running: **a model frozen on four days vs. one retrained on everything**, scored on tomorrow
- ☐ Trend your own model over weeks — error down, R² up, the actual-vs-predicted fan tightening
- ☐ Sync results from a private bucket to a **public GitHub repo** with a GitHub-native Action
- ☐ Keep credentials out with a **`.gitignore`**, and know why that isn't optional
- ☐ Sync more than a CSV — PNGs, and the **numeric partial-dependence values** so you can trend the *shape* of a PDP
- ☐ Accept that a cron job fires late and judge whether that's acceptable for your use case
- ☐ Explain why publicly posted predictions are gameable, and how **miners vs. validators** in decentralized AI addresses it

---

## The one-sentence version

**You can take a starter repo, deploy it as five scheduled cloud functions, turn scraped raw text into structured data two different ways, retrain a model every hour, and publish the results where anyone can check them.**

Most people with a data science degree have never done this. You will have done it in three weeks.
