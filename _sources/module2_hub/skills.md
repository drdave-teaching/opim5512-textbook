# Module 2 Skills Sheet — What You Can Do Now

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

After the Module 2 videos, notebooks, and assignments A03 and A04, these are the skills you own. Not "watched a video about" - *own*. Go down the list and check them off; if one feels shaky, the notebook or video to revisit is listed right there.

Module 1 taught you to fit a model. Module 2 teaches you to fit one you can **defend** - honestly evaluated, honestly tuned, and explainable to someone who will never read your code.

---

## ⚖️ Imbalanced data and sampling
*Videos 1-6 · Notebooks: `Module2/0a_Undersampling.ipynb` · `0b_Oversampling.ipynb` · `0c_SMOTE (numeric features only).ipynb` · `0d_SMOTE (numeric and categorical features).ipynb` · `0e_SMOTE with ML implementation.ipynb` · Assignment A03*

- ☐ Explain why **90% accuracy can mean zero skill**, and name the rare-event problems where this bites (fraud, sensor failure, outages)
- ☐ Identify the **majority** and **minority** class in a target and quantify the imbalance ratio
- ☐ Balance classes **by hand** with `sample()` + `concat` before reaching for a library
- ☐ Use `RandomUnderSampler(sampling_strategy="majority")` and `RandomOverSampler(sampling_strategy="minority")` from `imblearn` via `fit_resample`
- ☐ Explain why oversampling requires **`replace=True`** (you cannot make 332 rows from 148 without it)
- ☐ Describe **SMOTE in four steps** without notes: pick **home** → find its K nearest neighbors *in the same class* → pick one as **neighbor** → new row = **home + gap × (neighbor − home)**, gap random in [0,1]
- ☐ State the constraint that **SMOTE is numeric-only**, and use **SMOTENC** when you have categorical columns
- ☐ Build the `categorical_features` index list **programmatically** from dtypes instead of typing indices by hand
- ☐ Spot synthetic rows in a dataframe (they carry absurd decimal precision) and explain why they're quantitatively useful but not real
- ☐ **The rule of the module:** resample the **training partition only**, and keep the validation/test partitions at the original real-world distribution - and explain exactly what leaks if you don't

## 🔁 Cross-validation
*Videos 7-8 · Notebook: `Module2/1_CrossValidation.ipynb` · Assignment A03*

- ☐ Explain why a single train/test split gives you **one lucky number**, not an estimate
- ☐ Apply the smile test: train and test error within roughly **10%** of each other
- ☐ Code **k-fold by hand** with a `for` loop and shifting fold indices, then reproduce it with `KFold`
- ☐ Run **repeated k-fold** (e.g. 10 × 5-fold) and report the **mean and standard deviation** of the error metric, not a point value
- ☐ Read scikit-learn's **negative** error metrics correctly (`neg_mean_absolute_error` - multiply by −1)
- ☐ Explain why your hand-rolled folds and scikit-learn's don't match (it shuffles first)
- ☐ Use **leave-one-out** where it belongs (rare-event classification, small data) and explain its two costs: runtime, and invalidity for time series
- ☐ Use **leave-one-group-out** when your data has natural groups, and explain the storm-forecasting logic: train on 29 storms, predict the 30th, repeat - *seeing each storm for the first time*
- ☐ Name the group column in a dataset you care about (storms, patients, stores, campaigns, `ocean_proximity`)

## 🏇 Spot-checking and pipelines
*Videos 9-11, 14 · Notebooks: `Module2/1_Regression_SpotCheck.ipynb` · `1a_AdvancedPipelines_Regression.ipynb` · `2_Classification_SpotCheck_and_Pipelines.ipynb`*

- ☐ Do the **EDA Dave expects before any model**: histograms/box plots, a **scatterplot matrix**, and a rounded correlation matrix
- ☐ Define a **spot-check model** (cheap, fast, decent) and name six of them
- ☐ Store models as **`(nickname, model)` tuples** and evaluate them all in one loop - no more `dtr1`, `dtr2`, `dtr3`
- ☐ Compare algorithms from a **box plot of the CV folds** (high median, tight box wins)
- ☐ Build a **`Pipeline`** that chains scaler → model, and explain why the scaling must live *inside* the pipeline
- ☐ Keep **capital-P Pipelines** and lowercase-p preprocessing straight
- ☐ Swap scalers (`StandardScaler` ↔ `MinMaxScaler`) and light hyperparameters across pipeline variants to run many experiments at once
- ☐ Explain what a **random forest** is (≈100 trees on random row/column subsets, averaged) and what **gradient boosting** is (small trees fit successively on the **residuals** of the previous tree)
- ☐ Name a use for a **quantile forest** (a nonparametric interval around each point estimate)

## 🎛️ Hyperparameter tuning
*Videos 10-13 · Notebooks: `Module2/1a_AdvancedPipelines_Regression.ipynb` · `2a_Pipelines_GridSearch_Classification.ipynb` · `3_HyperparameterTuning_Widgets.ipynb` · `Optuna_NNs.ipynb` · Assignment A04*

- ☐ Write a **param grid** as a dictionary and print it to confirm what you're actually searching
- ☐ Run **`GridSearchCV`** over a *pipeline*, so preprocessing and tuning happen together
- ☐ **Do the arithmetic before you hit run:** 32 combinations × 10 folds = 320 fits; 36 × 10 = 360 for one decision tree; ~3,600 for a random forest
- ☐ Find hyperparameters in the **scikit-learn docs** rather than guessing, and know that `max_features` defaults to **√(n columns)**
- ☐ Choose **interesting grid values** - orders of magnitude or doubling - so the search isn't duplicative
- ☐ Run the **two-stage search**: cast a wide net, then a tighter grid around the winner
- ☐ Refit the winning configuration and report **train *and* test**, expecting test to be slightly worse - *test is always a little worse than train*
- ☐ Use `PCA(0.95)` vs `PCA(5)` correctly (**decimal = share of variance, integer = number of components**)
- ☐ Track `best_error` / `best_pipeline` across a loop of grid searches to crown an overall winner

## 🔍 Explainable AI
*Videos 15-18 · Notebooks: `Module2/0_PermutationImportance_Regression.ipynb` · `1_PartialDependence_Regression.ipynb` · `2a_SHAP_FromScratch_Regression.ipynb` · `2b_LIME_FromScratch_Regression.ipynb` · Assignment A04*

- ☐ Define **permutation importance** in one sentence: shuffle one column of the test set, leave the rest intact, and measure how far the model breaks
- ☐ Explain why it is **model-agnostic** and works on anything with a `.predict()`
- ☐ Run `permutation_importance(clf, X_test, y_test, n_repeats=10)` and read the **decrease in R²** on the x-axis
- ☐ Report importance as a **box plot** (many shuffles = a distribution), never a single bar
- ☐ Justify running it on the **test** partition - it shows how the model fails on unseen data
- ☐ Explain why **different models rank features differently**, and interpret an importance distribution that **straddles zero** as a feature that may not belong
- ☐ Use importance for **variable selection**: wide net → boosted tree → keep the top ~10 → refit a simple, defensible model
- ☐ Define an **ICE curve**: one row, one column swept across its unique values, everything else held fixed
- ☐ Define a **PDP** as the **average of the ICE curves**, and read direction off it ("as LSTAT rises, predicted price falls")
- ☐ Use **`num_grid_points`** to keep a PDP from hanging, and describe the trade-off in curve resolution
- ☐ Read the **rug marks** and refuse to over-interpret a wiggle where there's almost no data
- ☐ Explain what a **flat PDP** means, and why a linear model's PDPs are straight lines by construction
- ☐ Answer both boardroom questions in order: **which features matter** (importance) and **which way they push** (partial dependence)

## 🤖 autoML
*Video 19 · Notebook: `Module2/3_autoML and TPOT_updated.ipynb`*

- ☐ Explain what **TPOT** searches over (whole pipelines - preprocessing, model, and hyperparameters) and how (a **genetic algorithm**)
- ☐ Fit `TPOTClassifier` / `TPOTRegressor` starting at **`generations=1`**, then scale up once it works
- ☐ Budget the runtime honestly (~20 minutes at generations=1; hours if you mean it)
- ☐ **Save the fitted pipeline with `joblib`**, reload it later, and apply it to new data without refitting
- ☐ Read a TPOT-discovered pipeline and say what it chose and why it's surprising

---

## The one-sentence version

**You can take an imbalanced, real-world dataset, evaluate a model honestly enough that it won't embarrass you in production, tune it without leaking, and then explain to a non-technical decision-maker which variables mattered and which way each one pushed.**

That sentence is the entire A04 assignment, and it's most of what a working data scientist is paid to do.
