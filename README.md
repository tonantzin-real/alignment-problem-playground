# Three Alignment Failures You Can Play With

An interactive Book Club reflection on **_The Alignment Problem: Machine Learning and Human Values_** by Brian Christian (2020), for AIPI 590 (Explainable AI), Duke.

**Live demo:** https://tonantzin-real.github.io/alignment-problem-playground/
**Video walkthrough:** VIDEO-LINK

## What this is

The book is organized in three parts: **Prophecy**, **Agency** and **Normativity**. For each part I built one small interactive demo that makes one of its central ideas something you can play with.

| Part | Idea from the book | Demo |
|---|---|---|
| I. Prophecy | COMPAS and the impossibility of satisfying calibration and equal error rates at the same time when base rates differ | Sliders for group base rates and thresholds; live false positive and false negative rates, precision, and a calibration plot |
| II. Agency | Reward hacking (the boat that circles forever over respawning targets) and potential-based reward shaping | A gridworld agent solved with value iteration under three reward designs |
| III. Normativity | Inverse reinforcement learning: inferring what someone values from how they behave, and why behavior underdetermines values | Draw expert paths on a terrain map and run Maximum-Entropy IRL to infer the expert's terrain preferences |

## My reflection

> _Write this part yourself. The rubric asks for your experience, not your peers' or AI's._

### Why this book, and the alignment problem in one sentence
_[your words]_

### Prophecy: what the fairness demo taught me
_[your words]. Suggested prompts: Before the book, what did "a fair model" mean to you? Which fairness definition would you choose for your Project 1 credit model, and who bears the cost of that choice?_

### Agency: the metric becomes the target
_[your words]. Suggested prompts: Where have you seen a proxy metric get gamed in real life? How is an LLM giving credit advice (Project 2) a reward-specification problem?_

### Normativity: learning values from behavior
_[your words]. Suggested prompts: What surprised you when the learner misread the expert? How does this relate to RLHF and fine-tuning LLMs on human preferences?_

### Book club discussions
_[your words]. What came up in the three meetings that changed or challenged your view?_

### What I'm taking with me
_[your words]. Where do you agree or disagree with Christian? Is explaining a model enough if its objective is wrong?_

## How the demos work

Everything is plain HTML and JavaScript in `index.html`, with no libraries and no build step. All data is simulated with fixed seeds.

- **Fairness.** Each group's risk scores are drawn from a Beta distribution whose mean is the group's base rate, and each outcome is a Bernoulli draw with probability equal to the score. That makes the score perfectly calibrated by construction. With one shared threshold, the error rates differ between groups; equalizing false positive rates requires different thresholds, which breaks "same score, same decision" and opens a precision gap. ([Kleinberg, Mullainathan & Raghavan 2016](https://arxiv.org/abs/1609.05807); [Chouldechova 2017](https://arxiv.org/abs/1703.00056))
- **Reward hacking.** A 7×5 gridworld solved by value iteration. With a respawning +1 bonus tile, circling the bonus is worth more than the +10 goal at γ = 0.95, so the optimal agent never finishes. Potential-based shaping `F = γΦ(s') − Φ(s)` with `Φ = −distance to goal` adds dense feedback but provably leaves the optimal policy unchanged. ([Ng, Harada & Russell 1999](https://people.eecs.berkeley.edu/~russell/papers/icml99-shaping.pdf))
- **Inverse RL.** Maximum-Entropy IRL ([Ziebart et al. 2008](https://cdn.aaai.org/AAAI/2008/AAAI08-227.pdf)) with one-hot terrain features and a finite horizon. A soft value-iteration backward pass gives a stochastic policy, a forward pass gives expected feature counts, and gradient ascent matches them to the demonstrations' feature counts.

## Run it locally

Open `index.html` in a browser. That's it.
