# Module 5 Video Guide — What Each Video Covers

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Twelve videos, about 1 hour 5 minutes total, in two blocks. Block 1 is the **classical** pipeline — clean the text yourself, turn words into columns, fit a model you can fully explain. Block 2 is **pretrained transformers** — summarization, zero-shot labeling, sentiment, and embeddings that turn a document into a fixed-length vector you can cluster.

Two datasets: **NOAA Storm Events 2019** (hail vs. flash flood narratives, for classification) and **BillSum** (California legislation, for summarization and topic modeling).

```{admonition} Why this module isn't just "ask an LLM"
:class: important
Dave makes the argument explicitly at the end of video 5: an LLM is *a little hands-off, and you don't necessarily understand how it operated.* Everything in block 1 gives you full control and full explainability — you can run permutation importance on a text model and name the exact words driving it. That's a thing many stakeholders will require of you.
```

---

## Block 1 · Classical text analytics with NLTK
**The big ideas:** corpus vs. document · the preprocessing ladder — lowercase → strip non-alphabetic → stop words → tokenize → stem · why preprocessing *is* dimensionality control · bag of words, n-grams, and TF-IDF · **fit the vectorizer on train only** · xAI on text.

**Video 1 — Intro to Text Analytics for ML/DL models with NLTK** *(2:59)*
The setup, with a shout-out to former student **Vinny Datla** who curated the data. **NLTK** — the Natural Language Toolkit — does the traditional work: casing, punctuation, stop words, stemming. The dataset is **Storm Events 2019**, with a long `episode_narrative` and a short `event_narrative`, and the task is predicting **event type** from the narrative text.

The EDA drives the design decision: thunderstorm wind dominates, then flood and hail, trailing off into wonderful rarities (*volcanic ashfall, freezing fog, and a "sneaker wave," whatever that is*). He picks **flash flood vs. hail** deliberately — two classes with **similar record counts** and genuinely distinct vocabulary. That choice is a modeling skill, not a detail.

**Video 2 — Intro to the Hail DF and basic preprocessing + word cloud** *(7:53)*
The vocabulary first: **a corpus is a collection of documents; a document is a single record** — so each narrative is a document, and the column is the corpus. And the goal of every step that follows: *simplify the number of tokens to cut down on computational power.*

Then the ladder, run live on 3,783 hail narratives. **Lowercase** (so `Hail` and `hail` stop being two things). **Strip anything non-alphabetic**, because otherwise `yard.` and `yard,` are different features. **Stop words** — *"train spotter reported hail up to the size of quarters"* becomes *"train spotter reported hail size quarters"*, which *reads like a telegram*.

Two observations to carry forward. The domain quirk: hail size is described in **currency and sports equipment** — quarter, penny, golf ball, ping-pong ball — so those words are predictive. And the leakage-adjacent judgment call: the word **"hail"** trivially predicts hail, so *if I wanted to generalize my model, I might take that word out* — you can put a word in the stop list precisely because it's too good.

**Video 3 — Tokenizer, and do the same thing for the flood_df** *(5:37)*
**Tokenizing** = building an index of every unique word across the corpus, turning each document into a list of tokens. This is where the earlier cleaning pays off: without lowercasing, `hail` and `Hail` become separate columns and *it would explode the dimensionality*.

Then **stemming**, which he names properly as **lexicon normalization** — *lexicon* meaning words, *normalization* meaning simplify. `reported → report`, `inches → inch`, and the two examples worth memorizing: *consultant / consulting / consulted → consult*, *manager / managerial / management → manage*. Note the ordering advice: **stem before tokenizing** if you want the full benefit.

The back half repeats the entire pipeline on `flood_df` (3,851 rows) — where the common words are water, flooding, road — which is really a lesson about **writing your cleaning once and applying it to both classes** rather than keeping scripts scattered.

**Video 4 — Bag of Words (BoW, unigrams, bigrams, trigrams)** *(6:26)*
Concatenate hail and flood into **7,634 rows** and model. **Bag of words asks only whether (and how often) a word appears.** A **unigram** is one column per unique word — *if you've taken deep learning, you'll know this as one-hot encoding*. A **bigram** is a two-word sliding window ("this is", "is a", "a sentence"), a **trigram** three; higher n captures more sentence structure at the cost of dimensionality.

The methodological point is the one to get right: **split first, then `fit_transform` the vectorizer on train and only `transform` validation and test.** *If a word in test doesn't exist in train, I can't make a feature for it; that would be data leakage.* Train yields **4,961 unique words**.

He then forces the **sparse matrix** to display so you can see what it really is — mostly zeros, whole-number counts, and no column names — before fitting a decision tree that separates the classes easily. Permutation importance (hacked down to 100 rows, `n_repeats=1`, because it's slow) confirms the obvious: **"hail" is the most important word**, followed by flood, water, river.

**Video 5 — TF-IDF and wrapping up** *(4:04)*
**TF-IDF = term frequency × inverse document frequency**, worked as arithmetic: a word appearing 3 times in a 100-word document has TF = 0.03; appearing in 1,000 of 10 million documents gives IDF = log(10,000,000 / 1,000) = 4; so the score is **0.03 × 4 = 0.12**. The effect is to **penalize words that are common everywhere**.

Practical difference: **bag of words gives whole numbers, TF-IDF gives fractions**, so summing a TF-IDF row is meaningless. The two models fit about equally well but rank features differently — hail/size/water vs. hail/water/creek — which is the same lesson as Module 2's "different models, different flavors of important."

His parting exercise: **add "hail" and "flood" to the stop words and see if you can force your model to fit differently.** Do it. A model that classifies storms without the giveaway words is the one that learned something.

---

## Block 2 · Pretrained transformers and embeddings
**The big ideas:** encoder-decoder transformers as task-specific tools · the model and its tokenizer are a matched pair · controlling input/output length and beams · zero-shot labeling vs. sentiment · a document as a **1×384 vector** · k-means on embeddings, auto-labeled with TF-IDF · PCA vs. UMAP.

**Video 6 — Intro to HuggingFace and Summarization** *(5:10)*
A deliberate step back in time: *away from large language models... back to a technology from the 2010s and early 2020s, before ChatGPT and Gemini.* **Transformers** take an input sequence and decode it into something useful — translation, cleanup, summarization — and people still use them because they're **interpretable** and run locally.

The warning that saves the most time: **models are task-specific.** *A lot of these models are specific to certain tasks... you can't mix and match.* Same for tokenizers — each model was trained with its own, so the tokenizer travels with the model. Dataset is **BillSum** (California laws: text, summary, title), using **BART** for summarization through a `pipeline`. It takes ~40 seconds and produces a fair summary of a veterans-organization tax exemption.

**Video 7 — Customizing the summarizer** *(5:03)*
*Anyone who takes my classes knows I like to know the shapes of things going in and out.* The knobs, turned one at a time so you see the effect: **max input length**, **max output length**, and **number of beams** (*more beams means more creative responses; fewer beams can get stuck in a local optimum*).

The results are the lesson. At 1,000 in / <100 out you get a decent summary that **drops the alcohol clause** — a real, quiet information loss. Squeeze the output to 20 words and you get *"it's a law that says which properties are exempt"* — true, and nearly useless. Drop to one beam and it just *gave the first two sentences*.

Why you'd tune this: standardized output length for a database field, and **compute** — *as the number of tokens increases, the computation can increase exponentially*. The use case comes from his insurance work: scrape a company's website and summarize **locally** to describe what the business does, instead of shipping it to an LLM — *this one wouldn't hallucinate as much, but the quality might not be as good.* That's the trade stated honestly.

**Video 8 — Zero-shot learning and sentiment analysis with HuggingFace** *(5:42)*
**Zero-shot classification**: supply candidate labels the model was never trained on and let it choose. He labels a bill *"good for veterans"* vs. *"bad for veterans"* and gets ~80% for good. Note the distinction he draws — **that is not sentiment**, it's asking the model to pick between labels you invented. And since dozens of zero-shot models exist, *you could even ensemble their predictions.*

**Sentiment**: a **DistilBERT** model returns 98% negative for a legal document — *maybe it's negative because it's a legal document*, i.e. treat the number skeptically. Then the better idea, from a graduate consulting project with **Forbes** on which content was stickiest: go past positive/negative to **emotions** with a RoBERTa model (anger, joy, sadness, optimism) — take the highest-scoring label and use it as a feature in a downstream model.

Ends with **PySpark** for scale: two ways to apply a locally downloaded HuggingFace model across a Spark dataframe (a UDF and a `collect`).

**Video 9 — Intro on Topic Modeling and Embedding** *(2:48)*
The conceptual core of the block, and it's short enough to rewatch. A transformer encodes **any** document — long or short — into a fixed **1×384** vector. That constant shape is everything: *if you can distill all this information into a constant shape, then your unstructured data takes on a structured format*, which means classification, regression, and clustering all become available.

And the honest answer about what those numbers mean: **nothing individually.** *You don't really care what they are — you just know you can exploit them for your machine learning task.*

**Video 10 — Creating Embeddings for Text Data** *(7:02)*
Embeddings in practice on 2,000 BillSum bills. First, don't be misled by the name: *even though it says "sentence transformer," it's really a document transformer.* The intuition for where embeddings come from is an **autoencoder** — text in, squeezed through a 384-unit hidden layer, regenerated on the far side; if it can rebuild itself, that middle layer holds the useful compression.

Then **k-means with k=5** on the vectors, and the clusters are legible: parks, energy/health, transportation, Medicare/medication/food, IRS. Two things worth stealing:

- **Auto-labeling clusters with TF-IDF.** Instead of "topic 0," take the top TF-IDF terms per cluster as the name. First pass is clunky, so he **adds stop words and re-runs the labeling** — and states clearly that *this isn't refitting the clusters at all*, only renaming them.
- **The pitfall.** *Just because you put something into a numeric representation doesn't mean you can turn off the autopilot.* Five topics may be wrong; try three, try eight, use domain knowledge and the scree plot, and look at whether the groups make sense to a human.

**Video 11 — Simple visualizations of 1x384 embeddings** *(5:25)*
*Seeing is believing* — and these are, in his words, plots *you can steal and use on the job.* Render an embedding as a **1-D heat strip** of 384 colored cells, then stack 10 documents per cluster with red dividers. The clusters show up as **shared color patterns** — solid blue here, strong yellow around dimension ~140 — because similar vectors sit near each other in 384-space, *which means a natural grouping, which means they're related.*

He also concedes the method's limits in the same breath: *of course, squinting isn't the way to make clusters* — it's a way to *see* that the clustering is real. And both the 384 and the 5 are assumptions, not facts.

**Video 12 — From 1x384 to 1x2 with PCA and UMAP** *(5:35)*
Getting 384 dimensions onto a slide. **PCA** is the linear route you already know (`n_components=2`). **UMAP** is nonlinear, *trained specifically for embeddings*, and preserves **local** structure.

The comparison is made on a practical criterion rather than theory: if you had to draw a boundary between clusters, PCA is *kind of a shotgun*, with brown overlapping badly, while with UMAP *you can thread a needle between the points*. Then he ties it back — the same documents, shown as 384-cell strips *and* as 2-D points, so you can see why two "solidly blue" bills land next to each other.

Closing philosophy, and a good one to end a course on: **"playing with your food"** — *it's good to crack things open and understand how they work.*

---

## Where this goes next

Module 5 completes the set: Module 2 gave you honest evaluation and explanation, Module 3 put a model in production, Module 4 handled time, and Module 5 handles text. The **A10 assignment** (FDA food recalls) deliberately fuses Module 5 with Module 2 — an imbalanced, three-class text problem where the job is to beat the majority-class baseline, prepared **two ways** (TF-IDF/BoW *and* padded sequences), on a 90/10 split with `random_state=42` so everyone's numbers are comparable.
