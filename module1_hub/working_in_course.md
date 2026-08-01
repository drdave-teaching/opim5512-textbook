# Working in this Course — Colab, GitHub, VS Code

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

Read this before Module 1's first assignment. 5512 is a **GitHub-first** course - the notebooks are how you learn, but the repo is how you're graded, and it's the thing you show an employer. Here's the whole workflow.

---

## 1 · Where everything lives

- **Notebooks:** [github.com/drdave-teaching/OPIM5512-notebooks](https://github.com/drdave-teaching/OPIM5512-notebooks) - every lecture notebook, organized by module folder. Public, no account needed just to *view*.
- **Labs and scripts:** [github.com/drdave-teaching/OPIM5512-labs](https://github.com/drdave-teaching/OPIM5512-labs) - the production `.py` versions, week READMEs, slide decks.
- **This ebook:** [drdave-teaching.github.io/opim5512-textbook](https://drdave-teaching.github.io/opim5512-textbook/) - chapter prose, key points, and links back to the notebooks.
- **By-hand worksheets:** [opim-math/OPIM5512](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512) - confusion matrix, k-fold cross-validation, TF-IDF, each with a fully-worked key.
- **HuskyCT:** the videos, announcements, gradebook, and due dates. When in doubt, start there.

## 2 · Two different tools, two different jobs

This trips people up in week one, so let's be blunt about it.

| | **Google Colab** | **VS Code + GitHub Desktop** |
|---|---|---|
| Use it for | watching along with lectures, exploring, trying things | assignments, labs, anything you submit |
| Why | zero setup, runs in the browser, free GPU later | this is what the job looks like - files, folders, terminals, version control |
| Where your work lands | your Drive or your GitHub, **if you save a copy** | your repo, on every commit |

You'll live in both. Don't pick a side.

## 3 · Running a lecture notebook (10 seconds)

Every notebook has an **Open in Colab** badge. Click it and the notebook opens against the live version in the class repo - all you need is a free Google account, no installs ever. The first run shows **"Warning: This notebook was not authored by Google."** That's expected, it's loaded from our GitHub. Click **Run anyway**, then **Runtime → Run all**.

:::{admonition} Caution: the class copy is read-only
:class: warning
The notebook you just opened is running against **my** repo. Edit and run all you like - but close the tab and your changes are gone. Before you do real work: **File → Save a copy in Drive** (fine for following along) or **File → Save a copy in GitHub** (do this - it's the habit that pays).
:::

## 4 · Your repos for this course

You'll create three kinds of repo, and each has a purpose:

1. **Your profile repo** - named exactly after your GitHub username. This is the special one that renders on your account landing page. Make it good; it's your face to the technical world, and employers read it.
2. **`A01`** (add your NetID if you like) - the individual assignment repo. Structure it properly: `source/` for code, `figures/` for output, a `README.md`, and a **`requirements.txt`**.
3. **The Ping Pong repo (A02)** - shared with a classmate, with them added as a **collaborator** and a **branch protection rule** requiring a pull request plus one approval before anything reaches `main`.

## 5 · The loop - do it until it's reflex

This is the single most employable thing in Module 1. Every assignment, every lab, same six steps:

```bash
# 1. branch (in GitHub Desktop: Current Branch -> New Branch)
# 2. edit in VS Code, save
# 3. commit with a real message
# 4. publish / push the branch
# 5. open a pull request, assign your reviewer
# 6. they approve -> merge -> DELETE the branch
```

:::{admonition} Remember: never push straight to `main`
:class: important
Everybody has done it. Nobody should. Branches exist so work can be reviewed before it becomes real, and they should live **days, not months** - do the work, merge it, delete the branch. A repo full of stale branches is a repo nobody trusts.
:::

## 6 · Making your work reproducible

A repo that only runs on your laptop isn't a deliverable. Two rules:

- **Everything you import at the top of a script must be in `requirements.txt`.** No exceptions. That file is what lets a teammate - or a cloud runner, in Module 3 - reproduce your environment.
- **Save figures to `figures/`, keep code in `source/`.** When you create a new file in VS Code, click the README (at the repo root) *first*, or your new file will nest inside whatever folder was last selected.

Run your script the way a professional does, from a terminal in the repo:

```bash
pip install -r requirements.txt
python source/DS_pipeline.py
```

## 7 · The rhythm of the course

- **Async weeks:** watch the module's videos, run the notebooks alongside, and push what you built to your repo. The lab assumes you've watched.
- **In-person lab weeks:** bring a laptop that opens Colab *and* has VS Code and GitHub Desktop working. We build on the async - I facilitate, you code.
- **By-hand checks:** short, on paper, drawn from the week's core skill (a confusion matrix, a k-fold split, TF-IDF). The practice bank with worked keys is in [opim-math/OPIM5512](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512).

## 8 · When something breaks

| Symptom | Fix |
|---|---|
| "Not authored by Google" warning | Expected. **Run anyway.** |
| Runtime disconnected / variables gone | Colab idles out. **Runtime → Run all** again. |
| My Colab edits disappeared | You edited the class copy without saving. See §3 - save a copy FIRST, then work. |
| The commit button is grayed out | Usually branch protection scoped too widely (it happens to me on camera in video 3) - check **Settings → Rules** and target the default branch only. Also check that you've ticked the changed files. |
| I pushed but GitHub shows nothing | You committed but didn't push, or pushed but didn't open the PR. Refresh the repo and check the branch dropdown. |
| `ModuleNotFoundError` when running my script | The import isn't in `requirements.txt`, or you didn't `pip install -r` it. |
| Cell order confusion (`NameError`) | You ran cells out of order. **Runtime → Restart and run all.** |
| My new file nested inside the wrong folder | Click the README at the repo root before creating the file, then create it. |

:::{admonition} On your own
:class: tip
After A01, open the notebook and the matching `.py` script in [OPIM5512-labs](https://github.com/drdave-teaching/OPIM5512-labs/tree/main/Module1) side by side. List every difference you can find - functions, no `drive.mount()`, no narration, imports at the top. That list *is* the difference between analysis code and production code, and it's most of what this course is about.
:::

*Pssst - you also have Claude, ChatGPT, or Gemini at your side. Use them to explain code and debug, then make sure YOU can explain what the code does. The understanding is the deliverable; the tools are the accelerant.*
