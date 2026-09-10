# Preface

:::{admonition} ⚠️ Work in progress
:class: warning
These materials are a **living draft** — actively being written, revised, and expanded from my lecture transcripts and course notebooks. Expect rough edges, gaps, and changes between visits. This is a teaching companion, **not a final or official reference**. Spot something off? That's expected — it's a work in progress!
:::

Welcome to **Applied Data Science**, the book edition of **OPIM 5512** at the University of Connecticut.

Most data-science courses stop at "the model works in my notebook." This one doesn't. The throughline of this book is **production**: you'll build models you can *trust* and *explain*, deploy them to the cloud where they run on a schedule and update themselves, and apply them to the messy data types the real world actually hands you — imbalanced tables, time series, and unstructured text. The voice is casual; the engineering is real; everything is code-first and runs.

## Who this book is for

You arrive with solid Python and a working knowledge of machine learning — you can fit a model in scikit-learn and read a confusion matrix. We build everything else: professional Git workflows, tuning and explainability, cloud deployment, and modern NLP.

## How the book is organized

```{tableofcontents}
```

- **Chapter 1 — Tool Time.** GitHub, a real dev environment (VS Code, Git, GCP), and an ML refresher — including neural networks in scikit-learn.
- **Chapter 2 — Tuning Models & Explainable AI.** Imbalanced data and sampling, cross-validation, pipelines and grid search, and the interpretability tools (permutation importance, partial dependence) that let you *explain* a model.
- **Chapter 3 — Webscraping & Operational ML.** Deploy code to Google Cloud with GitHub Actions, scrape unstructured data, and turn raw text into structured features with **both** regular expressions and **generative AI**.
- **Chapter 4 — Time Series Modeling.** The window method, automated feature engineering with TSFresh, and anomaly detection.
- **Chapter 5 — Text Analytics.** Classic NLP (bag-of-words, TF-IDF), Hugging Face transformers, and text embeddings for topic modeling.

## How to read it

Read with a notebook open and a GitHub account ready. The recurring theme — and the thing employers actually pay for — is **reproducibility**: clean Git history, scaled-on-train pipelines, models that re-run on a schedule and prove their accuracy over time. All code is drawn from the course notebooks on [`drdave-teaching`](https://github.com/drdave-teaching).

Let's productionize some data science.

— *Dave*
