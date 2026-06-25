# Chapter 2 — Tuning Models & Explainable AI

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

Finally, **autoML** (e.g., **TPOT**) automates the whole search — preprocessing, model choice, and tuning — into one pipeline. It's a great way to set a strong benchmark, as long as you keep the train-only and explainability discipline from this chapter.

## Wrap-up

```{admonition} Key takeaways
:class: tip
- **Imbalanced data** breaks accuracy; rebalance with **under/over-sampling or SMOTE** — on **train only** — and report a **distribution** (mean/SD/CI), not one number.
- **Cross-validation** (k-fold, repeated, LOOCV/LOGOCV) gives honest, distributional scores and is where you tune.
- **Spot-check** for signal, then tune inside a **`Pipeline`** with **`GridSearchCV`** (or Optuna/autoML) to prevent leakage.
- Open the black box with **permutation importance** (shuffle a feature, watch performance) and **PDPs** (averaged ICE curves).
- **autoML/TPOT** automates the search — a strong benchmark, with the same discipline.
```
