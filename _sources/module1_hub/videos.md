# Module 1 Video Guide — What Each Video Covers

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Ten videos, about 1 hour 35 minutes total, in three blocks: set up the shop, refresh the modeling methodology, then build your first neural network. Watch in order - each video drives one stretch of its notebook.

---

## Block 1 · Setting up the shop
**The big ideas:** GitHub as your professional face · the four concepts that cover 90% of daily Git (repo, commit, branch, pull request) · the repo layout that makes work reproducible (`source/`, `figures/`, `requirements.txt`) · the industry review loop - branch → commit → PR → review → merge → delete · branch protection · running a script from a terminal.

**Video 1 — Hi from Dr. Dave** *(0:36)*
The hello. Dave Wanik - engineer by training, came to the business school in fall 2019, now associate professor in residence in Operations and Information Management, data scientist, and guitar nut (hence the guitars in every background). Thirty-six seconds, and then we're working.

**Video 2 — A nice intro to A01, making your first repos** *(11:16) · drives `SettingUpTechEnviro.ipynb`*
The A01 assignment, done live. Start with the **special repo named after yourself** - the one that renders on your account landing page. That's your face to the world; make it fancy. Then install **GitHub Desktop**, create the **A01** repo (public, with a README), and clone it. Notice Dave is on `main` and calls it out: *that's a little bit of a no-no in modern data science workflows* - so he makes a **`dev`** branch and works there. In VS Code he builds the file structure (`source/` for code, `figs/` for output - and shows the misclick where a folder nests inside another, plus how to avoid it by clicking the README first), writes **`requirements.txt`**, and drops in the starter `boxplot.py` that pulls the California housing dataset. Watch the **color coding** in VS Code: green means brand new, yellow means edited. Then the loop - publish the branch, discover it didn't push because he forgot to **commit** first, commit with a real message, push, open the **pull request**, confirm the merge, and **delete the `dev` branch** (*my computer science friends tell me branches shouldn't live that long*). It closes with running the script from the VS Code terminal after `pip install -r requirements.txt`.

**Video 3 — A02 "Ping Pong" live demo, with Rohit** *(29:33) · WebEx recording · drives `Week2_1_NN_Regression`*
The collaboration assignment, start to finish, with research scholar **Rohit** on the other side of the ping-pong table. Dave creates the repo, adds Rohit under **Settings → Collaborators**, and sets a **branch ruleset**: require a pull request before merging, required approvals = 1. He clones in GitHub Desktop, makes branch `dev-DW`, builds `source/` and `figures/`, updates the README, and writes `DS_pipeline.py` to read California housing from scikit-learn.

Then the best part of the video: **the commit that wouldn't commit.** The commit button is grayed out. They check file boxes, consider fetching origin, wonder about the co-author field, even screenshot it for ChatGPT. The real culprit - Dave protected **all** branches instead of just `main`, so his own branch was blocked. He rescopes the rule to the default branch and it commits. *This is normal, and knowing how to fix it is the skill.*

Then the loop, twice. Dave opens a PR with Rohit as reviewer - **pending review, blocked, cannot merge**. Rohit reviews in **Files changed**, chooses between *Request changes*, *Comment*, and *Approve*, approves, merges, deletes the branch. Then they flip: Rohit branches, edits, pushes, opens his own PR with Dave reviewing. (Aside: Dave removes **Copilot** as a reviewer - *I want the students doing the reviews*; with Copilot you'd need `reviewers = 2`.) Dave then makes the repo actually runnable - `requirements.txt` with scikit-learn, a box plot of **MedHouseVal** saved into `figures/`, and a terminal run of `python source/DS_pipeline.py`, with the rule stated plainly: **everything you import at the top of your script must be in `requirements.txt`.** The payoff is the **network graph** - two contributors, branches out and back. The ask: do this back-and-forth **at least five times**.

---

## Block 2 · The ML refresher
**The big ideas:** the fixed methodology - read → split → scale on train → fit → evaluate · the leakage rule (`fit_transform` on train, `transform` on test) · why scaling buys interpretability, not just performance · the four regression models and the four classification models · train-versus-test as the honest comparison · how Dave wants results *told*, not just computed.

**Video 4 — ML Regression Refresher** *(9:14) · drives `DTR_RFR_GBR_BostonHousing.ipynb`*
End-to-end regression, Dave's way. The imports (pandas, NumPy, matplotlib, seaborn, `train_test_split`, `MinMaxScaler`, four models, the metrics), then a note on style: *bomb the script with information* when you're doing an analysis, and dial the comments way back when you put something into production. The data: 506 rows, 14 columns, predicting **MEDV**, `.info()` clean. Split X and y, confirm the shapes every time (13 + 1 = 14), 80/20 train/test.

Then the two ideas worth pausing on. First, **why scale**: it matters more for neural nets than trees, but min-max scaling also buys you *interpretability* - a 0.9 means 90% of observations sit below that value, which a raw number can't tell you unless you're an expert with the data. Second, **the leakage rule**: `fit_transform` on the whole dataframe leaks extreme values from the future into your model. Fit on train, transform the test partition. (Notice the scaled arrays are NumPy, not pandas - the column names are gone, and a test value slightly above 1.0 just means train had the larger maximum.)

Four models get fit vanilla - linear regression, decision tree (which will overfit, because the default minimum leaf size is one; `min_samples_split=15` calms it down), random forest, gradient boosting - each with a suffix so beginners can copy-paste-modify. Evaluation is R², MAE, MSE for **both** partitions, plus **bias** (the mean of the errors - does the model systematically over- or under-predict), plus predicted-versus-actual **scatter plots with a 45-degree line** for train and test side by side. The warning that matters: if your scatter comes back a flat line, your model is predicting the mean and learned nothing - *it drives me bananas when students send me that*.

**Video 5 — ML Classification Refresher** *(5:39) · drives `DTC_RFC_GBC_BostonHousing.ipynb`*
The same workflow, one dataset, different target. Dave recodes median house price into 1 (above the median) / 0 (below) on purpose - *so you focus on the technique and not the dataset*. Split, scale the X partitions, fit logistic regression (the statistical baseline) plus decision tree, random forest, and gradient boosting classifiers.

Then the part he's particular about: **evaluation**. Two things are required from every classification model in this course - a **confusion matrix** for train *and* test (true negatives, true positives, false positives, false negatives; you want the mass on the top-left-to-bottom-right diagonal), and the **classification report**. The decision tree overfits as promised and the test partition tells a different story. Gradient boosting wins, and Dave reads it the way he wants you to: best confusion matrix, strongest precision and recall, therefore the best **weighted F1** - 0.88 against 0.85, 0.85, 0.85. *So I'm right; of course I'm right.*

---

## Block 3 · Neural networks in scikit-learn
**The big ideas:** a neural network is a nonlinear weighted sum · forward propagation, feedback signal, backpropagation · the three flavors of gradient descent and what an epoch actually is · where the nonlinearity comes from (activation functions between layers) · why scaling is non-negotiable here · `max_iter` and early stopping · regressor versus classifier is one activation on the output node · architecture rules of thumb.

**Video 6 — Intro to NNs for regression with sklearn** *(6:33) · drives `Week2_1_NN_Regression`*
The anatomy, with no math required. Eight California-housing inputs (x1 … x8) go in, one prediction comes out. In between: multiply the 1×8 input row by a learned weight matrix to get a 1×10 hidden layer, multiply that by another to get 1×5, multiply that by a third to get the 1×1 output. *A neural network is simply a weighted sum.* Dave prints the actual shapes of all three weight matrices so you can see the rectangles. Weights start random and get better as the model hums along. Once the forward pass reaches the output you compute a **feedback signal** - actual minus predicted - and its sign tells the network which way to move: negative residual means the weights were too small, positive means the model is overestimating.

**Video 7 — Gradient descent and backpropagation** *(5:08)*
How the update actually happens. Forward propagation carries information input → hidden → hidden → output; then **backpropagation** carries the error back to adjust every weight. First a warning: without nonlinearity, all of this collapses into linear regression (*in fact, you can do linear regression with a neural network - I leave that here as an exercise*). Then the three flavors of gradient descent, defined by **how many rows flow through before a weight update**: **stochastic** (one row at a time - very jumpy, 10,000 updates for 10,000 rows), **full** (the whole dataset, one averaged update per pass - fastest per epoch, heavily smoothed), and **batch** (10, 20, 50, 100 rows at a time - the middle ground, and what most people do). Along the way, the definition that trips everyone up: an **epoch** is one weight-update pass across the dataset, however you chunk it.

**Video 8 — Fit a NN (MLPRegressor) with sklearn** *(8:55) · drives `Week2_1_NN_Regression`*
Showtime - the first fit. Open the `MLPRegressor` documentation and be excited rather than overwhelmed by the keyword arguments. **Where the nonlinearity lives:** activation functions sit *between* layers. ReLU - Rectified Linear Unit - is positive in, positive out; negative in, zero out. Look at the learned weights: plenty are large negatives, which is fine - it's the *product* of input and weight that gets clipped to zero in the hidden layer. Without ReLU you'd have a linear regression.

**Why scaling is critical here** (not merely nice, as with trees): unscaled columns overwhelm the network, the weight updates jump around, and the model never settles. Standard scaler or min-max, it doesn't matter much - do one. Then the knobs: `hidden_layer_sizes` (he swaps 10-and-5 for 3 and reruns to show the shapes change), `activation` (ReLU is the default choice - *honestly they fit the same, and I always just use ReLU*), `max_iter` (the epoch count - 200 by default, which can be slow and can overfit), and **early stopping**, which holds out 20% *of the training data* as a feedback signal and stops when the model quits generalizing. Note carefully: `X_val_scaled` and `X_test_scaled` are never involved in fitting - the network has no idea they exist. Finally, a caveat about scikit-learn: you get the **training** loss curve only, not the separate train/validation curves you'd see in Keras or PyTorch - so treat it like any other estimator and judge it on metrics and scatter plots.

**Video 9 — MLPClassifier** *(9:36) · drives `Week2_1_NN_Classification`*
Generalizing to classification. Recode the housing target - above the 75th percentile is 1 (about 5,000 expensive houses against 15,000 others), split train/test *and* train/val, and scale, because unscaled inputs cause **divergence** - the weight updates get so big the network never finds its optimum. Then the single structural difference between the two models: **the activation on the output node.** Classification squishes the output between 0 and 1 with a sigmoid (you're predicting a probability); regression lets it run from negative to positive infinity (a linear activation). You can't change it in scikit-learn - *they put the guardrails on to keep you safe.*

Read the loss curve: 200 iterations were requested, but the model learned what it needed by roughly epoch 50. What you want is a **gentle downward curve** - a curve that plunges at iteration one and then flattens means something is wrong with your architecture. Then evaluate it like any classifier: classification report, weighted F1, confusion matrix. Dave's memory trick for the metrics - ***"R, row"*** - **recall runs across a row**, precision runs down a column. Worked live: recall for the 1 class is 385 / (385 + 131) ≈ 75%; precision is 385 / (385 + 73) ≈ 84%; recall for the 0 class is 1475 / (1475 + 73) ≈ 95%. The model is good at cheap houses and misses expensive ones. He then tunes batch size (1,000 → 100 → 10) and layer sizes live and **fails to beat 0.90** - *maybe that's the upper limit of what I'm trying to predict here.* Honest modeling on camera.

**Video 10 — Best practices for NN regression** *(7:39)*
The architecture advice, earned. Activations first: ReLU (0 to ∞), hyperbolic tangent (−1 to +1), logistic/sigmoid (0 to 1), identity (which is just a linear regression) - go Google the shapes. Then **batch size, timed live**: `batch_size=10` takes 20 seconds, `batch_size=1000` takes 3 - and the predictions are still good, because *when the model hits a bottleneck, it self-corrects during training.*

Then the rules of thumb for tabular data like California housing:
- **Don't use more than three hidden layers.** One might be enough. Start there.
- **Don't go bananas on hidden units.** People fire off 64, 128, 10 million because it's fun. But if you have eight inputs, how do you justify 200 learned intermediate features? One to two times the input size is plenty. (And remember: every input feeds every hidden unit, but random initialization means each unit becomes a *different* nonlinear combination.)
- **Treat the network as an information funnel.** The first hidden layer can be wide, then hold steady or narrow. Eight inputs → 10 → 20 → 1 is nonsensical design.
- **One hidden layer needs the trailing comma** in `hidden_layer_sizes=(10,)`, *or it'll freak out*.

It closes by handing off to the classifier: the only scikit-learn difference is `MLPRegressor` versus `MLPClassifier`, which changes the output node. Everything about training loss carries over - you just swap scatter plots for confusion matrices and RMSE for an entropy loss.

---

## A note on ordering

The recording dates on these videos are jumbled relative to the course order - the welcome video was recorded *last*, on December 22. The order above is the **teaching** order, and it's the order to watch them in. Videos 4 and 5 (the refreshers) were recorded alongside the Module 2 material and use the Boston Housing dataset; the neural network videos use California housing. Same methodology either way - that's the point.

---

*Companions: [Skills sheet](skills.md) (what you can do now) · [Talking points](talking_points.md) (the theory you should be able to explain) · [Working in this course](working_in_course.md) (how to run and save your work).*
