# Chapter 1 — Tool Time

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
