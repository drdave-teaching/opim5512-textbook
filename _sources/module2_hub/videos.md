# Module 2 Video Guide — What Each Video Covers

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Nineteen videos, about 2 hours 5 minutes total, in three blocks: fix the data problem that quietly wrecks classification models, tune your models *honestly*, then crack the black box open and explain what the model actually did. Watch in order — each block sets up the next, and the last one is the one your future boss will ask about.

---

## Block 1 · Imbalanced data, sampling, and cross-validation
**The big ideas:** why a 90%-accurate model can have zero skill · majority undersampling, minority oversampling, and SMOTE as three different answers to the same problem · SMOTE as *interpolation between neighbors*, done by hand before it's done in code · SMOTENC for mixed numeric/categorical data · **the leakage rule that governs the whole module — resample the training partition only** · cross-validation as a way to get a *distribution* of error metrics instead of one lucky number.

**Video 1 — Majority Undersampling** *(5:17) · drives `0a_Undersampling.ipynb`*
The problem statement for the whole block. When you're chasing a rare event — sensor failures, fraud, a power outage in a town — you have *a whole lot of nothing and just a little bit of signal*. Fit a model on that and it gets greedy: it calls everything a zero, it's right 90% of the time, and it has **no skill**. The first fix is to balance the classes. Dave introduces **imbalanced-learn (`imblearn`)** — *like scikit-learn, but for imbalanced data* — then does it the honest way first: on the loan dataset (480 clean rows, 332 yes / 148 no), grab all 148 nos, sample 148 yeses **without replacement**, `concat` them, and you have 296 balanced rows. Only then the shortcut: `RandomUnderSampler` with `sampling_strategy="majority"`, which fits *a lot like a linear regression from scikit-learn* — `fit_resample(X, y)` and you're done.

**Video 2 — Oversample Minority** *(5:34) · drives `0b_Oversampling.ipynb`*
The mirror image: instead of throwing majority rows away, duplicate minority rows. By hand it's the same `sample()` call with **`replace=True`** — and Dave makes you reason about why: *how can I make 332 rows out of 148?* You can't, without replacement. Then `RandomOverSampler` with `sampling_strategy="minority"`.

This is also where the rule of the module lands, and it lands hard. Duplicate rows **before** you split and copies of the same row end up on both sides of the split — *you'll leak information into the test partition, get artificially good results, and then your model fails spectacularly in production*. Whatever you do to train, the test partition keeps the **original, real-world distribution**: if the data was two-to-one, the holdout stays two-to-one.

**Video 3 — SMOTE Pt. 1** *(7:47) · drives `0c_SMOTE (numeric features only).ipynb`*
Is there something better than throwing information away or duplicating information you already have? Yes: **SMOTE — Synthetic Minority Oversampling Technique** — and *synthetic* is the operative word. Dave walks the algorithm by hand before any code, in the clearest four-line form you'll find:

> Pick a row of interest — call it **home**. Find its five closest neighbors *within the same class*. Pick one — call it **neighbor**. Subtract the rows, column by column: **diff** = neighbor − home. Pick a random number between 0 and 1 — **gap**. Your new synthetic row is **home + gap × diff**.

It's a fake row that lies on the line between two real ones. He runs the k-nearest-neighbor search live so you can see the neighbors it picks are genuinely similar rows, and notes the variants that draw a different gap per column (here, gap is fixed — the original algorithm).

**Video 4 — SMOTE Pt. 2 (numeric only)** *(3:23) · drives `0c_SMOTE (numeric features only).ipynb`*
The `imblearn` implementation plus the picture that makes it click. Ten thousand synthetic points, imbalanced 99-to-1 (9,900 zeros, 100 ones), plotted in 2D and colored by class. Run SMOTE and watch it **linearly interpolate between the orange minority points** — and notice what it never touches: *the blue majority class isn't used at all.* Then play with `n_neighbors` to see the result shift. The constraint that matters: **SMOTE is numeric-only** — hand it red/green/blue and *it'll freak out*.

**Video 5 — SMOTENC** *(3:51) · drives `0d_SMOTE (numeric and categorical features).ipynb`*
The fix for mixed data: **NC = numeric and categorical**, which handles a categorical column by taking the majority value among the neighbors. The practical problem is that `categorical_features` wants **column indices**, which is miserable to type by hand for a wide dataframe — so Dave does it the clunky way first (*get it right, then do it right*), then writes the helper that builds `cat_list` from dtypes automatically. Best moment: how to spot synthetic rows at the bottom of the dataframe — *look at all the decimal points*. Real loan amounts never ran to `$0.088432`. Quantitatively useful, real-world nonsense.

**Video 6 — SMOTE with ML** *(6:03) · drives `0e_SMOTE with ML implementation.ipynb`*
The whole thing end to end, done correctly. Split first (391 train / 98 test, both still ~2-to-1), SMOTE **the training partition only**, fit a decision tree with `min_samples_split=10`, and score against a test set that still carries the original distribution. The improvement is modest and Dave says so plainly: *sampling is never going to drastically change things, but it's worth trying to bolster the signal.* Ends with the setup you should actually use in the real world — train / validation / independent test.

**Video 7 — CV Pt. 1 (K-fold, repeated k-fold)** *(9:20) · drives `1_CrossValidation.ipynb`*
Cross-validation from scratch, then with the API. A single 80/20 holdout is fine for a proof of concept — and Dave's smile test for it is **train and test error within about 10%** of each other. But one split gives you one number. So: chop the 80% into five folds, hold out each in turn, and collect five MAEs. He codes the fold indices by hand with a `for` loop before showing `KFold`, then generalizes to **10-times-repeated 5-fold** — 50 error metrics, which is why people report a mean and a standard deviation instead of a single number.

Two gotchas he flags: scikit-learn returns **negative** error metrics (it's minimizing, so everything is negated — multiply by −1, the interpretation is unchanged), and its folds won't match your hand-rolled ones because it shuffles first.

**Video 8 — CV Pt 2 (LOOCV, LOGOCV)** *(5:09) · drives `1_CrossValidation.ipynb`*
The two exotic flavors, and when each is a trap. **Leave-one-out** trains on all rows but one, 16,000 times — Dave literally had to kill the cell, *it was taking about 20 minutes*. It's useful for rare-event classification, and it is **invalid for time series**: knowing what happened before *and after* a held-out point leaks information you'd never have in operation.

**Leave-one-group-out** is the one to remember, and it comes with the story from his own research. A decade of outage modeling for electric utilities: with about 30 storms they used a random 20% holdout, got beautiful test results, then *a new storm would come and we'd be way off*. The fix was to train on 29 storms and predict the 30th, thirty times over — *pretending we were running the model in operation, seeing each storm for the first time.* Here the grouping column is `ocean_proximity`. If your data has natural groups — storms, patients, stores, campaigns — this is the honest evaluation.

---

## Block 2 · Spot-checking, pipelines, and hyperparameter tuning
**The big ideas:** spot-checking as a horse race between cheap models · `Pipeline` (capital P) wraps preprocessing *and* model so the scaling happens inside each fold · ensembles as many weak learners voting · `GridSearchCV` and how fast the model count multiplies · casting a wide net then searching a tighter grid.

**Video 9 — Intro to "Spotcheck" Models and Pipelines** *(7:02) · drives `1_Regression_SpotCheck.ipynb`*
Stop hand-tuning one model at a time. A **spot-check model** is one that's cheap, fast, and decent — linear regression, lasso, elastic net, decision tree, KNN, and (slowly) SVM. Store them as `(nickname, model)` tuples, loop over them, run 10-fold CV on the training partition, and keep the mean and standard deviation of each. Instead of `dtr1`, `dtr2`, `dtr3` and four copies of `X_train` floating around, one loop evaluates them all.

Before the modeling, the EDA he expects every time: histograms and box plots to *appreciate the shape of everything*, the **scatterplot matrix** (*I used to teach stats, and this was always my favorite plot*), and a rounded correlation matrix. The result is read off a **box plot of the folds** — closer to the top is better, and you want a tight box. Linear regression wins this round; SVR is slow and last.

**Video 10 — Pipelines with some preprocessing and LIGHT hyperparameter tuning** *(6:57) · drives `1a_AdvancedPipelines_Regression.ipynb`*
The memory device is his: **capital P Pipelines, lowercase p preprocessing. P's and p's.** Every entry in the list becomes a `Pipeline` of scaler-then-model — `scaled LR`, `scaled LR MM` with min-max instead of standard, `scaled KNN`, `scaled CART` with `min_samples_split=15`, another at 30. Now one loop runs a whole experiment grid, and the CART at 30 beats both the CART at 15 and the linear regression. Then a first formal `GridSearchCV` on KNN — the param grid is a dictionary, printed so you can see it, and the winner (k=3) gets refit and taken to test. He leaves you an exercise: **also store the test result per model as a distribution.**

**Video 11 — Ensemble Methods and Spotchecking, then GridSearchCV for tuning** *(5:55) · drives `1a_AdvancedPipelines_Regression.ipynb`*
What an ensemble actually is, in plain terms. A **random forest** is ~100 decision trees, each grown on a random subset of rows *and* columns; average them for a robust point estimate (and if you want a quasi-nonparametric confidence interval around each prediction, that's a quantile forest). **Gradient boosting** he frames as curve fitting: small trees, each fit on the **residuals of the previous tree**, additive, and able to approximate any function if you let it run.

Then the arithmetic that surprises everyone: a grid of 32 combinations under 10-fold CV is **320 model fits**, and the winner (depth 3, 200 estimators) still tests a little worse than it trained. *Test is always a little worse than train.*

**Video 12 — Advanced Pipelines (Pipelines and Hyperparameters at the same time)** *(7:45) · drives `1a_AdvancedPipelines_Regression.ipynb`*
Why do the horse race and the tuning as two separate steps? The analogy is Dave's: *see who the best horse is, then take that horse and give it some horseshoes or a new saddle.* Or do both at once — four pipelines (decision tree, decision tree + PCA, random forest, random forest + PCA), each with its own parameter grid, all looped through `GridSearchCV`, tracking `best_error` and the winning pipeline name.

Also the PCA argument you should know: **an integer gives you that many components, a decimal gives you enough components to hit that share of variance** (`PCA(0.95)` = 95% of the variance). And the honest warning about cost — 36 combinations × 10 folds = 360 fits for a humble decision tree, ~3,600 for a random forest, *which is why I say go get a cup of coffee*. Winner here: the vanilla random forest, no PCA.

**Video 13 — Advanced Pipelines for Classification Models** *(5:31) · drives `2a_Pipelines_GridSearch_Classification.ipynb`*
The same machinery for classification, seven models deep, with a tour of where the hyperparameters come from — the scikit-learn supervised-learning docs, read live. Note `max_features` for a random forest defaults to the **square root** of the column count (16 columns → 4 per tree). Score on accuracy or swap in weighted F1.

The strategy advice is the takeaway: **use interesting values or the grid is duplicative.** Search by orders of magnitude or by doubling to cast a wide net, then run a **second, tighter grid** around the winner. It's iterative, and it's your judgment call, not the package's.

**Video 14 — Spotcheck Classification Models** *(5:31) · drives `2_Classification_SpotCheck_and_Pipelines.ipynb`*
The classification counterpart to Video 9, on the **Wisconsin Breast Cancer** dataset (569 rows, 33 columns, benign vs. malignant — small enough to run fast). Two details in passing: the data is **lightly imbalanced**, which he chooses to accept here while pointing at the `imblearn` pipelines from block 1 as the fix, and the `LabelEncoder` makes **benign = 0** simply because *benign starts with B*.

The spot-check lineup for classification: **logistic regression, decision tree, KNN, and Gaussian naive Bayes** — *simple estimators, they only have a few hyperparameters anyway* — with `max_iter` raised on logistic regression so it stops complaining. (Credit where due: he attributes the term "spot check" to **Jason Brownlee**.) Then the scaled/ensemble round: AdaBoost, KNN, GBM, random forest, extra trees.

The reading of the results is the part worth copying. Logistic regression has the best mean accuracy, but **scaled KNN is right behind it with a smaller standard deviation — *which means the results are more stable***. And scaled AdaBoost *most of the time gets 98%, but two realizations you get really poor models* — exactly the instability a single accuracy number would have hidden. That's the argument for box plots over point estimates, made with a real example.

He then tunes AdaBoost: 8 `n_estimators` values × 3 learning rates = **24 combinations × 10-fold = 240 fits**, winner `learning_rate=0.01` with 400 estimators, scoring **0.982 on test — slightly better than train**. And rather than take the win, he questions it: *maybe it's just a lucky split*, and suggests repeated k-fold with different test partitions to find out.

---

## Block 3 · Explainable AI
**The big ideas:** model-agnostic explanation · permutation importance = shuffle a column and watch the model break · why every model has a different flavor of "important" · importance for **variable selection**, not just storytelling · PDP as the average of the ICE curves · reading the rug marks before you trust a wiggle.

**Video 15 — Introduction to Permutation Importance** *(6:50) · drives `0_PermutationImportance_Regression.ipynb`*
The cleanest definition of feature importance that works for *any* model: take your fitted model, and feed it a **corrupted** test set — one column shuffled, everything else intact. *If you shuffle a column and still get a great model, that column isn't important; if you shuffle it and the model becomes horrible, that column is important.* Because there are a million ways to shuffle, you repeat it (`n_repeats=10`) and report a **box plot**, not a number. The x-axis is the **decrease in R²**.

Cost is worth knowing: 13 predictors × 10 repeats = 130 evaluations, cheap for a linear regression, slow when every evaluation walks 100 trees. And Dave's preference, with his reasoning: run it on the **test** partition — *it shows how the model would fail on unseen data versus learned patterns*, and it's less leakage-prone.

**Video 16 — Permutation importance as a tool for storytelling and variable selection** *(5:16) · drives `0_PermutationImportance_Regression.ipynb`*
Shouldn't every model agree on what's important? **No** — *because each model treats the input data differently.* A linear regression asserts a linear relationship; a decision tree doesn't. So the tree ranks rooms first, the linear model ranks LSTAT, DIS, RAD, rooms. Watch `RAD` on the tree: its distribution straddles **zero**, meaning shuffling it sometimes *improves* performance — a hint it doesn't belong.

Then the professional move, from his insurance-modeling years: with a thousand predictors, fit a boosted tree, read the importances, keep the **top ten**, and fit a simple interpretable model on those. *Use nonlinear models for variable selection, then linear models for prediction.* He lines up three models, keeps the four columns they agree on, refits, and loses very little.

He also sets up the next video with the meeting you don't want to be in: you've told the decision-maker which variables matter, and they ask *"okay, but how is your random forest treating the number of rooms?"* — and you're sweating, because importance says **what** matters, not **which way**.

**Video 17 — Intro to Partial Dependence Plots (PDPs)** *(8:00) · drives `1_PartialDependence_Regression.ipynb`*
*One of my favorite aha moments for aspiring data scientists.* The mechanism, built up row by row: take one row, hold every column fixed, and replace LSTAT with each of its **370 unique values** in turn. Predict all 370. That single wiggly line is an **ICE curve** — individual conditional expectation. Do it for all 404 training rows, and **average them at each step**: the red line is the partial dependence plot, the gray lines are the ICE curves it came from. As LSTAT rises, predicted price falls.

Dave runs PDPs on the **train** partition — *it answers "how did the model use the data that went into it?"* — and reads several curves out loud, including a lumpy one for crime, with a caution that the lumps may just be where the observations are.

**Video 18 — 'Clunk' PDPs and linear vs. nonlinear responses** *(5:36) · drives `1_PartialDependence_Regression.ipynb`*
Two practical things and one conceptual payoff.

Practical: **`num_grid_points`**. Evaluating all 370 unique values is why student PDPs hang — the source of *frantic emails at 11:50 at night*. Set it to 10 or 20 and it samples evenly spaced values instead; he drops it to 5 to show the curve getting coarse while the trend survives. Also the **rug marks** along the bottom: each tick is a percentile of the data. Where the ticks are dense there's signal; a dramatic wiggle over two data points means nothing.

Conceptual: run the exact same PDP code against a **linear regression** and every curve comes back a straight line, because a coefficient is a straight line by construction. Run it against the **random forest** and it's lumpy with a steep drop and a flat tail. That contrast *is* the difference between the two model families, drawn rather than asserted: **flat means the model isn't using that feature.**

Put the two tools together and you have the whole story — permutation importance says which columns matter, partial dependence says which way they push.

**Video 19 — TPOT and autoML** *(5:12) · drives `3_autoML and TPOT_updated.ipynb`*
**AutoML**: hand a clean dataset to `TPOTClassifier` / `TPOTRegressor` and a **genetic algorithm** searches over whole pipelines — preprocessing, model, and hyperparameters together — then hands you the best one. Start with `generations=1` (still ~20 minutes), confirm it works, *then* turn it up. Save the winner with **joblib**, because the point is to reload the fitted pipeline later and apply it to new data without refitting. The regression run returns a pipeline of percentile selection → PCA → LightGBM with 74 estimators at depth 4 — *I never would have coded that.*

---

## Where this goes next

Module 2 is the modeling toolkit; **Module 3 puts it in production.** The tuned model, the importance plot, and the PDP all reappear there as artifacts a scheduled cloud job regenerates every hour — and again in the A08 midterm, where you'll be asked to trend your error metrics *and* your interpretability over time.
