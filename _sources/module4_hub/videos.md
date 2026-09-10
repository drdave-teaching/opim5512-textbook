# Module 4 Video Guide — What Each Video Covers

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Thirteen videos, about 1 hour 20 minutes total, in three blocks: learn the moves on a **clean** time series, then survive a genuinely **dirty** one, then model time with the tools you already have. The unifying trick of this module is that you never need a special "time series model" — you need to **turn time into columns** and hand the result to scikit-learn.

Three datasets carry the module: **Apple stock** (clean, for feature engineering), **Bradley Airport (BDL) weather** (filthy, for cleaning), and **New England daily energy production** joined to that weather (for modeling).

```{admonition} The rule underneath everything here
:class: important
In a time series you can always *accidentally* use the future to predict the past. Every technique in this module comes with a leakage warning attached, and the two evaluation videos (8 and 12) exist almost entirely to show you what cheating looks like when it's working.
```

---

## Block 1 · EDA and feature engineering on a clean time series
**The big ideas:** convert to `datetime`, then **set it as the index** — that one move unlocks resampling, rolling windows, and readable axes · decompose a date into groupable parts · `diff` and `pct_change` (and why row one is `NaN`) · plot the thing · rolling averages as a way to *politely* lag the past into a model.

**Video 1 — Introduction to EDA for Time Series Data** *(7:15) · Apple stock*
Why time series is *a cousin of a regular structured dataset*: time orders the rows, and that ordering unlocks analyses that are meaningless anywhere else. Apple from 1980 to ~2020, deliberately clean *so we can focus on feature engineering instead of cleaning*.

The first bug is the one you'll hit forever: `df.info()` shows `date` as an **object**, not a datetime — it looks like a date and behaves like a string. Convert it, then start mining it: `day_name()`, month, day, year as their own columns, *so you can treat it almost like a database and do aggregations, groupbys, and slicing.* Why bother? Because day-of-week is real signal — *on Friday, people generally settle up their accounts.*

Then `diff` and `pct_change`, row-wise subtractions against the previous row, with two details worth keeping: **sort by date first** (in case anything got shuffled), and the first row is `NaN` because nothing precedes it — *you almost need a warm-up*. Drop or zero-fill those once prep is done.

**Video 2 — Grouping Data and Subplots** *(6:59) · Apple stock*
Mostly about not embarrassing yourself with a chart. *It drives me bananas when people have time-series data and don't actually visualize it.* Plot the adjusted close and the x-axis reads 0–10,000 — those are **record IDs**, and in a meeting your boss will ask what 8,000 means. Pass the actual date column and matplotlib formats it properly.

The percent-change plot tells the volatility story that the price plot hides — *it makes you appreciate people saying the stock market is a random walk* — including what looks like a **40% single-day drop** in the dot-com bust.

Then grouped analysis: a seaborn box plot by weekday (note the `hue` fix), and the finding that Wednesday is the strongest day — *so if I were building a trading bot, I'd consider settling up in the middle of the week.* Plus two mechanics you'll reuse constantly: **boolean masks need parentheses around each condition** or the chain fails, and **subplots** (1 row × 2 columns, `tight_layout`) so you're not pasting charts into PowerPoint one at a time.

**Video 3 — Moving Averages and Momentum Trading** *(7:03) · Apple stock*
Opens with the line Dave calls top-line news: **convert your date to datetime and set it as the index.** `set_index(..., inplace=True)` removes the column for you; the hacker route (`df.index = col`) leaves you to delete it yourself.

Then **`np.sign`** (pandas has no sign function) wrapped around `pct_change`, turning every day into +1/0/−1 — the seed of a trading rule. Aggregate the signs by month across 30 years and you get a real finding: **June loses, August wins**, and the "October surprise" isn't much of one.

Finally **rolling averages**, which he frames in the way that matters for modeling: *I use them to politely lag information from the past into my model.* A 5-day and a 100-day average (watch `min_periods` leave the head of the series empty), overlaid — the **crossover** is the classic momentum signal: cross above, people are buying; drop below, they're selling.

---

## Block 2 · Cleaning a genuinely dirty time series
**The big ideas:** a single sentinel character poisons a whole column's dtype · missingness has *shape*, so look before you impute · **mean imputation destroys a time series** · interpolation, forward-fill, back-fill and when each fails · resampling to a resolution you choose.

**Video 4 — Missing/Dirty Weather Data EDA Pt. 1** *(5:17) · BDL weather*
The real-data video. Bradley Airport hourly observations: temperature, dew point, humidity, wind direction and speed, precipitation — and **`M` wherever a value is missing**. One letter in a numeric column and pandas stores the entire column as strings, *and you can't do math on a string.*

The diagnosis workflow is the transferable part: `df.info()` looks wrong → print the **unique values** → there's the `M`. Fix it column-wise by replacing `M` with `np.nan` and casting, and then discover precipitation hides a **second** sentinel, `T` for *trace*. Which motivates the better move: run `pd.to_numeric` across the numeric columns and let **every** junk sentinel become `NaN` at once, whether or not you knew about it.

Then measure the damage — about **90% of temperature and humidity readings missing** — and *plot the missingness before choosing a strategy*, because the observations arrive in clumps. *Right off the bat, if you imputed with the mean, you'd destroy this time series.*

**Video 5 — Missing/Dirty Weather Data EDA Pt. 2 (imputation strategies)** *(7:27) · BDL weather*
The comparison, run as experiments on a copy. First the discovery that sinks the obvious approach: **~8,000 of 9,000 rows are dirty somewhere**, so `dropna()` on rows leaves nothing, and dropping dirty *columns* leaves only metadata — the timestamp, lat/long, and the raw **METAR** string (*a coded message sent from the weather station to the airplane... it would actually be a fun deep-learning project to decode*).

So, imputation, judged by eye:
- **Mean** — a flat line through a signal that is never flat. *A lot of students do this... for a time-series problem it's very dangerous.*
- **Median** — the same flat artifact, shifted.
- **Linear interpolation** — the default recommendation. *If you showed me this as your boss, I'd say it makes perfect sense.* The failure mode: a whole missing day gets a straight line where real dynamics belong.
- **Forward-fill / back-fill** — with the asymmetry spelled out: `ffill` can't fix a missing **first** row, `bfill` can't fix a missing **last** row, so use them **in combination**.

The diagnostic to steal: forward-fill and back-fill plotted on top of each other land in nearly the same place, *which means the interpolation isn't very sensitive in this problem* — i.e. the choice doesn't matter much here, and you know that because you checked.

**Video 6 — Resampling to different time horizons** *(3:26) · BDL weather*
Irregular sampling — sometimes every 5 minutes, sometimes hourly, sometimes a gap — fixed by `resample()` once the datetime is the index. Resample to 60 / 600 / 2,000 minutes with any aggregation you like, and **print the shape each time**: the curve still looks fine while the blue line quietly becomes 22 points. That's the trade — smoothness for resolution — and you should be able to state which you chose and why. (Note the modern bin aliases: `min` and `hour`, not the old `T`.)

Closing thought worth acting on: this is *one* of roughly 10,000 airport stations, and a clean script runs on all of them.

---

## Block 3 · Modeling time
**The big ideas:** the **window method** — lags as columns, so any tabular model works · shifting carefully so today never sees today · xAI on a time-series model · **TSFresh** for automated feature engineering · the curse of dimensionality · random holdout vs. a held-out tail, and why the honest one scores worse.

**Video 7 — Intro to the Window Method** *(7:00) · NE energy + BDL weather*
*A hacker's way of using traditional machine learning models with a time-series dataset* — no tensors, no sequence models. If the series is 1, 2, 3, 4, 5 and you're standing at 5, then lag 1 is 4, lag 2 is 3, and so on: **make each lag its own column** and predict with a decision tree.

First a genuinely interesting look at the data — New England daily production by fuel type: gas dominates, nuclear is steady (with maintenance dips), **oil appears only when it's needed** to fill a gap, hydro has a seasonal shape. Then the mechanics: `shift()` to build the lags, three lags by default, so 29 current features become **116 columns** (current + 3 lags). Train before 2025, test after.

The design decision is the lesson: **predict today's energy from today's *weather* and the *past* days' energy.* You can't know today's energy before predicting it, but you can know today's forecast — *today is December 10th; I have tomorrow's weather forecast and know today's usage, so I should be able to predict tomorrow's usage.* Which columns you shift, and which you don't, is exactly where leakage lives.

**Video 8 — Interpretability xAI and the Window Method** *(4:54) · NE energy + BDL weather*
Module 2's tools pointed at a time-series model, and it works beautifully. **10-times-repeated permutation importance** on the test partition ranks **yesterday's total energy** first — unsurprising and reassuring — followed by today's average feel, today's minimum temperature, and (odd, and named as odd) **solar radiation from two days ago**.

Then **partial dependence** on the top three, and the moment the module is built around: plot raw `average_feel` against actual energy, and the physical relationship has the **same shape** as the model's PDP. *I always love seeing where the domain knowledge or physics ties to what your machine learning model did.*

And the warning to tape to your monitor:

> If you get a really good model, you probably did the window method wrong, or you're leaking information, or your problem's just really easy. Whenever I see a scatter plot that's really tight on the line, the first thing I do is go to my permutation importance and ask, *am I leaking something I shouldn't be?*

**Video 9 — Intro TSFresh and Robot Failures** *(6:03)*
*Something I'm completely obsessed with*: **automated feature engineering for time-series data**, rooted in physics and signal processing. The setup is **grouped** time series — many short sequences, each with an ID and a time column — illustrated with the classic robot-failures dataset, where a healthy robot traces smooth lines and a failing one is visibly jagged.

You could hand-engineer rolling averages and variances... or use the features *people have been developing for 150 years*: absolute energy, autocorrelation, counts of slopes above a threshold, the value **and the location** of the maximum. `extract_features` with `column_id` and a time column produces **~200 features per column** — six columns of robot coordinates becomes ~1,200 features. Which immediately creates the problem: with 100 rows and 1,200 features you will overfit, so `select_features` / `extract_relevant_features` exist to cut it back. Cost warning up front: it's slow, and Dask can parallelize it.

**Video 10 — TSFresh in Action Pt. 1** *(6:52) · BDL five-year weather*
TSFresh on real data, with the data-shaping done carefully — this is the video to copy when you apply it yourself. Five years of BDL weather, predicting the **daily maximum dew point**. Resample to **60 minutes** so every day has exactly 24 observations (with a print statement that *checks* it), linearly interpolate the gaps first, group by date, and let time be the sort key.

Why hourly: the station *chirps every hour to tell planes the weather*, and chirps more often when conditions change — so raw data has uneven sequence lengths. (TSFresh tolerates that; the demo standardizes for clarity.) The run takes **12–15 minutes** and returns **6,304 features** over ~1,900 daily rows — Fourier coefficients, kurtosis, entropy, quantiles, percentage of recurring values — *absolutely incredible automated feature engineering*, and an obvious dimensionality problem.

**Video 11 — TSFresh in Action Pt 2: don't leak!** *(6:19) · BDL five-year weather*
First the sanity check that every TSFresh user should run: 1,900 rows isn't the original 40,000 hourly observations — it's **365 × 5 days**, one row per day, matching `y` one-to-one. Then `select_features` cuts 6,304 columns to ~1,920 against 1,928 rows, and Dave says plainly that this is *still not great* — his real next step would be to fit a quick model and keep the top 100.

Then the comparison that gives the video its name. **Random 20% holdout:** R² 0.99 train / 0.93 test, MAE 3.4 — and it will never throw an error. **Last-20%-in-time holdout:** the same model scores **worse**, MAE 3.7. That gap is the whole point: *a very penalizing way to evaluate a time-series model, and a time-series model demands that scrutiny so you're not cheating.* The honest number is the higher one. Bonus: with a time-ordered holdout you can finally plot **actual vs. predicted over time**, not just as a scatter.

**Video 12 — Time series plots for results** *(3:59)*
The chart that beats a scatter plot in a meeting. A scatter of actual vs. predicted *tells one version of the story*; plotting the same predictions **against time** shows you *where* the model is wrong — and here it's wrong in a specific, describable way: **it misses the extremes**, top and bottom, shaving the peaks and troughs. You can see that in the scatter as points off the 45-degree line, but you can't see *when*.

The precondition matters: this plot only works if you held out a **contiguous chunk at the end** (video 11's honest split). Shuffle the rows and *your time series is gonna look really funky*. He also names **walk-forward validation** as the next step up — train on the first 20%, predict the rest; then 30%, then 50%, and so on.

Two practical fixes worth watching, because both will happen to you: a `split_point` error that turns out to be a **DataFrame-vs-Series mismatch** (he pulls the dates out into their own variable and reattaches them as the index), and unreadable x-axis labels fixed by setting **weekly tick marks** — because in a meeting *your boss is going to be like, well, what day is that?*

**Video 13 — Intro to Anomaly Detection** *(3:58)*
The best domain story in the module, and the reason the whole course keeps circling back to residuals. From Dave's **insurance and IoT** days: sensors in homes and businesses — water, temperature, door, motion — and the goal of *being proactive... to delight our customers* by texting them when something's wrong.

The hard part isn't the sensor, it's that **every building is different**. A restaurant's temperature drops at night because they prop the doors open after the deep fryer's been running — normal, someone's there. A church that's been empty all week and starts cooling on a Wednesday is *someone forgot to set the heat, or maybe the furnace exploded.* Same signal, opposite meaning.

So the method is the modeling you already know, pointed at its own errors: predict the next time step from history, take the **residual** (actual − predicted), and flag it as an anomaly when it exceeds a **percentile threshold** of the residual distribution — he uses the **99th**. The threshold is a product decision, not a statistical one: *if the threshold is too low, we're going to annoy you with a lot of text messages; if the threshold is too high, we might not text you enough.* Flagged points get plotted — actuals in red, predictions as yellow ×'s — so you can look at a case where the model said 62 and reality was 48 and decide whether you'd have sent that text.

One nuance he flags for your own use case: **absolute error treats both directions the same**, and often you only care about one (a freeze loss is not the same as an unexpected warm-up). Ends on where this actually lands in practice — **human-in-the-loop**: the system doesn't act, it escalates to an operator who decides whether the model needs retraining.

---

## Where this goes next

Module 4 hands you two habits that outlive the course: **make time into columns**, and **hold out the tail, not a random sample**. Module 5 changes the raw material from timestamps to text, but the shape of the work is identical — turn something that isn't a number into columns a model can use, then check honestly whether it learned anything.
