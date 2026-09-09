# Lab 2 — Explaining a Model (SHAP)

**Module 2 · Explainable AI — the in-person lab.** Half lecture, half hands-on **with GitHub**, on the same branch → pull request → review → merge loop from Lab 1.

:::{admonition} The one-sentence version
:class: important
A good score is not the finish line. Tonight you take a model that already predicts New England electricity demand well (**R² ≈ 0.90**) and make it **explain itself** — two partners explain the *same* model two ways, then merge the views into one report.
:::

The model — a random forest on the Lab 1 energy data — is **already trained for you** in the template. You don't tune it tonight; you make it talk. Your only code is **one line of SHAP.**

---

## SHAP in five sentences (the lecture, short)

1. A good model isn't the finish line — you have to be able to say **how it decided**.
2. **SHAP** gives every feature, for every prediction, a number: how many **MW** it pushed the prediction **up (+)** or **down (−)** from the average prediction.
3. Add a row's SHAP values to the average and you get that exact prediction — it's **additive and honest** (`14,952 MW base + the feature pushes = 23,082 MW`).
4. **Global** = stack all rows to see which features matter overall, and in which direction (a **beeswarm**).
5. **Local** = one row, one prediction, one story (a **waterfall**) — the answer you give a stakeholder.

The full walk-through is in the [SHAP lecture](https://github.com/drdave-teaching/OPIM5512-labs/blob/master/Module2/Week3_xAI/Lab2_SHAP/SHAP_LECTURE.md); the by-hand version is in [Chapter 2 §2.4](../m2_tuning_xai/index.md).

## Who owns what

| | Partner A — **global** | Partner B — **local** |
|---|---|---|
| question | *Which features matter, overall?* | *Why THIS one prediction?* |
| notebook | `Lab2_A_Global_SHAP` | `Lab2_B_Local_SHAP` |
| branch | `dev-global` | `dev-local` |
| given | the model's built-in importances bar | a predicted-vs-actual scatter |
| you write | one line → a SHAP **beeswarm** | one line → a SHAP **waterfall** |

## What we're doing

1. **Set up the repo** — Partner A: [Lab 2 template](https://github.com/drdave-teaching/opim5512-lab2-template) → **Use this template** → add Partner B → branch protection on `main` (PR + 1 approval). Both clone once; each makes a `dev-` branch.
2. **Run the setup (given)** — `Runtime → Run all` installs SHAP, loads the data, fits the random forest (note the **R²**), and builds `shap_values` — one push in MW per feature, per row. You didn't write any of it; you're about to *use* it.
3. **Write your one SHAP line** in the TODO cell:
   - **A:** `shap.plots.beeswarm(shap_values, show=False)` → save `shap_global.png`
   - **B:** `i = int(np.argmax(model.predict(X)))` then `shap.plots.waterfall(shap_values[i], show=False)` → save `shap_local.png`
4. **Look at your plot, then ship it** — download the PNGs, **File → Save** the notebook to your `dev-` branch, drag the PNGs into `images/`, commit, push.
5. **Review & merge** — open a PR, your partner reads your SHAP cell and approves; merge both; `Fetch → Pull`.
6. **The report** — fill `REPORT.md` (image links are pre-wired). The finding to nail: **do the global view and the local hour tell the *same* story about what drives demand?**

:::{admonition} The one honest caveat
:class: warning
SHAP explains **the model**, not physical cause. "The forest leans on `hour_of_day` and `dewpoint_f`" is a statement about the model's behavior — not a claim that muggy 6 PMs *cause* demand. Say so in your report.
:::

## What the model looks at

The data is the Lab 1 energy join — one row per hour. Target = `load_mw` (New England demand).

| feature | meaning | units |
|---|---|---|
| `temp_f` | air temperature | °F |
| `hour_of_day` | 0–23 | hour |
| `dewpoint_f` | dew point | °F |
| `humidity_pct` | relative humidity | % |
| `wind_kt` | wind speed | knots |
| `weekend` | 1 = Sat/Sun | 0/1 |

**Ahead of schedule?** Run the joint notebook for a **dependence plot** (`shap.plots.scatter`) — how one feature's push changes across its range, colored by a second feature — and chase the interaction it exposes.

## Definition of done

- [ ] Branch protection on `main`; both partners are collaborators
- [ ] `images/` has four PNGs (the given plot + your SHAP plot, per partner)
- [ ] Both notebooks saved back with one SHAP line each (beeswarm / waterfall)
- [ ] `REPORT.md` — one sentence per plot, plus the "what SHAP can't tell us" line
- [ ] ≥3 merged pull requests, branches deleted, both authoring **and** reviewing

The full kit — instructions, the 20-step map, printable handouts, run-of-show slides, and the Colab starters — is in [Module2/Week3_xAI/Lab2_SHAP](https://github.com/drdave-teaching/OPIM5512-labs/tree/master/Module2/Week3_xAI/Lab2_SHAP).

:::{admonition} New to the workflow?
:class: tip
It's the same branch → PR → review → merge loop as [Lab 1](../module1_hub/lab1.md). If that felt shaky, skim the Lab 1 kit first — Lab 2 assumes it.
:::

---

*Global says what the model leans on; local tells the story of one hour. Tonight you shipped both — and the report only closes when you can say whether they agree.*
