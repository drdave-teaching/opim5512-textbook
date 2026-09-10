# Module 1 Hub — Tool Time (Fall 2026)

:::{admonition} ⚠️ Work in progress
:class: warning
This book is under active development for Fall 2026 - pages may change as the course evolves.
:::

Everything for Module 1 in one place: the videos, the notebooks, the skills you're building, the theory you should be able to explain, and the Lab 1 materials. One arc runs through all of it:

> **Your GitHub is your face to the technical world - and the methodology never changes: read → split → scale on train → fit → evaluate.**

Module 1 is called Tool Time because we set up the workshop before we build anything. Two things happen at once: you get a real professional stack running (GitHub, GitHub Desktop, VS Code, a terminal), and you re-establish the modeling habits that every later module leans on. By the end you'll have pushed code through a reviewed pull request and fit six kinds of model - including your first neural network.

## The pieces

| What | Where | Why |
|---|---|---|
| 🎥 **The 10 videos** | HuskyCT (Module 1) · [what each one covers](videos.md) | the lectures - watch in order, run the notebook alongside |
| 📓 **The notebooks** | table below | every video drives one of these |
| ✅ **Skills sheet** | [what you can do now](skills.md) | check yourself off after the videos |
| 🧠 **Talking points** | [the theory to know](talking_points.md) | explain each in two sentences and you've got the module |
| 🛠️ **Working in this course** | [Colab, GitHub, VS Code](working_in_course.md) | READ FIRST - how to run and SAVE your work |
| 🏛️ **Lab 1 (Sep 2 / Sep 9)** | [Data ER + First Commit](lab1.md) | the in-person application |
| ✍️ **By-hand checks** | [opim-math worksheets](https://github.com/drdave-teaching/opim-math/tree/main/OPIM5512) | the by-hand practice bank |

## The two assignments

Module 1 is where both individual GitHub assignments live. They're not busywork - they're the habit the whole course runs on.

- **A01 - your first repos.** Make the special profile repo named after yourself, make an `A01` repo, clone it with GitHub Desktop, branch, add `source/` and `figures/` folders and a `requirements.txt`, write `boxplot.py`, commit, push, open a pull request, merge, delete the branch. Video 2 walks the whole thing.
- **A02 - "Ping Pong."** Pair up with a classmate. One of you creates a repo, adds the other as a **collaborator**, sets a **branch protection rule** (a PR plus one approval before anything hits `main`), and fits a neural network on California housing. Then you send pull requests back and forth - **at least five times** - reviewing each other's work. Video 3 is the full live demo. The network graph at the end is the deliverable you'll want to screenshot.

## The notebooks

Open in Colab and **Runtime → Run all**. Save your own copy before editing - see [Working in this Course](working_in_course.md).

**Block 1 · Setting up the shop** (videos 1-3)
- **Setting Up Your Tech Environment** - the full stack walkthrough in notebook form &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/SettingUpTechEnviro.ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/SettingUpTechEnviro.ipynb)
- **Shell (Bash) commands** - the terminal commands you'll actually use (`pwd`, `cd`, `ls`, `pip install -r`) &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Shell_(Bash)_commands.ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Shell_(Bash)_commands.ipynb)

**Block 2 · The ML refresher** (videos 4-5)
- **Regression: DTR / RFR / GBR on Boston Housing** - the end-to-end methodology, four models, honest evaluation &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTR_RFR_GBR_BostonHousing.ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTR_RFR_GBR_BostonHousing.ipynb)
- **Classification: DTC / RFC / GBC on Boston Housing** - same workflow, different evaluation &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTC_RFC_GBC_BostonHousing.ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/DTC_RFC_GBC_BostonHousing.ipynb)
- **Tree models on California housing** - the same tree lineup on the modern dataset the rest of the module uses &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/M1_TreeModels_CAHousing.ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/M1_TreeModels_CAHousing.ipynb)

**Block 3 · Neural networks in scikit-learn** (videos 6-10)
- **NN Regression (MLPRegressor)** - the network anatomy, ReLU, early stopping, batch size &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Regression%20(2).ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Regression%20(2).ipynb)
- **NN Classification (MLPClassifier)** - the same network with a sigmoid on the output node &nbsp; [![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Classification%20(2).ipynb) · [GitHub](https://github.com/drdave-teaching/OPIM5512-notebooks/blob/main/Module1/Week2_1_NN_Classification%20(2).ipynb)

🔷 **The nugget:** the notebooks are exploratory - lots of comments, lots of narration. The `.py` scripts in [OPIM5512-labs](https://github.com/drdave-teaching/OPIM5512-labs/tree/main/Module1) are the production versions of the same work: functions, no `drive.mount()`, runnable from anywhere. Open both side by side. That gap - notebook to script - is most of what "production data science" means.

```{tableofcontents}
```
