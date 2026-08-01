# Chapter 4 — Time Series Modeling

:::{admonition} 🔗 Notebooks for this chapter
:class: seealso dropdown
Open in Colab and **Runtime → Run all** — data loads from a stable link, nothing to upload.

- **EDA Time Series Financial** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/EDA_TimeSeries_Financial.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/EDA_TimeSeries_Financial.ipynb)
- **Missing Data Weather** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/Missing_Data_Weather.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/Missing_Data_Weather.ipynb)
- **TSFresh Example BDLFive Years (3)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/TSFresh_Example_BDLFiveYears%20(3).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/TSFresh_Example_BDLFiveYears%20(3).ipynb)
- **The Window Method Lags** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/TheWindowMethod_Lags.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module4/TheWindowMethod_Lags.ipynb)
:::


Time series is data where **order matters** and **yesterday helps predict today** — hourly temperature, daily prices, sensor streams. This chapter turns raw temporal data into reliable, interpretable, production-ready models, using ordinary ML (not deep learning) plus a powerful automated feature-engineering library.

## 4.1 Cleaning and resampling

Real time series are missing observations and arrive at irregular timestamps. First **resample** to a consistent resolution (hourly, daily, monthly) and **fill the gaps** thoughtfully:

- **Forward fill** (`ffill`) — carry the last known value forward (sensible for slow-moving readings).
- **Back fill** (`bfill`) — pull the next value backward.
- **Linear interpolation** — draw a straight line across the gap.

```python
df = df.set_index('timestamp').sort_index()
hourly = df.resample('1H').mean()            # consistent hourly grid
hourly['value'] = hourly['value'].interpolate(method='linear')   # fill gaps
```

The choice of imputation is a modeling decision — compare strategies and see which preserves the signal, because a careless fill can invent patterns that aren't there.

## 4.2 Feature engineering for sequences

To let an ordinary ML model "see" time, you engineer **time-aware features**:

- **Lags** — the value 1, 2, … *k* steps ago (`df['value'].shift(k)`).
- **Rolling statistics** — moving average / std over a window (`df['value'].rolling(24).mean()`), which smooth noise and capture momentum.
- **Seasonal indicators** — hour-of-day, day-of-week, month, to capture cycles.

This is the **window method**: slide a window of length *k* across the series so each row's features are the previous *k* values, and the target is the next value. Once it's a flat table, **any** model works — random forest, gradient boosting, an `MLPRegressor` from Chapter 1.

```{admonition} The cardinal sin: leakage across time
:class: warning
A time series must be split **chronologically** — train on the past, test on the future. **Never** shuffle a time series before splitting, and never compute a rolling feature that peeks at future rows. If a feature could only be known *after* the moment you're predicting, it's leakage, and your gorgeous test score is a fantasy. In production you only ever have the past.
```

## 4.3 Automated features with TSFresh

Hand-crafting lags and rolling stats is fine, but **TSFresh** automates it — extracting *hundreds* of physics-inspired features (autocorrelation, spectral statistics, entropy, peak counts…) from each sequence. The one requirement is shaping your data as **sequences with an `id` per sequence and a `time` column** to sort by:

```python
from tsfresh import extract_features
features = extract_features(long_df, column_id='id', column_sort='time')
```

TSFresh shines on problems like **robot-failure detection**, where the right discriminating feature is some obscure signal statistic you'd never think to compute by hand. (And it has a strict select-features step so you **don't leak** — respect it.)

## 4.4 Anomaly detection

Not every time-series task is forecasting. **Anomaly detection** asks *which points are weird* — a fraud spike, a failing sensor, an outage. ML approaches learn what "normal" looks like and flag departures from it (distance from a moving baseline, isolation-based models, reconstruction error). It's the same data-prep discipline — clean, resample, respect chronology — pointed at a different question.

## Wrap-up

```{admonition} Key takeaways
:class: tip
- Time series have **order**; **resample** to a consistent grid and fill gaps (**ffill/bfill/interpolate**) deliberately.
- Engineer **lags, rolling stats, and seasonal indicators**; the **window method** flattens the series so any ML model works.
- **Split chronologically** and never let a feature peek at the future — **leakage** is the cardinal sin.
- **TSFresh** auto-extracts hundreds of features from `id`/`time`-shaped sequences (great for robot-failure-style problems).
- **Anomaly detection** reframes the task to "what's abnormal," with the same prep discipline.
```


---

## 📌 Lecture key points

*Distilled takeaways from the video lectures behind this chapter — click each to expand.*


:::{admonition} Introduction to EDA for Time Series Data
:class: note dropdown
- Time series = order matters; **yesterday helps predict today**.
- EDA: plot the series, inspect trends/seasonality/gaps.
- Prepare data as sequences (id + time column).
- Watch for missing/irregular timestamps.
- Foundation for A09.
:::

:::{admonition} Grouping Data and Subplots
:class: note dropdown
- **`groupby`** + subplots to compare series/segments.
- Visualize multiple variables together.
- Spot patterns across groups.
- Clean plotting habits.
- EDA craft for temporal data.
:::

:::{admonition} Moving Averages and Momentum Trading
:class: note dropdown
- **Moving averages** smooth noise and capture momentum.
- Define your own vs built-in rolling functions.
- Evaluate with MAE/RMSE (RMSE sensitive to outliers).
- Momentum-trading framing (9- & 21-day averages).
- Rolling features feed models.
:::

:::{admonition} Missing/Dirty Weather Data EDA — Pt 1 & 2
:class: note dropdown
- Real weather data is **missing and dirty**.
- Compare **imputation strategies** (ffill, bfill, interpolation) for time series.
- Imputation choice is a modeling decision (can invent patterns).
- Clean before feature engineering.
- Resample to a consistent grid.
:::

:::{admonition} Resampling to different time horizons
:class: note dropdown
- **Resample** to hourly/daily/monthly resolution (`df.resample(...)`).
- Aggregate (mean/sum) to the target horizon.
- Fill gaps after resampling.
- Match resolution to the prediction task.
- Consistent grid = honest features.
:::

:::{admonition} Intro to the Window Method
:class: note dropdown
- **Window method**: previous *k* values become features (lags) for the next value.
- Flattens the series → **any** ML model works.
- Slide the window across the series.
- Tune the look-back length.
- The simplest way to model sequences.
:::

:::{admonition} Interpretability xAI and the Window Method
:class: note dropdown
- Apply **permutation importance / PDPs** to window-method models.
- Understand which lags/features drive predictions.
- xAI carries over from Module 2.
- Interpret time-dependent features.
- Trust + explain temporal models.
:::

:::{admonition} Intro TSFresh and Robot Failures
:class: note dropdown
- **TSFresh** auto-extracts **hundreds of physics-inspired features** from sequences.
- Needs data shaped as sequences with an **`id`** and a **`time`** column.
- Great for **robot-failure** detection (obscure signal stats matter).
- Replaces manual lag/rolling engineering.
- Then select features to avoid bloat.
:::

:::{admonition} TSFresh in Action — Pt 1 (create features) & Pt 2 (don't leak!)
:class: note dropdown
- Pt 1: `extract_features(df, column_id, column_sort)`.
- Pt 2: respect the **feature-selection / don't-leak** step.
- Never let features peek at the future.
- Split **chronologically**.
- Combine TSFresh features with any model.
:::
