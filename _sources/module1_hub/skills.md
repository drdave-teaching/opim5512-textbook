# Module 1 Skills Sheet — What You Can Do Now

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

After the Module 1 videos, notebooks, and two assignments, these are the skills you own. Not "watched a video about" - *own*. Every one is something you did with your own hands, and every one shows up in real data science jobs. Go down the list and check them off; if one feels shaky, the notebook or video to revisit is listed right there.

---

## 🐙 GitHub - the professional loop
*Videos 2-3 · Notebook: `Module1/SettingUpTechEnviro.ipynb` · Assignments A01, A02*

- ☐ Explain the four core concepts - **repository, commit, branch, pull request** - without looking them up
- ☐ Create the **special profile repo** named after your username and make the landing page worth reading
- ☐ Clone a repo with **GitHub Desktop** and tell at a glance which branch you're on
- ☐ Create a branch, publish it, and explain why you don't work on `main`
- ☐ Commit with a message that says what changed and why (*"data loading - read the CA housing dataset from sklearn"*, not *"update"*)
- ☐ Open a **pull request**, assign a **reviewer**, and read the "pending review / blocked" state correctly
- ☐ **Review** someone else's PR: open **Files changed**, and choose between *Approve*, *Comment*, and *Request changes* on purpose
- ☐ Merge a PR and **delete the branch** afterward
- ☐ Add a **collaborator** to a repo (Settings → Collaborators)
- ☐ Set a **branch protection ruleset** - require a PR plus one approval before merging - and scope it to the **default branch only** (scoping it to *all* branches blocks your own work; that's the bug in video 3)
- ☐ Read a **network graph** and describe the back-and-forth it shows

## 💻 The development environment
*Video 2 · Notebooks: `Module1/SettingUpTechEnviro.ipynb` · `Module1/Shell_(Bash)_commands.ipynb`*

- ☐ Get the stack running end to end: **GitHub, GitHub Desktop, VS Code, Python**
- ☐ Lay out a repo like a professional: `source/` for code, `figures/` for output, `README.md`, `requirements.txt`
- ☐ Create a file at the **repo root** instead of accidentally nesting it in a folder
- ☐ Read VS Code's change colors - **green means new, yellow means modified**
- ☐ Write a `requirements.txt` that lists **every package you import**
- ☐ Navigate a terminal with `pwd`, `cd`, and `ls`, and know where your repo actually lives on disk
- ☐ Run `pip install -r requirements.txt`, then `python source/your_script.py`, from the repo
- ☐ Explain why a script that only runs on your laptop is not a deliverable

## 🔬 The machine learning methodology
*Videos 4-5 · Notebooks: `Module1/DTR_RFR_GBR_BostonHousing.ipynb` · `Module1/DTC_RFC_GBC_BostonHousing.ipynb` · `Module1/M1_TreeModels_CAHousing.ipynb`*

- ☐ Run the fixed loop on any tabular dataset: **read → split → scale (fit on train) → fit → evaluate**
- ☐ Check `.shape` and `.info()` before you model, and confirm your X/y split adds back up
- ☐ Split with `train_test_split` and hold out a real test partition (and a validation partition when you need one)
- ☐ Scale with `MinMaxScaler` - **`fit_transform` on train, `transform` on test** - and explain exactly what leaks if you don't
- ☐ Explain the interpretability bonus of a 0-1 scale (a 0.9 means 90% of observations sit below it)
- ☐ Recognize that scaled output is a **NumPy array**, not a DataFrame, and that a test value slightly above 1.0 is expected

## 🌳 Fitting and comparing models
*Videos 4-5 · Same notebooks · Scripts: `OPIM5512-labs/Module1/Week2_TreeModels/*.py`*

- ☐ Fit the regression lineup: **linear regression, decision tree, random forest, gradient boosting**
- ☐ Fit the classification lineup: **logistic regression, decision tree, random forest, gradient boosting** classifiers
- ☐ Predict why a default decision tree overfits (minimum leaf size of one) and calm it with `min_samples_split`
- ☐ Report **R², MAE, and MSE for both train and test** - never test alone, never train alone
- ☐ Compute **bias** (the mean of the errors) and say whether your model systematically over- or under-predicts
- ☐ Build predicted-versus-actual **scatter plots with a 45-degree line**, for train and test, and read them
- ☐ Recognize the failure mode: a flat line means your model is predicting the mean and learned **nothing**
- ☐ Build a **confusion matrix** for train and test, and name all four cells
- ☐ Read a **classification report** - precision, recall, F1 per class, plus the **weighted F1** for comparing models
- ☐ Use ***"R, row"*** to keep recall (across the row) and precision (down the column) straight
- ☐ Pick a winning model and defend the choice with the evidence in front of you, not a vibe

## 🧠 Neural networks in scikit-learn
*Videos 6-10 · Notebooks: `Module1/Week2_1_NN_Regression` · `Module1/Week2_1_NN_Classification`*

- ☐ Describe a neural network as a **nonlinear weighted sum**: 1×8 input → 1×10 → 1×5 → 1×1 output
- ☐ Print the shapes of the learned weight matrices and explain what each one does
- ☐ Trace a **forward pass**, compute the **feedback signal** (actual minus predicted), and say which way the weights move
- ☐ Explain **backpropagation** in two sentences
- ☐ Name the three flavors of gradient descent - **stochastic, full, batch** - by how many rows pass before an update
- ☐ Define an **epoch** correctly (one weight-update pass across the dataset, however you chunk it)
- ☐ Locate the nonlinearity: **activation functions between layers**, and know that without them you have linear regression
- ☐ Explain **ReLU** - positive in, positive out; negative in, zero out - and sketch tanh, sigmoid, and identity
- ☐ Justify why **scaling is mandatory** for a neural net (unscaled inputs cause divergence) when it's optional for trees
- ☐ Fit an **`MLPRegressor`** with `hidden_layer_sizes`, `activation`, `max_iter`, `batch_size`, and `early_stopping`
- ☐ Explain what **early stopping** holds out (20% of *training* data) and confirm that val and test never touch the fit
- ☐ Fit an **`MLPClassifier`** and name the one structural difference: the **activation on the output node** (sigmoid for classification, linear for regression)
- ☐ Read a **training loss curve** - a gentle downward slope is healthy; a cliff at iteration one then nothing is a red flag
- ☐ Trade `batch_size` against runtime and know why bigger batches finish faster
- ☐ Apply the architecture rules of thumb: **no more than three hidden layers**, units at **1-2× the input count**, and a **funnel** (never widen after narrowing)
- ☐ Remember the trailing comma in `hidden_layer_sizes=(10,)`
- ☐ Evaluate a neural net like any other model - metrics and plots, not the loss curve alone

---

## The one-sentence version

> **You can set up a professional environment, collaborate through reviewed pull requests the way industry actually does, and take any tabular dataset from raw file to a defensible, honestly-evaluated model - including a neural network - without ever leaking test data into your training.**

That's not a homework skill. That's the job.
