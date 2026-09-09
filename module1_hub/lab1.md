# Lab 1 — First Commit

**Hartford: Wed Sep 2 · Stamford: Wed Sep 9 · 5:30–7:30 PM**

The in-person half of Module 1. The async taught the mechanics; the lab is where you and a partner push a real result through a **reviewed pull request** — the loop you'll reuse for everything else this semester.

:::{admonition} The one-sentence version
:class: important
**Tonight the deliverable is a *repo*, not a notebook.** Two people each own half of the same story — one takes the **weather**, one takes the **electricity demand** — and the only way to the finished report is to **merge your work together**.
:::

Bring a laptop with **Colab** working and **GitHub Desktop** installed and signed in. If your stack isn't running yet, come anyway — the first few minutes exist for exactly that.

---

## What we're doing

| | Part | Time | Working |
|---|---|---|---|
| 1 | **Set up the shared repo** | ~10 min | Partner A drives |
| 2 | **Plot & ship** | ~30 min | each partner, own branch |
| 3 | **Review & merge** | ~20 min | both |
| 4 | **The report** | ~20 min | both, one screen |

The whole night is the collaboration loop: **branch → commit → push → pull request → review → merge.**

## Part 1 · Set up the shared repo (Partner A drives)

Start from the class **template repo** — it already contains the data, both notebooks, an empty `images/` folder, a README with the data dictionary, and a `REPORT.md` skeleton. You build none of that.

1. Partner A: open the [Lab 1 template](https://github.com/drdave-teaching/opim5512-lab1-template) → green **Use this template → Create a new repository** → owner = you, name `opim5512-lab1-<netidA>-<netidB>`, **Public**.
2. Partner A: **Settings → Collaborators** → add Partner B → Partner B **accepts** the invite.
3. Partner A: **Settings → Rules → Rulesets** → a branch ruleset on `main`: **require a pull request + 1 approval**. This is the two-person gate — nobody merges their own work.
4. Both: **clone once** in GitHub Desktop, then **Repository → Show in Explorer** to find the files on disk.
5. Each: make a branch — `dev-weather` (A) / `dev-demand` (B) → **Publish branch**.

:::{admonition} Read the top bar before every commit
:class: warning
If GitHub Desktop says you're on **`main`**, stop and switch to your `dev-` branch. A push rejected on `main` is the branch protection working, not a bug.
:::

## Part 2 · Plot & ship (each partner, on your own branch)

1. Open **your** notebook in Colab: **File → Open notebook → GitHub tab** → paste your repo URL → open `notebooks/Lab1_A_Weather` (A) or `Lab1_B_Demand` (B).
2. Partner A sets the campus (`hartford` / `stamford`). **Runtime → Run all** — a **line plot** appears and saves itself as a PNG.
3. In the **TODO** cell, write **one histogram** (the shape is given) — temperature (A) or demand (B). This is your only code tonight. Run it, look at it, title it with what a reader should notice.
4. Run the **download** cell → your two PNGs land in Downloads.
5. **File → Save** the notebook back to GitHub — your repo, your `dev-` branch, **same path**, with a real commit message. *(Plain **File → Save** — not Ctrl+S, which only autosaves to Drive.)*
6. GitHub Desktop → **Show in Explorer** → drag both PNGs into **`images/`** → **Commit** (real message) → **Push**.

:::{admonition} 🚫 No AI in Lab 1
:class: important
The histogram is three lines — type it. Git is muscle memory, and you can't approve a pull request you didn't read. Module 3 is the GenAI unit; this is not yet, not never.
:::

## Part 3 · Review & merge (both)

1. On github.com, open your **Compare & pull request** → request your **partner** as reviewer.
2. Open your **partner's** PR → **Files changed** → actually read their histogram cell and look at the PNGs → **Approve**.
3. **Merge** both PRs → **Delete branch**.
4. Both: GitHub Desktop → `main` → **Fetch → Pull**. Open `images/`: **four PNGs**. Neither of you could have produced that alone.

## Part 4 · The report (both, one screen)

Open `REPORT.md` → **Edit**. The four plots already render. Replace each **➜** line with one sentence — every number gets a unit (°F, MW, hours). Commit it on a `report` branch, PR it, the *other* partner approves, merge.

**Ahead of schedule?** Run the joint notebook (both series joined on the hour), drag `temp_vs_load.png` into `images/`, and chase the question at the bottom — the surprise is that the **hottest hour is not the peak-demand hour** (thermal mass + 6 PM behavior + cumulative heat).

:::{admonition} The deliverable is the repo, not the notebook
:class: important
A cleaned dataframe living in a Colab tab is worth nothing on Thursday morning. A repo your partner reviewed, merged, and can re-run is worth something for the rest of your career. That's the whole point of the lab.
:::

---

## Two editions

- **Simple edition (Stamford, and the current default)** — the data comes **pre-cleaned** in the template repo, so you spend the night on the *workflow*. Everything you need is in the kit: [START HERE, instructions, the 20-step map, printable handouts, and the Colab starters](https://github.com/drdave-teaching/OPIM5512-labs/tree/master/Module1/Week1_TechStack/Lab1_FirstCommit/simple).
- **Extended edition (Hartford's first run)** — you **clean the two datasets yourself** (METAR weather at `:51` past the hour with `M`/`T` flags; ISO-NE demand on *Hour Ending 1–24*) and stage a merge conflict on purpose. The [extended kit](https://github.com/drdave-teaching/OPIM5512-labs/tree/master/Module1/Week1_TechStack/Lab1_FirstCommit) is one folder up.

:::{admonition} Online / solo students
:class: tip
Pair over Teams if you can — each on your own account, one owns the repo and adds the other. Otherwise run two GitHub accounts (needs a second email), or go solo with **Required approvals = 0**: branch → PR → merge yourself. Join the live session on Webex for at least one lab so you know how these run.
:::

## Definition of done

- [ ] Both partners are collaborators; branch protection on `main`
- [ ] `images/` has four PNGs with the exact filenames (two per partner)
- [ ] Both notebooks saved back with a histogram in each
- [ ] `REPORT.md` — one real sentence under each plot, plus one honest "what this data can't tell us"
- [ ] ≥3 merged pull requests, branches deleted, both of you authoring **and** reviewing
- [ ] A network graph (Insights → Network) showing the loop going both ways

## The by-hand check

Short, on paper, from the week's core skill. The practice bank with fully-worked keys lives in [opim-math/OPIM5512](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512):

- [Confusion matrix by hand](https://github.com/drdave-teaching/opim-math/blob/main/OPIM5512/worksheets/ConfusionMatrix_ByHand_worksheet.pdf) — build the four cells, then compute precision, recall, and F1 without a library ([key](https://github.com/drdave-teaching/opim-math/blob/main/OPIM5512/worksheets/ConfusionMatrix_ByHand_key.pdf))

---

*The hook into Module 2: tonight you shipped a model's inputs through a reviewed loop. Next module you fit models you can **defend** — resample the training partition only, report a distribution instead of a lucky number, and say which features mattered. That last part becomes [Lab 2 — Explaining a Model (SHAP)](../module2_hub/lab2.md).*
