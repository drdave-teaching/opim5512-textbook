# Module 4 Hub — Time Series Modeling (Fall 2026)

:::{admonition} ⚠️ Work in progress
:class: warning
This book is under active development for Fall 2026 - pages may change as the course evolves.
:::

Time-series work has a reputation for needing special models. It mostly doesn't — it needs **special discipline**.

> **Turn time into columns, and hold out the tail rather than a random sample. The honest score is the worse one.**

## The pieces

| What | Where | Why |
|---|---|---|
| 🎥 **The 13 videos** (~1h20m) | HuskyCT (Module 4) · [what each one covers](videos.md) | watch in order |
| ✅ **Skills sheet** | [what you can do now](skills.md) | check yourself off after the videos |
| 📓 **The chapter** | [Chapter 4](../m4_timeseries/index.md) | the narrative version |

## The three blocks

**Block 1 · A clean time series** (videos 1-3) — Apple stock, chosen because it's clean, so you can focus on feature engineering: datetime conversion, setting the index, `diff`/`pct_change`, grouping by time attributes, rolling averages.

**Block 2 · A dirty one** (videos 4-6) — Bradley Airport weather, where `M` means missing, 90% of temperature readings are gone, and `dropna()` would delete the dataset. Compare imputation strategies and see why **mean imputation destroys a time series**.

**Block 3 · Modeling time** (videos 7-13) — the **window method** (lags as columns), xAI on a time-series model, **TSFresh** automated feature engineering, honest evaluation, and anomaly detection from residuals.

## The three datasets

| Dataset | Used for | Why this one |
|---|---|---|
| **Apple stock** (1980-2020) | feature engineering, plotting | clean, so nothing distracts from technique |
| **BDL airport weather** | cleaning, imputation, TSFresh | genuinely filthy — sentinels, gaps, irregular sampling |
| **NE energy + BDL weather** | the window method | a real forecasting problem with a physical explanation |

## The assignment

- **A09 — time series end to end.** Clean an irregular series, engineer lag features, model it, interpret it with permutation importance and PDPs, and evaluate it with a **time-ordered holdout**. Watch for leakage across time domains — it's the thing being graded.

:::{admonition} If your model looks great, suspect yourself first
:class: warning
Dave's rule, verbatim: *"If you get a really good model, you probably did the window method wrong, or you're leaking information, or your problem's just really easy. Whenever I see a scatter plot that's really tight on the line, the first thing I do is go to my permutation importance and ask, am I leaking something I shouldn't be?"*
:::

## Where this goes next

Module 5 swaps timestamps for text, but the shape of the work is identical: turn something that isn't a number into columns a model can use, then check honestly whether it learned anything.
