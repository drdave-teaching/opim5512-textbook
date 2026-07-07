# Chapter 1 — Tool Time

:::{admonition} 🔗 Notebooks for this chapter
:class: seealso dropdown
Open in Colab and **Runtime → Run all** — data loads from a stable link, nothing to upload.

- **DTC RFC GBC Boston Housing** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTC_RFC_GBC_BostonHousing.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTC_RFC_GBC_BostonHousing.ipynb)
- **DTR RFR GBR Boston Housing** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTR_RFR_GBR_BostonHousing.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTR_RFR_GBR_BostonHousing.ipynb)
- **Setting Up Tech Enviro** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/SettingUpTechEnviro.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/SettingUpTechEnviro.ipynb)
- **Shell (Bash) commands** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Shell_(Bash)_commands.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Shell_(Bash)_commands.ipynb)
- **Week2 1 NN Classification (2)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Classification%20(2).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Classification%20(2).ipynb)
- **Week2 1 NN Regression (2)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Regression%20(2).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Regression%20(2).ipynb)
- **M1 Tree Models CA Housing** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/M1_TreeModels_CAHousing.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/M1_TreeModels_CAHousing.ipynb)
:::


Before we model anything in production, we set up the workshop. This chapter is about the tools real data scientists use every day — **GitHub** and a proper **development environment** — plus a fast refresher on the machine learning we'll build on. The framing matters: your GitHub is your **face to the technical world**. Employers want to hire you because they can *see how good your code is*.

## 1.1 GitHub — your professional home

Four concepts cover 90% of daily Git:

- **Repository** — a project folder under version control.
- **Commit** — a saved snapshot with a message; your project's history.
- **Branch** — a parallel line of work you can develop and discard safely.
- **Pull request (PR)** — propose merging a branch, get it **reviewed**, then merge.

```{admonition} The "Ping-Pong" workflow — how industry actually collaborates
:class: important
The professional loop, practiced until it's reflex: create a repo and add a collaborator → make a **branch** → edit and **commit** → open a **pull request** with your partner as **reviewer** → they review, approve, and **merge**, then **delete the branch**. Then you swap roles and they send *you* a PR. Back and forth — ping-pong — and the **network graph** shows the loop. Branch protection (require a PR + one approval before merging `main`) enforces that nobody pushes straight to `main`. This is the single most employable skill in the chapter.
```

## 1.2 The development environment

We graduate from notebooks-only to a real stack: **VS Code** (editor), **Git/GitHub Desktop** (version control), **Google Cloud Platform** (where models will run), and **OpenAI** (for the GenAI work later). Set it up once, organize your repo with a `source/` folder, a `figures/` folder, a `README`, and — critically — a **`requirements.txt`** so anyone (including a cloud runner) can reproduce your environment. Reproducibility isn't bureaucracy; it's what makes a model *deployable*.

## 1.3 ML refresher — regression and classification

The methodology is fixed regardless of model: **read → split → scale (fit on train only) → fit → evaluate**. Regression uses $R^2$/MAE/RMSE; classification uses the confusion-matrix family (accuracy, precision, recall, F1). Keep one habit sacred:

```{admonition} Fit transforms on TRAIN only
:class: warning
Scalers, encoders, vectorizers — **fit on the training partition, then `transform` validation and test.** Fitting on the full dataset leaks information and inflates your scores. Everything in this book — pipelines, cross-validation, deployment — is built to protect this boundary.
```

## 1.4 Neural networks in scikit-learn

You don't need TensorFlow to start with neural nets. scikit-learn's **`MLPRegressor`** and **`MLPClassifier`** (multi-layer perceptrons) are perfect for learning the theory and the knobs without leaving the sklearn world:

```python
from sklearn.neural_network import MLPRegressor
nn = MLPRegressor(hidden_layer_sizes=(64, 64),   # two hidden layers of 64 units
                  activation='relu',
                  early_stopping=True, random_state=42)
nn.fit(X_train, y_train)
preds = nn.predict(X_test)
```

A neural network is, at heart, **gradient descent with backpropagation**: it makes a prediction, measures the error, and nudges its weights to do better next time. The hyperparameters that matter — `hidden_layer_sizes`, `activation`, learning rate, early stopping — are exactly the ones you'll tune systematically in Chapter 2. (The course capstone for this material is the **"Ping-Pong" assignment**: collaborate via PRs while fitting an `MLP` on California housing — tools and modeling in one exercise.)

## Wrap-up

```{admonition} Key takeaways
:class: tip
- **GitHub** = repos, commits, branches, PRs; the **ping-pong** PR/review loop is how teams really work — and your most employable skill.
- Build a real **environment** (VS Code, Git, GCP, OpenAI) and always ship a **`requirements.txt`** for reproducibility.
- The ML methodology never changes: **read → split → scale-on-train → fit → evaluate**.
- **`MLPRegressor`/`MLPClassifier`** bring neural nets into scikit-learn; their knobs are Chapter 2's tuning targets.
```


---

## 📌 Lecture key points

*Distilled takeaways from the video lectures behind this chapter — click each to expand.*


:::{admonition} ML Regression Refresher
:class: note dropdown
- The fixed methodology: read → split → **scale (fit on train)** → fit → evaluate.
- Regression metrics: **R², MAE, RMSE** (RMSE punishes big misses).
- Baseline (linear) + tree models; fitting is ~3 lines.
- Evaluate quantitatively **and** visually (predicted-vs-actual).
- Reused as the A01/A02 prep.
:::

:::{admonition} ML Classification Refresher
:class: note dropdown
- Same workflow; the **evaluation** differs.
- Metrics from the **confusion matrix**: accuracy, precision, recall, F1.
- Baseline = **logistic regression**.
- Mind class balance for honest metrics.
- Bridges into neural-net classification.
:::

:::{admonition} Intro to NNs for regression with sklearn
:class: note dropdown
- Neural nets are available in **scikit-learn** (`MLPRegressor`) — no TensorFlow needed to start.
- An NN is a nonlinear, flexible "super-model."
- Pay attention to the **hyperparameters/keyword arguments**.
- Same fit/predict API as any sklearn model.
- Primary engine for the **A02 Ping-Pong** assignment.
:::

:::{admonition} Gradient descent and backpropagation
:class: note dropdown
- A network **predicts, measures error, and nudges weights** to do better next time.
- **Backpropagation** propagates the error to update each weight.
- Learning rate controls step size (overshoot vs crawl).
- The theory behind every `MLP` fit.
- Iterated over epochs/batches.
:::

:::{admonition} Fit a NN (MLPRegressor) with sklearn
:class: note dropdown
- Build with `hidden_layer_sizes`, `activation`, `early_stopping`.
- Fit/predict like any estimator.
- Watch for overfitting; use early stopping.
- Tune the same knobs you grid-search in Module 2.
- California-housing style regression target.
:::

:::{admonition} MLPClassifier
:class: note dropdown
- `MLPClassifier` for classification targets.
- Output handles binary/multiclass; evaluate with confusion-matrix metrics.
- Same hyperparameters as the regressor.
- Limited vs TensorFlow/PyTorch but perfect for theory + implementation.
- Completes the sklearn-NN toolkit.
:::

:::{admonition} Best practices for NN regression
:class: note dropdown
- "How many layers / hidden units?" → it's a **grid search**.
- Several architecture strategies (units = #features, 2×features, deeper/wider).
- Different architectures can reach similar fits (random init).
- Start simple, then tune.
- Don't over-engineer the first model.
:::

:::{admonition} A nice intro to A01 (making your first repos)
:class: note dropdown
- The **"hello world"** of GitHub: make your first repos.
- Core GitHub functions: repositories, commits, branches, pull requests.
- Set up the **tech stack** (GitHub/GitHub Desktop, VS Code).
- Refresh ML fundamentals, then try it yourself.
- Preps the A01 individual assignment + M1 quiz.
:::

:::{admonition} A02 "Ping Pong" demo (Dave & Rohit)
:class: note dropdown
- The pro **collaboration loop**: branch → commit → **pull request** → review/approve → merge → delete branch.
- Add a **collaborator**; set **branch protection** (require a PR + 1 review).
- Live debugging: a branch-protection misconfig blocked the commit (real-world lesson).
- Fit a neural net on **California housing**; add a `requirements.txt` for reproducibility.
- Do it **≥ 5 times** back-and-forth; the **network graph** shows the loop.
:::

:::{admonition} Hi from Dr. Dave
:class: note dropdown
- Course welcome/orientation.
- Framing: GitHub is your **face to the tech world** (employers see your code).
- Sets expectations and tone.
- Points to the tools and workflow ahead.
- Motivation for the production focus.
:::
