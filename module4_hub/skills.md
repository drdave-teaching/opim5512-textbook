# Module 4 Skills Sheet — What You Can Do Now

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Time-series work has a reputation for needing special models. It mostly doesn't — it needs **special discipline**. These are the moves and the guardrails.

---

## 🕰️ Datetime handling and feature engineering
*Videos 1-3 · Notebooks: Apple stock EDA · Assignment A09*

- ☐ Spot a date column that pandas is storing as a **string** (`df.info()` says object) and convert it
- ☐ **Set the datetime as the index** — and name the three things it unlocks (resampling, rolling windows, readable axes)
- ☐ Use `set_index(..., inplace=True)` rather than assigning `df.index` and leaving a duplicate column behind
- ☐ Decompose a date into groupable parts: `day_name()`, month, day, year
- ☐ Justify why day-of-week is a real feature and not decoration
- ☐ **Sort by date before any temporal operation**
- ☐ Use `diff()` and `pct_change()`, and explain why the first row is `NaN` and what to do about it
- ☐ Apply `np.sign()` to turn a change into up/flat/down (pandas has no `sign`)
- ☐ Compute **rolling averages** with `min_periods`, and explain why the head of the series is empty
- ☐ Describe a **moving-average crossover** and what traders read into it
- ☐ Frame rolling stats correctly for modeling: a polite way to **lag the past into a model** without leaking

## 📊 Plotting time properly
*Video 2*

- ☐ Never ship a time plot whose x-axis is the **record ID** — pass the actual date column
- ☐ Read a percent-change plot as a **volatility** story distinct from the level plot
- ☐ Build a grouped **seaborn box plot** by a time attribute (and set `hue` correctly)
- ☐ Subset a date range with boolean masks, **each condition in its own parentheses**
- ☐ Lay out **subplots** with `tight_layout` instead of pasting charts one at a time
- ☐ Explain a gap in a trading-day series (weekends and holidays) rather than "fixing" it

## 🧹 Cleaning a dirty time series
*Videos 4-6 · Notebooks: BDL weather EDA · Assignment A09*

- ☐ Recognize that **one sentinel character** (`M`, `T`) forces an entire numeric column to string
- ☐ Diagnose it the fast way: `df.info()` → print **unique values** → find the sentinel
- ☐ Clean it robustly with `pd.to_numeric` across columns instead of chasing sentinels one at a time
- ☐ Quantify missingness per column and **plot missingness over time** before choosing a strategy
- ☐ Explain why **mean/median imputation destroys a time series** (a flat line through a signal that is never flat)
- ☐ Apply **linear interpolation** as the sane default, and name its failure mode (a whole missing day)
- ☐ Use **forward-fill and back-fill together**, knowing `ffill` can't fix a missing first row and `bfill` can't fix a missing last one
- ☐ Test whether your imputation choice even matters by plotting two strategies on top of each other
- ☐ Recognize when `dropna()` would delete essentially the whole dataset — and why that's information, not an obstacle
- ☐ **Resample** to a chosen resolution with any aggregation, and **print the shape** so you can see what you traded away
- ☐ Use the modern bin aliases (`min`, `hour`)

## 🪟 The window method
*Videos 7-8 · Notebooks: NE energy + BDL weather*

- ☐ Explain the window method in one sentence: **turn lags into columns** so any tabular model works
- ☐ Build lag features with `shift()` and predict how the column count multiplies (29 features + 3 lags = 116)
- ☐ Decide **which columns to shift and which not to**, and defend it as a leakage argument
- ☐ Set up the realistic framing: today's *forecast* is knowable, today's *outcome* is not
- ☐ Run **repeated permutation importance** on a time-series model and interpret which **lag** matters
- ☐ Run **partial dependence** on the top features of a time-series model
- ☐ Cross-check a PDP against the **raw physical relationship** in the data and say whether the model learned the real thing
- ☐ Treat a suspiciously good model as a **leakage alarm**, and go to permutation importance first to find it

## 🧬 TSFresh and automated feature engineering
*Videos 9-11 · Notebooks: robot failures · BDL five-year weather · Assignment A09*

- ☐ Shape data the way TSFresh needs it: a **grouping ID** plus a **time column**, one row per observation
- ☐ Resample so each group has a consistent length, and **assert it** (a print statement that checks 24/day)
- ☐ Interpolate gaps *before* extraction
- ☐ Run `extract_features` and predict roughly how many features you'll get (~200 per column)
- ☐ Name what those features are: absolute energy, autocorrelation, Fourier coefficients, kurtosis, entropy, quantiles, location of the maximum
- ☐ Recognize the **curse of dimensionality** when features outnumber rows, and say what overfitting looks like there
- ☐ Cut features with `select_features` / `extract_relevant_features` — and judge honestly when it hasn't cut enough
- ☐ Sanity-check the output shape against the number of **groups**, not the number of raw observations
- ☐ Budget the runtime (12-15 minutes here) and know Dask exists for scaling it

## ⚖️ Honest evaluation
*Videos 11-13*

- ☐ Explain why a **random holdout** on time-series data can leak the future into training
- ☐ Build a **last-20%-in-time** holdout and expect it to score **worse**
- ☐ State plainly that the honest number is the worse one
- ☐ Describe **walk-forward validation** as the next step up
- ☐ Plot **actual vs. predicted over time**, not just as a scatter, and read *where* the model fails (missed extremes)
- ☐ Fix the two things that break that plot: a DataFrame-vs-Series index mismatch, and unreadable tick density
- ☐ Build an **anomaly detector** from residuals: predict → residual → threshold at a percentile (e.g. 99th) → flag
- ☐ Defend your threshold as a **product decision** (too low annoys, too high misses)
- ☐ Decide whether **absolute** error is right for your use case, or whether only one direction matters
- ☐ Describe a **human-in-the-loop** design where the system escalates rather than acts

---

## The one-sentence version

**You can take a filthy, irregular, partly-missing time series, clean it defensibly, turn time into columns, model it with tools you already know, evaluate it in a way that won't flatter you, and flag the moments where it fails.**
