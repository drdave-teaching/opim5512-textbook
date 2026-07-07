# Chapter 5 — Text Analytics

:::{admonition} 🔗 Notebooks for this chapter
:class: seealso dropdown
Open in Colab and **Runtime → Run all** — data loads from a stable link, nothing to upload.

- **NLP Pandas Hugging Face (2)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/NLP_Pandas_HuggingFace%20(2).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/NLP_Pandas_HuggingFace%20(2).ipynb)
- **Text Analytics Storms ML** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/Text_Analytics_Storms_ML.ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/Text_Analytics_Storms_ML.ipynb)
- **Topic Modeling (1)** &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/TopicModeling%20(1).ipynb) &nbsp; [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module5/TopicModeling%20(1).ipynb)
:::


The last frontier is **unstructured text** — the reviews, tickets, and documents that don't fit in a spreadsheet. This chapter takes you from classic NLP pipelines you can build by hand to state-of-the-art **pretrained transformers** you can run in a few lines, and finishes with **embeddings** that reveal hidden structure in a pile of documents.

## 5.1 From text to numbers

A model can't read words, only numbers, so we **preprocess** first — lowercase, strip punctuation and non-letters, remove stop words, optionally **stem/tokenize**. Then we vectorize. The simplest representation is **bag-of-words** (count each word), and its smarter cousin is **TF-IDF**, which up-weights words that are distinctive to a document and down-weights words common to all of them:

$$
\text{tfidf}(t,d) = \text{tf}(t,d)\,\times\,\log\frac{N}{1+\text{df}(t)}
$$

```python
from sklearn.feature_extraction.text import TfidfVectorizer
tfidf = TfidfVectorizer(ngram_range=(1, 2), max_features=10000)
X = tfidf.fit_transform(train_text)     # fit on TRAIN only (the rule never changes)
```

**n-grams** (`ngram_range`) capture short phrases — "not good" is very different from "good" — but every added gram multiplies the columns, so watch the **dimensionality** (you can end up with more columns than rows). A TF-IDF matrix feeds straight into any classifier from Chapter 1; it's a strong, cheap baseline.

## 5.2 Pretrained transformers with Hugging Face

Between brittle regex and a full LLM API sits a sweet spot: the **encoder/decoder transformer**, downloadable from **Hugging Face** and run **locally** — a big win for **data privacy** (your text never leaves your machine). With a few lines you get capabilities that used to be research projects:

```python
from transformers import pipeline

summarizer = pipeline("summarization")
summarizer(long_document, max_length=60)                      # document summarization

classifier = pipeline("zero-shot-classification")             # no training data needed!
classifier("The flight was delayed three hours.",
           candidate_labels=["complaint", "praise", "question"])

emotion = pipeline("text-classification", model="...emotion...")
emotion("I can't believe how great this was!")               # emotion detection
```

```{admonition} Zero-shot is the magic word
:class: tip
**Zero-shot classification** labels text into categories you invent **at inference time**, with **no training examples**. Hand the model your candidate labels and a sentence, and it scores them. For a huge class of "just sort these comments into buckets" problems, this replaces an entire labeling-and-training project with three lines.
```

## 5.3 Embeddings and topic modeling

The deepest representation is the **embedding**: a pretrained model maps each document to a dense vector (e.g. **1×384**) where **semantic similarity = geometric closeness**. Documents about the same topic land near each other, even if they share no exact words. Similarity is the **cosine** of the angle between vectors:

$$
\text{sim}(\mathbf{a},\mathbf{b}) = \frac{\mathbf{a}\cdot\mathbf{b}}{\lVert\mathbf{a}\rVert\,\lVert\mathbf{b}\rVert}
$$

With embeddings you can do **unsupervised topic modeling**: cluster the document vectors with **k-means** to discover themes nobody labeled, then **reduce dimensions** for visualization. A 384-dimensional vector is impossible to plot, so project it to 2-D with **PCA** (linear) or **UMAP** (nonlinear, better at preserving local neighborhoods):

```python
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA
import umap

labels = KMeans(n_clusters=8, random_state=42).fit_predict(embeddings)   # topics
coords = PCA(n_components=2).fit_transform(embeddings)                    # or umap.UMAP()
# scatter coords, colored by label -> a map of your document collection
```

The payoff is a **map of your corpus** — clusters you can read as topics, outliers you can investigate, structure you never had to label. Simple word counts have evolved into genuinely semantic models, and you can now point them at real problems.

## Wrap-up

```{admonition} Key takeaways
:class: tip
- Preprocess text, then vectorize with **bag-of-words / TF-IDF**; **n-grams** add phrase context but inflate dimensionality.
- **Hugging Face** transformers run **locally** (privacy) and do **summarization, zero-shot classification, and emotion detection** in a few lines.
- **Zero-shot** classifies into labels you invent at inference time — no training data.
- **Embeddings** place documents in a semantic space (similarity = **cosine**); **k-means** finds topics and **PCA/UMAP** visualize them.
- The arc of the chapter: **counts → TF-IDF → pretrained transformers → embeddings** — simple representations evolving into semantic ones.
```


---

## 📌 Lecture key points

*Distilled takeaways from the video lectures behind this chapter — click each to expand.*


:::{admonition} Intro to Text Analytics for ML/DL models with NLTK
:class: note dropdown
- Text must become numbers; **NLTK** for classic NLP.
- Leverage frequency/occurrence/co-occurrence of words.
- BoW → TF-IDF progression.
- Corpus vs document.
- Foundation for A10.
:::

:::{admonition} Intro to the Hail DF + preprocessing + word cloud
:class: note dropdown
- Preprocess: **lowercase**, strip funky characters.
- Make a **word cloud** for quick EDA.
- Storm (hail) narratives as the corpus.
- Reusable script (name things generically).
- Set up for tokenizing/vectorizing.
:::

:::{admonition} Tokenizer (and the flood_df)
:class: note dropdown
- **Tokenize** text into indexed words.
- Remove stop words; optional stemming (when too many words).
- Reapply the same pipeline to a second dataset (flood_df).
- Careful, consistent naming = reusable code.
- Feeds BoW/TF-IDF.
:::

:::{admonition} Bag of Words
:class: note dropdown
- **BoW** counts word occurrences (order-free).
- `CountVectorizer` builds the matrix.
- Experiment with **unigrams/bigrams/trigrams**.
- Larger n → bigger feature space (**dimensionality** risk: more columns than rows).
- Strong, fast baseline (note: BoW taught **before** TF-IDF).
:::

:::{admonition} TF-IDF and wrapping up
:class: note dropdown
- **TF-IDF** up-weights distinctive words, down-weights ubiquitous ones.
- Feed into any classifier; **fit on train only**.
- n-grams add phrase context at a dimensionality cost.
- Compare BoW vs TF-IDF.
- Closes the classic-NLP thread.
:::

:::{admonition} Intro to HuggingFace and Summarization
:class: note dropdown
- **Hugging Face** pretrained transformers run **locally** (data-privacy win).
- `pipeline("summarization")` summarizes documents in a few lines.
- Encoder/decoder transformers = the sweet spot between regex and full LLMs.
- Require input before generating.
- State-of-the-art with minimal code.
:::

:::{admonition} Customizing the summarizer
:class: note dropdown
- Tune `max_length`/parameters for better summaries.
- Pick the right pretrained model.
- Evaluate summary quality.
- Run on your dataset locally.
- Practical inference tuning.
:::

:::{admonition} Zero-shot learning and sentiment analysis with HuggingFace
:class: note dropdown
- **Zero-shot classification** = labels you invent at inference time, **no training data**.
- Hand the model candidate labels + text; it scores them.
- Sentiment/emotion detection in a few lines.
- Replaces a whole label-and-train project for many tasks.
- Powerful for "sort these comments into buckets."
:::

:::{admonition} Intro on Topic Modeling and Embedding
:class: note dropdown
- **Embedding** = dense vector where **semantic similarity = geometric closeness**.
- Topic modeling = **k-means cluster** the document vectors (unsupervised).
- Discover themes nobody labeled.
- Similarity via **cosine**.
- Embeddings beat counts for meaning.
:::

:::{admonition} Creating Embeddings for Text Data
:class: note dropdown
- Use a pretrained model to map each document to a vector (e.g., **1×384**).
- Documents about the same topic land near each other.
- Even with no shared exact words.
- Feed embeddings to clustering/classification.
- The deepest text representation in the course.
:::

:::{admonition} Simple visualizations of 1×384 embeddings
:class: note dropdown
- Raw 384-D vectors can't be plotted directly.
- Visualize structure/clusters qualitatively.
- Motivates dimensionality reduction.
- Inspect outliers and groupings.
- Bridge to PCA/UMAP.
:::

:::{admonition} From 1×384 to 1×2 with PCA and UMAP
:class: note dropdown
- **PCA** (linear) and **UMAP** (nonlinear) project embeddings to **2-D** for plotting.
- UMAP better preserves local neighborhoods.
- Scatter colored by k-means label = a **map of your corpus**.
- Reveal topics, outliers, structure without labels.
- Counts → TF-IDF → transformers → embeddings: representations grow semantic.
:::
