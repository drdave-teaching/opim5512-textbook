# Module 5 Skills Sheet — What You Can Do Now

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Two complete toolkits for text: the classical pipeline you control end to end, and the pretrained models you can point at a problem in an afternoon. You should be able to argue for either one in a meeting.

---

## 🧽 Text preprocessing with NLTK
*Videos 1-3 · Notebooks: Storm Events 2019 · Assignment A10*

- ☐ Use the vocabulary correctly: a **corpus** is a collection of documents, a **document** is one record
- ☐ State the purpose of preprocessing in modeling terms — **fewer tokens, lower dimensionality, less compute**
- ☐ Lowercase an entire text column
- ☐ Strip non-alphabetic characters, and explain why `yard.` and `yard,` must not be separate features
- ☐ Remove **stop words**, and **customize the list** — including removing a word that's *too* predictive
- ☐ **Tokenize** a document into a list of tokens
- ☐ Explain why lowercasing must happen **before** tokenizing
- ☐ Apply **stemming** and name it properly as **lexicon normalization** (*consult/consulting/consulted → consult*)
- ☐ Sequence it correctly: stem **before** tokenizing
- ☐ Count word frequencies across a corpus and build a **word cloud**
- ☐ Read domain signal out of the vocabulary (hail is measured in **quarters, pennies, and golf balls**)
- ☐ Choose a modeling problem deliberately — two classes with **similar counts** and distinct vocabulary
- ☐ Write the cleaning **once** and apply it to every class rather than scattering scripts

## 🔢 Turning words into columns
*Videos 4-5 · Notebooks: BoW / TF-IDF · Assignment A10*

- ☐ Explain **bag of words** in one sentence and connect it to **one-hot encoding**
- ☐ Generate **unigrams, bigrams, and trigrams**, and say what each buys and costs
- ☐ **Split first, then `fit_transform` on train and `transform` on validation/test** — and explain the leakage if you don't
- ☐ Explain why a word appearing only in test cannot become a feature
- ☐ Recognize a **sparse matrix**, know why it's stored that way, and force it to dense only to inspect it
- ☐ Recover **feature names** so importance output is readable
- ☐ Compute **TF-IDF by hand** for one word: TF = count / doc length, IDF = log(N / docs containing it), score = TF × IDF
- ☐ Say what TF-IDF is *for* — penalizing words that are common everywhere
- ☐ Know the practical tell: **BoW gives whole numbers, TF-IDF gives fractions**, so summing a TF-IDF row is meaningless
- ☐ Fit a classifier on text features and evaluate with a **confusion matrix**
- ☐ Run **permutation importance on a text model** and name the exact words driving it
- ☐ Sanity-check a suspiciously easy result, and **stop-word the giveaway terms** to force the model to learn something harder

## 🤗 Pretrained transformers
*Videos 6-8 · Notebooks: BillSum / HuggingFace*

- ☐ Explain what an **encoder-decoder transformer** does, and name three tasks it's used for
- ☐ Find a model on **Hugging Face** and check what task it was trained for
- ☐ State the rule that saves the most time: **models are task-specific, and the tokenizer travels with the model**
- ☐ Run a summarization `pipeline` end to end
- ☐ Tune **max input length**, **max output length**, and **number of beams**, and describe what each changed
- ☐ Notice **information loss** in a shorter summary rather than accepting it silently
- ☐ Justify a length limit on grounds of a downstream schema or compute cost
- ☐ Run **zero-shot classification** with your own candidate labels, and explain why that is **not** sentiment
- ☐ Run **sentiment** and **multi-emotion** classification, and take the top-scoring label as a feature
- ☐ Read a sentiment score skeptically in a domain where the register is inherently negative
- ☐ Argue **local transformer vs. LLM API**: privacy, cost, hallucination risk, and quality — honestly, in both directions
- ☐ Apply a local HuggingFace model across a **PySpark** dataframe

## 🧭 Embeddings and topic modeling
*Videos 9-12 · Notebooks: BillSum embeddings*

- ☐ Explain the core move: **any document → a fixed 1×384 vector**, so unstructured data becomes structured
- ☐ Say what an individual embedding dimension means (**nothing on its own**) and why that's fine
- ☐ Sketch the **autoencoder intuition** for where an embedding comes from
- ☐ Know that a "sentence transformer" is really a **document** transformer
- ☐ Cluster embeddings with **k-means** and inspect example documents per cluster
- ☐ **Auto-label clusters with TF-IDF** top terms, and improve the labels by adding stop words
- ☐ State clearly that relabeling **does not refit** the clusters
- ☐ Challenge `k`: try 3, try 8, use a scree plot *and* domain judgment — *don't turn off the autopilot*
- ☐ Visualize an embedding as a **1-D heat strip** and read shared patterns within a cluster
- ☐ Reduce 384 → 2 with **PCA** and with **UMAP**
- ☐ Compare them on a practical criterion: could you draw a boundary between the clusters?
- ☐ Explain why UMAP suits embeddings (nonlinear, preserves **local** structure)
- ☐ Treat both 384 and k as **assumptions you chose**, not facts

---

## The one-sentence version

**You can take a pile of raw text and either engineer it into columns you fully understand and can explain word by word, or hand it to a pretrained model for summaries, labels, and embeddings — and you can say out loud which approach fits the problem and why.**

The A10 food-recalls assignment asks for exactly this, fused with Module 2: an imbalanced multi-class text problem, prepared **two ways**, that has to beat a majority-class baseline.
