# Chapter 5 — Text Analytics

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
