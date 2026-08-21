# Module 3 Video Guide — What Each Video Covers

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Twenty videos, about 2 hours 10 minutes total, in four blocks. This is **the production module** - the one where your code stops living on your laptop and starts running in the cloud on a schedule, whether you're awake or not. You'll deploy five cloud functions, scrape real Craigslist car ads every hour, turn that raw text into structured data **twice** (old-school RegEx and new-school Gemini), fit a model that retrains itself, and ship the results back to GitHub.

Everything is anchored on one starter repo: **`myscrapers`**. You clone it, make it yours, and by the end of block 1 you're looking at five green check marks.

```{admonition} Watch these with a second screen
:class: tip
Blocks 1 and 3 are *do-as-I-do* videos. Dave says it outright: "I'll do it live in case you want to follow along on another screen (which I hope you are)." Deploying alongside the video is the point.
```

---

## Block 1 · Getting deployed on GCP
**The big ideas:** cloud functions as small deployable units · GitHub Actions as the deployment mechanism · **the permissions are the hard part** · cron schedules · buckets, and the raw → structured folder layout · the five-function pipeline.

**Video 1 — GCP Pt. 1: Overview** *(7:39)*
The tour before the work, and the reasoning behind the whole module. Dave wanted an example with **unstructured text, web scraping, and analytics at scale** that students could actually run in the cloud — built over five weeks with research scholar **Rohit Akole** (MS BAPM alum, credited generously and repeatedly). Why GCP: *we were told it's the one that changes the least*.

You see where you're headed: a bucket where `scrapes/` holds raw text files (find the 2016 Mercedes, then find it again in the cleaned JSON) and `structured/` holds the parsed output, with five functions running under Cloud Run. The pipeline in one breath — **scrape Craigslist → extract with RegEx → extract again with an LLM → materialize into one CSV → train a model on the history and predict today's cars.** Practical tip up front: free credits are per-Google-account, so *if you've already used your free credits, just make a new Gmail address*.

**Video 2 — GCP Pt 2: Setting up GCP for our project** *(8:17)*
The setup, live and unedited. **Import** (not fork) `drdave-teaching/myscrapers` into your own repo, name it after your NetID, create the matching GCP project, and attach billing (you get $300 to start; Vertex AI adds $1,000 — Dave had used $32 in a week). Then the Cloud Shell block from the guide, where **you update exactly three things: project ID, bucket name, GitHub repo**, and paste the rest.

What's actually happening while it scrolls by: enabling APIs (Eventarc, storage, service usage, compute, resource manager), creating three **service accounts** (a runtime, a deployer, a scheduler), and building the **workload identity pool** that lets GitHub and GCP mint tokens for each other. Dave's honest framing: *there are so many permissions that need to be activated correctly; we're trying to make it as plug-and-play as possible.* Watch him hit a hang and escape with **Ctrl+C**, then re-run — that's the expected experience, not a failure.

**Video 3 — GCP Pt 3: Green check marks!** *(10:04)*
The GitHub side. Settings → Secrets and variables → Actions → **Variables**, and add six: `WORKLOAD_IDENTITY_PROVIDER`, `DEPLOYER_SA`, `RUNTIME_SA`, `PROJECT`, `REGION` (us-central, *I think because it's the cheapest*), and `BUCKET`. Then run the five workflows.

**And one comes back red.** The scraper fails with *permission denied*, Dave clicks "show more," finds the one line that must be run once as project owner, runs it, redeploys — **green check mark**. Do not skip this stretch. Watching someone read a deployment error and fix it calmly is most of what this module teaches; the five green checks are just the receipt.

**Video 4 — GCP Pt. 4: Testing your functions** *(9:16)*
Now prove each function works, using **Test in Cloud Shell** on each Cloud Run service, and watch the bucket fill in real time. The scraper finds 286 listings and dumps raw text (*oh my gosh, they're all coming in from Craigslist!*). The extractor turns 50 into 36 JSON files (14 already existed). Materialize collapses them into one CSV. The model trains.

Two things to notice. First, the extractor is **deliberately mediocre** — it gets price and mileage but reports the make as "contact" and the model as "information." *I left this crappy on purpose, because I want you to make it better.* That's your Module 3.2 assignment in one sentence. Second, even with bad data, **price versus mileage already slopes down** — the signal survives the mess.

His closing advice is worth taking literally: delete it all and do it again as v2, v3, until you can do it in 20 minutes.

---

## Block 2 · Old-school ETL with regular expressions
**The big ideas:** bronze → silver → gold data · a regex is transparent, rules-based, and explainable · capture groups · why real-world text defeats clean patterns · fallback chains · the blast radius of adding a field to a pipeline.

**Video 5 — Intro to ETL with RegEx** *(4:43)*
Where we are and what's next. Open a raw scrape — a Jeep Wrangler ad — and try to read it as a human: 111,000 miles, gray, four-wheel drive, and a price you have to **Ctrl+F for a dollar sign** to find. That manual act *is* the algorithm you're about to write. **ETL = Extract, Transform, Load**, and this week's focus is the `extractor_per_listing` function, where the same listing already comes out as structured JSON — correct price and mileage, and make/model reading "contact information."

**Video 6 — RegEx for PRICE_RE** *(5:23)*
The first pattern, built piece by piece: a literal `$`, then optional whitespace (*depending on how someone typed their Craigslist ad, they might hit a space by accident, and we don't want the code to fail just because of a space*), then a **capture group** of digits and commas, running until whitespace or a word. Then the post-processing that makes it modelable — strip the commas, cast to `int`, so `"$12,995"` becomes `12995`.

This is also where the **medallion vocabulary** lands: raw text is **bronze**, lightly-parsed output is **silver**. And the case for regex over a model: *it's not the most elegant solution, but it's very transparent and rules-based, so you can explain it to everybody.*

**Video 7 — YEAR_RE and MAKE_MODEL_RE** *(6:35)*
Two more patterns and an honest accounting of where they break. **Year**: a word boundary, `19` or `20`, two more digits, another boundary — which fails the moment someone writes `'18 Jeep Wrangler`, and might grab the posting date instead. **Make and model**: the first two capitalized words side by side — *and I think this one is hysterical*, because ads that open with `CONTACT INFORMATION` or `GREAT DEAL` in caps hand you exactly that as the car's make.

Then he opens `listings_master.csv` and grades his own work: year is excellent (there's a 1951 in there), price is decent, make/model is *about 50% data quality* — "buy here," "West Haven," "4350." And the reward at the end: price versus mileage on a **log axis**, sloping cleanly down.

**Video 8 — Three flavors of mileage** *(4:54)*
A fallback chain, which is the real lesson. Default to `None`, then try three patterns in increasing desperation: (1) a keyword search for **"mileage"/"odometer"** followed by an optional colon or dash, then digits; (2) the **"80k miles"** form — number, optional decimal, `k`, then `mi`/`mile`/`miles` — where you must **multiply by 1,000**; (3) plain `X miles`, written to catch both `1,500` and `150,000`. Each later attempt overwrites an earlier failure. *When I was building this, I kept getting bugs, so I kept adding logic to make things better* — that's how production parsers actually get written.

**Video 9 — How to improve the ETL RegEx starter code from Dave** *(8:16)*
Your enhancement brief. What's sitting in the ad and going unused: **condition, cylinders, drivetrain, fuel, title status, dealer-vs-private, Carfax, even the VIN** (fixed length, so easy to match). He walks through adding `condition` to the `D` dictionary and immediately hits the ambiguity — *"like new" is two words* — which is the honest state of this work.

Then the systems lesson, and it's the important one: **the pipeline is a chain on a clock.** Scrape at :00, RegEx extract at :10, LLM extract at :12, materialize at :15, model at :20 — all set by cron in the deploy YAMLs. So *if you start adding a bunch of fields to the regex extractor, it's going to break*, because `materialize_master` joins files and expects consistent fields. The professional move he recommends: **don't edit in place.** Clone the function to `extractor_per_listing_v2`, give it its own YAML at :11, deploy alongside, and keep the working version running while you noodle. Or clone the whole repo as a `-dev` project.

**Video 10 — Push yourself... customize the outputs... review of the DT model** *(7:07)*
The part of the pipeline nobody's looked at yet: the **hourly model**. It reads `listings_master`, converts `scraped_at` to a datetime, treats **everything before today as training and today as the holdout** — *as if your model were refreshed every day at midnight* — and runs a pipeline of imputer → one-hot encoder → decision tree to predict price from make, model, year, and mileage. Predictions land in `preds/` every hour.

Which sets up the assignment worth doing: as data accumulates, **trend your own model** — error down, R² up, the actual-versus-predicted fan tightening week over week. He also floats the experiment that a good student will run: **freeze a model on four days of data and race it against one retrained on everything**, then decide which you'd actually put in production. And a teaser — the `json_llm` files already appearing in your bucket, where the make really is a Dodge Charger.

---

## Block 3 · GenAI for ETL
**The big ideas:** schema-constrained extraction · prompt engineering as a *contract*, not a vibe · temperature 0 · required vs optional fields and the flattery problem · retries and provenance stamps · running old-school and new-school **in parallel** so you can compare them.

**Video 11 — Intro to GenAI (Vertex) and GCP** *(3:36)*
Turn on **Vertex AI**: run the last permission block in the guide, raise the function timeout (*large language model calls can take a bit*), then search "credits" in the console for the **$1,000 Gen AI App Builder credit** — which *showed up like a birthday cake on my Google Cloud Console homepage one day*. The framing that separates this course from the deep learning course: here you use an LLM to **structure unstructured data into a schema**, then treat the result as ordinary tabular data.

**Video 12 — Overview of the extractor-llm-poc** *(5:27)*
The head-to-head. Same listing, two outputs: the RegEx `jsonl` says the make is "contact information"; the `json_llm` says **Dodge Charger**. Back to the raw ad to see why — the whole thing is in caps, so the two-proper-case-words pattern never had a chance. Then the code tour: `vertexai` in requirements, environment variables, and the prompt — *extract only the following fields; return a strict JSON object that conforms to the provided schema* — plus `temperature=0` and `max_attempts` for retries, *because LLM calls can be flaky*.

**Video 13 — Interesting aspects of the extractor-llm-poc** *(8:39)*
The best video in the block, and the one to rewatch before the midterm. **Text data really is the new oil** — Dave's own arc from *if it wasn't a number, it wasn't useful* to feeding raw ads to a model.

What makes the code production-grade rather than a demo:
- **The schema is the contract.** Give the model variable names *and data types* and output stays consistent enough to concatenate, store, and model. Formatting *is everything when you're using an LLM as an accentuator in your business.*
- **Required vs optional, and the flattery problem.** Force too many fields and the model will invent one to please you — *"color must exist, so I'll just say silver to make you happy."* `nullable=true` lets absence be absence.
- **`temperature=0`** — *just the facts.*
- **Robust retries** on resource-exhausted / server / timeout errors, with sleep, so one flaky call doesn't crash the hourly job.
- **Post-processing you own**: strip commas, cast ints, tidy strings.
- **Provenance**: every record stamps the **LLM provider, the model, and the run time**. Two months from now a better model ships, and you can re-run the archive and compare — a cost decision, but an available one.
- **The gold/silver line**: gold means correct types *and* no missing data; since imputation happens later in modeling, this output is honest silver.

He also flags his own design smell — the LLM function reads the old `jsonl` path only to split a file name — and hands it to you as an exercise.

**Video 14 — Three cool ways to enhance/update the extractor-LLM-POC** *(4:05)*
The roadmap: **(1)** add fields to the schema — `transmission`, fuel type, color, title status, location (and you can ask Gemini itself what data type to use), keeping new fields optional; **(2)** write a **`materialize_llm`** function, because right now nothing downstream consumes the LLM output at all; **(3)** drop the dependency on the legacy `jsonl` path and name outputs after the source text file.

**Video 15 — Trying to update the extractor-LLM-POC... got stuck!** *(9:04)*
An unedited debugging session, kept in on purpose. Add `transmission` to the schema, commit, watch Actions spin, get the green check — and then the test returns **"50 processed, 50 skipped, written zero."** He asks Gemini, tries an overwrite flag, re-points the run at an older `run_id`, makes the field required, redeploys — and it's *still* 50 skipped. The video ends **unresolved**.

Watch it anyway. This is what the work looks like on a Tuesday, and the fix (next video) is not where anyone would look first.

**Video 16 — Updating the extractor-llm-poc to predict the transmission + materialize the LLM JSONs into one CSV** *(10:19)*
The catch, and it's a genuinely useful lesson about LLM plumbing: **the schema alone wasn't enough — the field has to be named in the prompt too.** Dave had said "model" in the instructions but never "transmission," so a field marked required still errored out. Copy the `model` line, change it to `transmission`, state that it's automatic or manual, commit, redeploy — green.

Second half: build `materialize_master_LLM` by copying `materialize_master` and swapping the folder paths to the `json_llm` prefix, plus a matching requirements file and a new YAML scheduled at **:25 past the hour** with its own deploy name. *A bit of a hacker's way, but exactly the exercise I want for students.*

**Video 17 — Materialize the LLM jsonl data, add a column for jsonl** *(5:23)*
Two self-inflicted bugs, both instructive. **One:** the new YAML still named the cloud function `materialize_master`, so the LLM version **overwrote the original deployment** — a name collision that silently clobbers a working function. **Two:** the materialize script has an explicit column list, so a new field must be added *there too* or it never reaches the CSV.

Then the payoff — open `listings_master_LLM.csv`: the `transmission` column exists but is mostly empty (the extractor hasn't re-run over the old files), the **makes are genuinely good** rather than "contact information," and price-versus-mileage on a log axis is visibly **tighter** than the RegEx version. That plot is the whole argument for the LLM in one picture.

**Video 18 — Oops we forgot transmission at the end!** *(2:27)*
The last place the field was missing: the **docstring and the final record-assembly step** at the bottom of `main.py`. Schema, required list, instructions, *and* the return — **four places**, and forgetting the fourth is why the column stayed empty. Short, and worth the two minutes.

**Video 19 — Diffs and wrapping up** *(2:55)*
The whole week reviewed as **diffs** on diffchecker.com, which is the cleanest possible summary. Extractor: docstring, schema, required, instructions, and the saved field. Materialize: add the column, read from `json_llm`, write `listings_master_LLM.csv`. YAML: the corrected function name, the deploy name, the path. *As you can see, it's not that many changes* — the skill is knowing **all** the places one field has to appear.

---

## Block 4 · Getting your results out
**Video 20 — Sync-data with GitHub Actions** *(6:06)*
Your bucket is private, so nobody can see your results — including the person grading you. The fix is a **GitHub-native** workflow (`sync_data.yml`, no cloud function involved) that at **:45 past the hour** copies `preds.csv` out of Cloud Storage into a `results/` folder in your repo. Also required: a **`.gitignore` to keep credentials out**.

Once results are public you can trend everything — R² and error over time, **which features are most important over time**, even the raw partial-dependence values so you can watch the *shape* of a PDP change as data accumulates. That's Module 2 coming back as a time series about your own model. Dave notes the job often fires late (*7:56 instead of 7:45*) and that this is fine: *we're in academia... we'll take the flexibility of GitHub Actions over the business requirement of serving something exactly on time.*

He closes by naming the flaw in the design — public predictions mean you could just copy a classmate's — which is exactly the problem **decentralized AI** solves by separating miners from validators. That's the thread the final project picks up.

```{admonition} Status note for Fall 2026
:class: warning
The **GCP → GitHub sync step is being revised** for this term; the scraping, RegEx/LLM ETL, and modeling threads are unchanged. Check the current Module 3.4 page before building your midterm around `sync_data.yml`.
```

---

## Where this goes next

Everything here converges on the **A08 midterm**: keep the scraper fixed, extend the **LLM** ETL with new fields, retrain with tuning from Module 2, sync predictions plus **permutation importance and your top-3 PDPs** to GitHub, and build a **model-trending notebook** that shows how your error and your explanations move over time. Module 4 then takes the same instinct — *what changed, and when?* — and makes time itself the subject.
