# Lab 1 — Data ER + First Commit

**Hartford: Wed Sep 2 · Stamford: Wed Sep 9 · 5:30-7:30 PM**

The in-person half of Module 1. The async carried the mechanics; the lab is where you apply them to a dataset nobody pre-chewed for you, and then push the result through a **reviewed pull request** - the loop you'll use for everything else this semester.

Bring a laptop with **Colab** working and **VS Code + GitHub Desktop** installed. If your stack isn't running yet, come anyway - the first twenty minutes exist for exactly that.

---

## What we're doing

| | Part | Time | Working |
|---|---|---|---|
| 0 | **Triage the stack** | ~20 min | solo, with help |
| 1 | **Data ER** | ~45 min | pairs |
| 2 | **First Commit** | ~45 min | same pairs |
| 3 | **Rounds** | ~10 min | everyone |

## Part 0 · Triage the stack (~20 min)

Everyone runs the same three checks in a terminal and gets a green light before we start:

```bash
git --version
python --version
```

Then clone any repo with GitHub Desktop, open it in VS Code, and make one commit. If all three work, help the person next to you - that's the fastest way to a room where everybody's environment is running. If you're stuck, flag me.

## Part 1 · Data ER (~45 min, pairs)

A messy dataset just rolled into your emergency room. Unlike the notebooks, **nobody tells you what's broken**. Your job is triage, treatment, and a discharge report.

**Triage** - before you fix anything, write down what's wrong. `.shape`, `.info()`, `.describe()`, `.isna().sum()`, and `.dtypes` are your vitals. Look for numbers stored as strings, rows that aren't observations (summary rows, totals, blanks), impossible values, and duplicated records.

**Treat** - fix what you diagnosed, and *comment why* for each fix. A fix you can't justify is a fix I'll ask you about.

**Discharge report** - three plots and **one insight**: a single sentence, backed by a number, that a smart non-technical person would care about. Association only. If your sentence contains the word "causes," rewrite it.

:::{admonition} Caution: check the vitals before and after
:class: warning
Run `.describe()` **before** you delete anything and **after**. If a summary row was hiding in your data, that comparison is where you'll see how badly it was skewing your statistics - and it's the number that makes your discharge report convincing.
:::

*Starter notebook and the patient dataset: to be posted in `OPIM5512-notebooks/Labs/Lab1_DataER/` before the first session.*

## Part 2 · First Commit (~45 min, same pairs)

Now the part that makes it count. Take your cleaned data and get it into a repo **through the loop**.

1. One of you creates a repo for the pair. Add your partner as a **collaborator** (Settings → Collaborators).
2. Set the **branch protection ruleset**: require a pull request before merging, required approvals = 1, scoped to the **default branch only**.
3. Lay it out properly: `source/`, `figures/`, `README.md`, `requirements.txt`.
4. Each of you makes your own branch. One takes the cleaning code, one takes the plots and the baseline model.
5. Fit a **baseline model** on the cleaned data - a linear regression or a decision tree, split properly, scaled on train only. It does not need to be good. It needs to be honest: report train *and* test.
6. Commit, push, open a **pull request** with your partner as reviewer. They open **Files changed**, review it for real, approve, merge, and delete the branch.
7. Swap. Do it again the other direction.

**You're done when the network graph shows the loop going both ways** and `python source/your_script.py` runs from a clean terminal after `pip install -r requirements.txt`.

:::{admonition} Remember: the deliverable is the repo, not the notebook
:class: important
A cleaned dataframe living in a Colab tab is worth nothing on Thursday morning. A repo your partner reviewed, merged, and can re-run is worth something for the rest of your career. This is the whole point of the lab.
:::

## Part 3 · Rounds (~10 min)

Two or three pairs share screens: the *before* and *after* `.describe()`, the one-sentence insight, and the network graph. We'll talk about what everybody found that nobody else did.

---

## The by-hand check

Short, on paper, from the week's core skill. The practice bank with fully-worked keys lives in [opim-math/OPIM5512](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512):

- [Confusion matrix by hand](https://github.com/drdave-teaching/opim-math/blob/main/OPIM5512/worksheets/ConfusionMatrix_ByHand_worksheet.pdf) - build the four cells, then compute precision, recall, and F1 without a library ([key](https://github.com/drdave-teaching/opim-math/blob/main/OPIM5512/worksheets/ConfusionMatrix_ByHand_key.pdf))

:::{admonition} On your own
:class: tip
Rerun your baseline model, but this time call `fit_transform` on the **whole** dataset before splitting. Record the test score. Then do it correctly - fit on train only - and record it again. The gap between those two numbers is data leakage, measured in your own data. Put both numbers in your README.
:::

---

*The hook into Module 2: you just tuned nothing and defended everything by hand. Next module we replace the hand with a search - pipelines, cross-validation, and grid search - so the leakage boundary you just measured is enforced by the code instead of your memory.*
