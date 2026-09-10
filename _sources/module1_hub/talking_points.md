# Module 1 Talking Points — The Theory You Should Be Able to Explain

**OPIM 5512 - Applied Data Science · Dr. Dave Wanik · University of Connecticut**

These are the *ideas* behind Module 1 - the things I say out loud in the lectures that you should be able to explain to a classmate (or an interviewer) without opening a notebook. The skills sheet is what you can *do*; this sheet is what you *understand*. If you can teach each of these in two sentences, you've got the module.

---

## 1 · GitHub and how teams really work

**Your GitHub is your face to the technical world.** Employers don't hire you because you claim you can code - they hire you because they can *see* how good your code is. The profile repo named after you renders on your landing page. That's not vanity; it's a portfolio that's open 24 hours a day.

**Four concepts cover 90% of daily Git.** A **repository** is a project under version control. A **commit** is a timestamped snapshot with a message. A **branch** is a parallel line of work you can develop or throw away safely. A **pull request** proposes merging a branch and - this is the point - invites review before it becomes real.

**Working directly on `main` is the habit to break first.** Everyone's guilty of it. The reason it's wrong isn't ceremony: `main` is the version other people trust and deploy. Anything unreviewed that lands there is a change nobody agreed to.

**Branches should live days, not months.** They serve a purpose, you do the work, you merge, you delete. Stale branches drift away from `main` and become painful to merge - the longer a branch lives, the more it costs.

**Branch protection is how you make the rule real.** Requiring a pull request plus one approval before merging means nobody - including you - can push straight to `main`. Scope it to the **default branch**. Protecting *all* branches locks you out of your own work, which is exactly the bug that grays out my commit button on camera in video 3.

**Ping-pong is the actual industry loop.** Branch → commit → PR → your partner reviews → approve → merge → delete, and then you swap roles and review theirs. The network graph shows the loop. Doing it five times with a classmate is worth more than reading about it fifty times.

**A review is a decision, not a rubber stamp.** In *Files changed* you can **Approve**, **Comment** without approving, or **Request changes** with an explanation of what's needed. All three are legitimate. Choosing on purpose is the skill.

**Reproducibility isn't bureaucracy - it's what makes a model deployable.** Everything you import at the top of a script must be in `requirements.txt`. A repo someone else can clone and run is a deliverable. A repo that only runs on your laptop is a story about a deliverable.

**Debugging in public is the lesson, not the blooper.** The commit that wouldn't commit, the folder that nested in the wrong place, the branch that didn't publish because it wasn't committed first - these happen to everyone, forever. The employable skill is calmly finding the cause.

---

## 2 · The machine learning methodology

**The methodology never changes: read → split → scale (fit on train) → fit → evaluate.** Different data, different target, different model - same five steps. When you get lost later in the course, come back to this sentence.

**Data leakage is the sin, and it's silent.** If you call `fit_transform` on the whole dataframe, your scaler learns the minimum and maximum of data your model isn't supposed to have seen. Nothing errors. Your scores just come back better than they deserve to be, and then production disappoints. **Fit on train, transform the test partition.** Every pipeline, every cross-validation split, every deployment in this book is built to protect that boundary.

**Scaling buys interpretability, not just performance.** Tree models mostly don't care - they find the optimal split either way. Neural networks care enormously. But even for trees, a 0-to-1 scale means a value of 0.9 tells you *90% of observations sit below this one*. On a raw scale that number means nothing unless you're already an expert with the data.

**A test value slightly above 1.0 is not a bug.** It means the training partition's maximum was smaller than the test partition's. Expect it, and know why.

**Never report one partition.** Train performance alone tells you the model memorized; test performance alone hides overfitting. Reporting both, side by side, is how you show the model generalizes - and it's what I want to see on every assignment.

**A default decision tree will overfit, by design.** The default minimum leaf size is one, which means the tree can memorize every row. Setting `min_samples_split` makes it slightly worse on train - *rightly so*, because you don't want memorization, you want generalization to a partition it's never seen.

**Different problems, different evidence.** Regression is judged with R², MAE, and MSE (MSE punishes the big misses hardest), plus **bias** - the mean of the errors - to catch a model that systematically over- or under-predicts. Classification is judged from the **confusion matrix**: precision, recall, F1 per class, and the weighted F1 for comparing models. Same methodology, different verdict.

**The scatter plot is the lie detector.** Plot predicted against actual for train *and* test, with a 45-degree line. If the cloud comes back as a flat horizontal line, your model is predicting the mean and has learned nothing - and no metric table will tell you that as fast as the picture does.

**"R, row."** Recall runs **across a row** of the confusion matrix: of everything that truly was a 1, how many did we catch? Precision runs **down a column**: of everything we *called* a 1, how many really were? The two fail in different directions, which is why F1 - their harmonic mean - exists.

**Comment for the audience you have.** During an analysis, bomb the notebook with text and comments so anyone can follow what you did. When you put something into production, strip it down - clean, fast, minimal. Knowing which mode you're in is a professional judgment.

---

## 3 · Neural networks

**A neural network is a nonlinear weighted sum.** Eight inputs get multiplied by a learned weight matrix to make a 1×10 hidden layer, that gets multiplied to make a 1×5, that gets multiplied to make a single prediction. No mathematics required to say that sentence, and it's genuinely what's happening.

**Weights start random and get better.** Before training they're initialized randomly; during training they're nudged, epoch after epoch, toward values that predict well. "Learned" just means "arrived at by repeated correction."

**The feedback signal is just error, and its sign is an instruction.** Actual minus predicted. A negative residual says the weights were too small - make a bigger prediction next time. A positive one says the model is overestimating. **Backpropagation** is the procedure that carries that instruction back through every layer.

**Without nonlinearity, the whole thing collapses into linear regression.** Stack as many weight matrices as you like - a chain of linear operations is still linear. The **activation functions between layers** are the entire source of a neural network's power.

**ReLU in one line: positive in, positive out; negative in, zero out.** That's it. Big negative weights are fine and normal - it's the *product* of input and weight that gets clipped to zero on the way into the hidden layer.

**An epoch is one weight-update pass across the dataset, however you chunk it.** The chunk size is the difference between the three flavors of gradient descent: **stochastic** (one row at a time - jumpy, 10,000 updates for 10,000 rows), **full** (the whole dataset, one smoothed update), and **batch** (10 to 100 rows - the middle ground, and what most people do).

**Scaling is optional for trees and mandatory here.** Unscaled columns overwhelm the network: the errors are large, the weight updates are large, and the model jumps around its optimum without ever landing on it. That failure has a name - **divergence**. Standard scaler or min-max, pick one, but scale.

**Early stopping should be the default and isn't.** It holds out 20% *of the training data* as a private feedback signal and stops training when the model quits improving on it. Note what it does **not** touch: your validation and test partitions are never involved in fitting - the network has no idea they exist.

**Regression and classification are the same network with a different output valve.** Classification puts a **sigmoid** on the output node to squish the result between 0 and 1, because you're predicting a probability. Regression uses a linear activation so the output can be any value. In scikit-learn you can't change it - the guardrails are there to keep you safe. In Keras (my deep learning class) you get softmax, sigmoid, linear, tanh, and the responsibility that comes with them.

**Read the loss curve for shape, not for score.** A gentle downward slope that flattens out is healthy. A cliff at the first iteration followed by nothing means something is wrong with the architecture. And the epoch count tells you nothing about how *good* the model is - a run that stops at 40 epochs can beat one that runs 200.

**Architecture: think about what you're asking the model to do.** People fire off 64, 128, 10 million hidden units because it's fun. But with eight inputs, how do you justify 200 learned intermediate features? One to two times the input count is plenty for tabular data, one to three layers is plenty, and the shape should be a **funnel** - wide first, then steady or narrowing. Eight inputs → 10 → 20 → 1 is nonsensical design.

**Networks are more forgiving than you'd expect.** Change the batch size, change the layer sizes, change the activation, and you often land in roughly the same place - when the model hits a bottleneck it self-corrects during training. So don't agonize over the architecture; start simple and tune deliberately in Module 2.

**Know when you've hit the ceiling.** In video 9 I try three batch sizes and two architectures and can't break 0.90 accuracy. That's not a failure to report - *maybe that's the upper limit of what I'm trying to predict here.* Recognizing the ceiling of a dataset is a real skill, and it's what keeps you from burning a week on tuning that was never going to pay.

---

## The bridge to Module 2

Every knob mentioned above - `hidden_layer_sizes`, `activation`, `max_iter`, `min_samples_split`, learning rate - was set by hand and defended with a story. Module 2 replaces the hand and the story with a **search**: pipelines, cross-validation, and grid search, so the tuning is systematic and the leakage boundary you just learned is enforced by the code instead of your memory.
