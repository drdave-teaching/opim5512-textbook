# OPIM 5512 — *Applied Data Science* (textbook)

A casual-but-rigorous, code-first **applied data-science textbook** built from Dr. Wanik's OPIM 5512
lecture transcripts and course notebooks.

## View it (local, nothing public)
```
C:\Users\dww05002\kaltura\OPIM5512-textbook\_build\html\index.html
```

## Rebuild after edits
```bash
python -c "import sys; from jupyter_book.cli.main import main; sys.argv=['jupyter-book','build','.']; main()"
```

## Chapters
1. **Tool Time** — GitHub, dev environment (VS Code/Git/GCP/OpenAI), ML refresher, NNs in sklearn
2. **Tuning Models & Explainable AI** — imbalanced/sampling, cross-validation, pipelines+GridSearch, permutation importance, PDPs, autoML
3. **Webscraping & Operational ML** — GCP deploy via GitHub Actions, RegEx ETL, GenAI/Gemini ETL, scheduled cloud models
4. **Time Series Modeling** — window method, TSFresh, anomaly detection
5. **Text Analytics** — NLP/BoW/TF-IDF, Hugging Face transformers, embeddings & topic modeling (PCA/UMAP)

Weaves transcript narrative + real code (from `OPIM5512`, pipelines/sklearn/HuggingFace) + math callouts.
A `.github/workflows/deploy.yml` is included to publish to GitHub Pages (public repo) when ready.
**Kept LOCAL per request.** Sources: `opim5512-transcripts/scripts` (71), code `OPIM5512` (40 nb).
