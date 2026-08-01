# Chapter 2 — Tuning Models & Explainable AI

:::{admonition} 🔗 Notebooks for this chapter
:class: seealso dropdown
Open in Colab and **Runtime → Run all** — data loads from a stable link, nothing to upload.

- **Permutation Importance Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0_PermutationImportance_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0_PermutationImportance_Regression.ipynb)
- **a Undersampling** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0a_Undersampling.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0a_Undersampling.ipynb)
- **b Oversampling** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0b_Oversampling.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0b_Oversampling.ipynb)
- **c SMOTE (numeric features only)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0c_SMOTE%20(numeric%20features%20only).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0c_SMOTE%20(numeric%20features%20only).ipynb)
- **d SMOTE (numeric and categorical features)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0d_SMOTE%20(numeric%20and%20categorical%20features).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0d_SMOTE%20(numeric%20and%20categorical%20features).ipynb)
- **e SMOTE with ML implementation** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0e_SMOTE%20with%20ML%20implementation.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/0e_SMOTE%20with%20ML%20implementation.ipynb)
- **Cross Validation** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_CrossValidation.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_CrossValidation.ipynb)
- **Partial Dependence Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_PartialDependence_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_PartialDependence_Regression.ipynb)
- **SHAP From Scratch Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2a_SHAP_FromScratch_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2a_SHAP_FromScratch_Regression.ipynb)
- **LIME From Scratch Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2b_LIME_FromScratch_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2b_LIME_FromScratch_Regression.ipynb)
- **Regression Spot Check** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_Regression_SpotCheck.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1_Regression_SpotCheck.ipynb)
- **a Advanced Pipelines Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1a_AdvancedPipelines_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/1a_AdvancedPipelines_Regression.ipynb)
- **Classification Spot Check and Pipelines** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2_Classification_SpotCheck_and_Pipelines.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2_Classification_SpotCheck_and_Pipelines.ipynb)
- **General Modeling Framework Advanced** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2_General%20Modeling%20Framework_Advanced.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2_General%20Modeling%20Framework_Advanced.ipynb)
- **a Pipelines Grid Search Classification** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2a_Pipelines_GridSearch_Classification.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/2a_Pipelines_GridSearch_Classification.ipynb)
- **Hyperparameter Tuning Widgets** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/3_HyperparameterTuning_Widgets.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/3_HyperparameterTuning_Widgets.ipynb)
- **auto ML and TPOT updated** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/3_autoML%20and%20TPOT_updated.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/3_autoML%20and%20TPOT_updated.ipynb)
- **Optuna NNs** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/Optuna_NNs.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/Optuna_NNs.ipynb)
- **scripts 1 Regression Spot Check** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__1_Regression_SpotCheck.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__1_Regression_SpotCheck.ipynb)
- **scripts 1a Advanced Pipelines Regression** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__1a_AdvancedPipelines_Regression.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__1a_AdvancedPipelines_Regression.ipynb)
- **scripts 2 Classification Spot Check and Pipelines** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__2_Classification_SpotCheck_and_Pipelines.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__2_Classification_SpotCheck_and_Pipelines.ipynb)
- **scripts 2a Pipelines Grid Search Classification** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__2a_Pipelines_GridSearch_Classification.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__2a_Pipelines_GridSearch_Classification.ipynb)
- **scripts 3 Hyperparameter Tuning Widgets** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__3_HyperparameterTuning_Widgets.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module2/scripts__3_HyperparameterTuning_Widgets.ipynb)
:::


This chapter is about building models you can **trust** and **explain**. First we deal with the data problems that quietly wreck models (imbalance), then we tune hyperparameters *honestly* with cross-validation and pipelines, and finally we crack the black box open with model-agnostic interpretability.

## 2.1 Imbalanced data and sampling

Real classification targets are often **imbalanced** — far more zeros than ones (sensor failures, fraud, rare disease). A greedy model just predicts the majority class and scores 90% accuracy while being **useless** (it can't beat predicting "everything is normal"). The fix is **sampling** to balance the classes:

- **Undersample** the majority (throw away some zeros).
- **Oversample** the minority (duplicate the ones).
- **SMOTE** — *synthesize* new minority examples by interpolating between real ones; **SMOTENC** handles mixed numeric/categorical data.

```{admonition} Resample the TRAINING partition ONLY
:class: warning
SMOTE and over/under-sampling touch the **training** data only. The **validation and test** sets must keep the **original, real-world class distribution** — otherwise you're scoring your model on synthetic data, which tells you nothing about reality. This is the single most common way students invalidate their own results.
```

And don't report one accuracy number — **repeat** the sampling-and-fitting many times and report the **distribution**: mean, standard deviation, and a confidence interval ("accuracy was 88–94% (95% CI), mean 92%, SD 2%"). A range is an honest answer; a single number is a lucky one.

## 2.2 Cross-validation

A single train/test split is one roll of the dice. **k-fold cross-validation** splits the training data into *k* folds, trains on $k-1$ and validates on the held-out fold, rotating through all *k* — so every row gets a turn as validation. **Repeated k-fold** does this several times with different shuffles; **LOOCV** (leave-one-out) and **LOGOCV** (leave-one-*group*-out) are extremes for small or grouped data. CV gives you that *distribution* of scores, and — crucially — you tune hyperparameters **on the cross-validated training folds**, never on the test set.

## 2.3 Spot-checking, pipelines, and grid search

Before tuning anything, **spot-check**: throw a handful of default models at the problem to see whether the data has *any* signal worth modeling. If nothing beats the baseline, no amount of tuning will save you.

Then tune properly — and tune **inside a pipeline** so every preprocessing step is refit within each CV fold (no leakage). A **`Pipeline`** chains preprocessing and model into one object; **`GridSearchCV`** searches hyperparameter combinations with cross-validation:

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import GridSearchCV

pipe = Pipeline([('scl', StandardScaler()),
                 ('pca', PCA(0.95)),                      # optional dimensionality reduction
                 ('clf', RandomForestRegressor(random_state=42))])

grid = GridSearchCV(pipe,
                    param_grid={'clf__n_estimators': [100, 300],
                                'clf__max_depth':   [None, 5, 10]},
                    cv=5, scoring='neg_mean_absolute_error')
grid.fit(X_train, y_train)          # scaler+PCA refit inside every fold — no leakage
print(grid.best_params_)
```

```{admonition} Why a pipeline, not just a script?
:class: tip
A pipeline guarantees the **scaler is fit on the training folds only** during cross-validation — do it by hand and you'll eventually leak test information into your scaling and never know. As datasets grow, beyond grid search you can use **Optuna** or **autoML** to search smarter; but the pipeline is the non-negotiable foundation that keeps the search honest.
```

## 2.4 Explainable AI

A tuned model that nobody understands is a liability. Two **model-agnostic** tools open any black box:

**Permutation importance.** Shuffle one feature's column in the **test** set, leave the rest intact, and re-score. If shuffling crime rate tanks the $R^2$, crime was important; if nothing changes, it wasn't. It directly measures *how much the model relies on each feature*.

```python
from sklearn.inspection import permutation_importance
r = permutation_importance(model, X_test, y_test, n_repeats=30, random_state=42)
# r.importances_mean ranks features by how much shuffling each one hurts performance
```

**Partial dependence plots (PDPs).** Take a feature (say crime), sweep it across its range while holding the other features fixed, and re-predict — repeat for every row to get hundreds of **individual conditional expectation (ICE)** curves, then **average** them. The result shows *how the model's prediction responds* to that feature: linear, nonlinear, flat, a cliff.

Permutation importance and PDPs are **global** — one story for the whole dataset. The next two tools go **local**: they explain a *single* prediction.

**SHAP — fair credit from game theory.** For one prediction, how much did *each* feature push it above or below the baseline $E[f(x)]$? SHAP borrows the **Shapley value** (Nobel-winning cooperative game theory): a feature's contribution is its **average marginal contribution across every coalition** of the other features. It is the *only* attribution that satisfies **local accuracy** (the contributions sum exactly to prediction − baseline), **symmetry**, **dummy**, and **additivity** all at once. We build it **from scratch** — enumerating coalitions and weighting marginal contributions by hand — before checking it against the `shap` library. Average the absolute SHAP values over many rows and you recover a permutation-importance-style ranking; scatter them and you recover a PDP. SHAP *unifies* the chapter.

```python
# SHAP from scratch: average marginal contribution over all coalitions S
phi_i = sum( weight(|S|, p) * (v(S | {i}) - v(S)) for S in subsets_without(i) )
# where v(S) marginalizes "absent" features over a background sample, and
# weight(s, p) = s! (p - s - 1)! / p!  ->  the fraction of orderings with S before i
```

**LIME — a local linear surrogate.** Any curvy model, viewed up close, looks almost straight. LIME **perturbs** the row you want to explain, asks the black box for predictions on that cloud of nearby points, **weights** them by closeness (a kernel of width $\sigma$), and fits a **weighted linear regression**. Its coefficients *are* the local explanation — on a 1-D curve they literally recover the **derivative**. It's fast and intuitive, but the **kernel width** is a real knob: too wide and the "local" story drifts global, so always report it and check the surrogate's local $R^2$. SHAP and LIME answer the same question differently — when they **agree on direction**, trust the story.

```python
# LIME from scratch
Z  = x0 + noise                          # 1) perturb into a neighborhood
yz = model.predict(Z)                    # 2) query the black box
w  = exp(-dist(Z, x0)**2 / (2*sigma**2)) # 3) weight by closeness
coefs = LinearRegression().fit(Z, yz, sample_weight=w).coef_   # 4) local importances
```

Finally, **autoML** (e.g., **TPOT**) automates the whole search — preprocessing, model choice, and tuning — into one pipeline. It's a great way to set a strong benchmark, as long as you keep the train-only and explainability discipline from this chapter.

## Wrap-up

```{admonition} Key takeaways
:class: tip
- **Imbalanced data** breaks accuracy; rebalance with **under/over-sampling or SMOTE** — on **train only** — and report a **distribution** (mean/SD/CI), not one number.
- **Cross-validation** (k-fold, repeated, LOOCV/LOGOCV) gives honest, distributional scores and is where you tune.
- **Spot-check** for signal, then tune inside a **`Pipeline`** with **`GridSearchCV`** (or Optuna/autoML) to prevent leakage.
- Open the black box with **permutation importance** (shuffle a feature, watch performance) and **PDPs** (averaged ICE curves) — both **global**.
- Explain a **single** prediction with **SHAP** (fair, additive credit from game theory; built from scratch over coalitions) and **LIME** (a weighted local linear surrogate; mind the kernel width). When they **agree**, trust the story.
- **autoML/TPOT** automates the search — a strong benchmark, with the same discipline.
```


---

## 📌 Lecture key points

*Distilled takeaways from the video lectures behind this chapter — click each to expand.*


:::{admonition} Majority Undersampling
:class: note dropdown
- Imbalanced data breaks accuracy (greedy model predicts the majority).
- **Undersample** the majority class to balance.
- Repeat sampling → report metrics as a **distribution** (mean/SD/95% CI).
- Loses data but simple and fast.
- A03 leverages this.
:::

:::{admonition} Oversample Minority
:class: note dropdown
- **Oversample** (duplicate) the minority class to balance.
- Keeps all majority data.
- Risk of overfitting duplicated points.
- Compare against undersampling.
- Part of the sampling toolkit.
:::

:::{admonition} SMOTE — Pt 1 & Pt 2 (numeric only)
:class: note dropdown
- **SMOTE** *synthesizes* new minority examples by interpolating between real ones.
- Pt 2: numeric-only feature handling.
- More principled than naive oversampling.
- **Apply on the TRAINING partition only.**
- Validation/test keep the original distribution.
:::

:::{admonition} SMOTENC
:class: note dropdown
- **SMOTENC** handles **mixed numeric + categorical** features.
- Same train-only rule.
- Choose the right SMOTE variant for your data types.
- Avoids breaking categorical encodings.
- Completes the synthetic-sampling options.
:::

:::{admonition} SMOTE with ML
:class: note dropdown
- Integrate SMOTE **inside** the ML workflow (within CV folds).
- Prevents leakage from resampling the whole set.
- Report error metrics as a distribution (CI).
- "Accuracy was 88–94% (95% CI), mean 92, SD 2."
- Sampling + modeling combined honestly.
:::

:::{admonition} CV Pt 1 (K-fold, repeated k-fold)
:class: note dropdown
- **k-fold CV**: rotate folds so every row is validated once.
- **Repeated k-fold** adds shuffles for a richer score distribution.
- Tune hyperparameters on the **CV training folds**, never the test set.
- A single split is one roll of the dice.
- Foundation for honest tuning.
:::

:::{admonition} CV Pt 2 (LOOCV, LOGOCV)
:class: note dropdown
- **LOOCV** (leave-one-out) for very small data.
- **LOGOCV** (leave-one-group-out) for grouped data.
- Extremes of the k-fold idea.
- Choose CV scheme to match data structure.
- Avoids group leakage.
:::

:::{admonition} Intro to "Spotcheck" Models and Pipelines
:class: note dropdown
- **Spot-check**: throw several default models to see if the data has **any** signal.
- If nothing beats the baseline, tuning won't save you.
- Introduces **`Pipeline`** (chain preprocessing + model).
- Pipelines prevent leakage inside CV.
- Sets up grid search.
:::

:::{admonition} Pipelines with preprocessing and LIGHT hyperparameter tuning
:class: note dropdown
- Wrap **scaler (+PCA) + model** in a `Pipeline`.
- Pipeline refits preprocessing **inside each fold** (no leakage).
- Start with light tuning to get a feel.
- `clf__param` syntax for nested params.
- Reproducible, leakage-safe tuning.
:::

:::{admonition} Ensemble Methods and Spotchecking, then GridSearchCV
:class: note dropdown
- Spot-check ensembles (RF, GBM) for signal.
- **`GridSearchCV`** searches hyperparameter combos with CV.
- Pick `best_params_`; score with a chosen metric.
- Ensembles often strong baselines.
- Grid search = exhaustive but honest.
:::

:::{admonition} Advanced Pipelines (pipelines + hyperparameters together)
:class: note dropdown
- Tune **preprocessing and model hyperparameters simultaneously**.
- One pipeline object searched by GridSearchCV.
- Cleaner, leakage-safe, reproducible.
- Scales to many preprocessing choices.
- The professional pattern.
:::

:::{admonition} Advanced Pipelines for Classification Models
:class: note dropdown
- Same advanced-pipeline pattern for classification.
- Spot-check + grid search classifiers in pipelines.
- Evaluate with confusion-matrix metrics.
- Handle imbalance within the pipeline.
- Reproducible classification tuning.
:::

:::{admonition} Introduction to Permutation Importance
:class: note dropdown
- **Shuffle one feature's column in the test set**, keep others, re-score.
- Big drop in R² ⇒ that feature was **important**.
- Model-agnostic interpretability.
- `permutation_importance(..., n_repeats=...)`.
- Ranks features by reliance.
:::

:::{admonition} Permutation importance as storytelling and variable selection
:class: note dropdown
- Use permutation importance to **explain** and to **select variables**.
- Tell a clear story about what drives predictions.
- Drop low-importance features to simplify.
- Repeated permutation for stability.
- Communicates model behavior to stakeholders.
:::

:::{admonition} Intro to Partial Dependence Plots (PDPs)
:class: note dropdown
- **PDP** = average of per-row **ICE** curves: sweep one feature, hold others fixed, re-predict.
- Shows **how** the model responds to a feature (linear/nonlinear/flat/cliff).
- Model-agnostic.
- Complements permutation importance (which feature vs how it's used).
- Built on the test partition.
:::

:::{admonition} Clunky PDPs and linear vs. nonlinear responses
:class: note dropdown
- PDPs reveal **nonlinear** feature responses a linear model would miss.
- "Clunky" steps show discretized sweeps.
- Interpret shapes for business insight.
- Caveats when features are correlated.
- Deepens xAI intuition.
:::

:::{admonition} SHAP values from scratch
:class: note dropdown
- **SHAP** explains **one** prediction: each feature's push above/below the baseline $E[f(x)]$.
- Rooted in **game theory** — a feature's value is its **average marginal contribution over all coalitions**.
- **Local accuracy**: baseline + Σ SHAP = the prediction, exactly (plus symmetry, dummy, additivity).
- Built **from scratch** by enumerating coalitions; absent features marginalized over a background sample.
- mean|SHAP| recovers permutation importance; a SHAP dependence plot recovers the PDP.
:::

:::{admonition} LIME from scratch
:class: note dropdown
- **LIME** fits a simple **local linear surrogate** to the black box around one prediction.
- Recipe: **perturb → predict → weight by closeness → fit a weighted line → read coefficients**.
- On a 1-D curve it recovers the **local slope (derivative)** — verified by hand.
- The **kernel width** is the master knob; check the local $R^2$ for fidelity.
- Fast and intuitive vs SHAP's fairness guarantees — when they agree on direction, trust the story.
:::

:::{admonition} TPOT and autoML
:class: note dropdown
- **autoML (TPOT)** automates preprocessing + model choice + tuning into one pipeline.
- Great for a strong **benchmark**.
- Keep the train-only and explainability discipline.
- Searches smarter than brute grid search.
- A04 leverages this.
:::
