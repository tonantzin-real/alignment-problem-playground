# Three Alignment Failures You Can Play With

An interactive Book Club reflection on **_The Alignment Problem: Machine Learning and Human Values_** by Brian Christian (2020), for AIPI 590 (Explainable AI), Duke.

**Live demo:** https://tonantzin-real.github.io/alignment-problem-playground/

## What this is

The book is organized in three parts: **Prophecy**, **Agency** and **Normativity**. For each part I built one small interactive demo that makes one of its central ideas something you can play with.

| Part | Idea from the book | Demo |
|---|---|---|
| I. Prophecy | COMPAS and the impossibility of satisfying calibration and equal error rates at the same time when base rates differ | Sliders for group base rates and thresholds; live false positive and false negative rates, precision, and a calibration plot |
| II. Agency | Reward hacking (the boat that circles forever over respawning targets) and potential-based reward shaping | A gridworld agent solved with value iteration under three reward designs |
| III. Normativity | Inverse reinforcement learning: inferring what someone values from how they behave, and why behavior underdetermines values | Draw expert paths on a terrain map and run Maximum-Entropy IRL to infer the expert's terrain preferences |

## My reflection and Why this book

I picked this book because AI is everywhere now, and I think it’s really important for us to understand what we can do so these tools are aligned with what we ask them and with what we actually want them to do. Something I’ve noticed in other classes, like ethics and causal inference, is that these models will always give us an answer. But we need to make sure the questions we are asking are the ones we really want answered. If I had to explain it to a friend in one sentence: the alignment problem is basically the gap between what we tell an AI to do and what we actually want it to do.

### Prophecy: what the fairness demo taught me
For me, it all comes down to how we define fairness. As the book shows with COMPAS, there are different definitions, and we can choose between them depending on what we care about. But we can’t have all of them at the same time. If two groups have different base rates, using the same threshold for everyone gives each group different error rates, and fixing that means the same score gets treated differently depending on the group. In my Project 1 credit model, I honestly don’t think there is one right answer. If I had to pick, I would go with giving the same chance of approval to people who would actually pay back, because that’s the whole point of the model, no matter their age or if they are married. In the end, the ones who pay for any of these choices are the customers, especially the ones from groups that already get rejected more. In my project, those were people over 30 and married people, who got fewer recourse plans.

### Agency: the metric becomes the target
I will run into this in my second project, where I check the advice LLM chatbots give to people who were rejected for credit. The tricky part is choosing the right proxy, because in the end the proxy is what we measure, and sometimes it isn’t a good match for what we actually want.

### Normativity: learning values from behavior
What surprised me is that the learner could only tell the expert didn’t like mud when there was a mud path the expert could have taken and didn’t. If the expert never had that choice, the model just can’t know. This also shows up with chatbots, because they are trained with people rating their answers, so they learn what people reward and not always what people really meant. For my second project this matters a lot, because a chatbot can give credit advice that sounds good and gets good ratings, but doesn’t actually help someone get approved.

### What I'm taking with me
What I’m taking with me is that alignment doesn’t come for free. We have to define very carefully what we want, and even then the model might find a way around it. As someone taking an XAI class, I also think explaining a model is not enough if the goal of the model is wrong, because SHAP can explain really well a model that is optimizing the wrong thing. For me, explainability and alignment have to go together.

## How the demos work

Everything is plain HTML and JavaScript in `index.html`, with no libraries and no build step. All data is simulated with fixed seeds.

- **Fairness.** Each group's risk scores are drawn from a Beta distribution whose mean is the group's base rate, and each outcome is a Bernoulli draw with probability equal to the score. That makes the score perfectly calibrated by construction. With one shared threshold, the error rates differ between groups; equalizing false positive rates requires different thresholds, which breaks "same score, same decision" and opens a precision gap. ([Kleinberg, Mullainathan & Raghavan 2016](https://arxiv.org/abs/1609.05807); [Chouldechova 2017](https://arxiv.org/abs/1703.00056))
- **Reward hacking.** A 7×5 gridworld solved by value iteration. With a respawning +1 bonus tile, circling the bonus is worth more than the +10 goal at γ = 0.95, so the optimal agent never finishes. Potential-based shaping `F = γΦ(s') − Φ(s)` with `Φ = −distance to goal` adds dense feedback but provably leaves the optimal policy unchanged. ([Ng, Harada & Russell 1999](https://people.eecs.berkeley.edu/~russell/papers/icml99-shaping.pdf))
- **Inverse RL.** Maximum-Entropy IRL ([Ziebart et al. 2008](https://cdn.aaai.org/AAAI/2008/AAAI08-227.pdf)) with one-hot terrain features and a finite horizon. A soft value-iteration backward pass gives a stochastic policy, a forward pass gives expected feature counts, and gradient ascent matches them to the demonstrations' feature counts.

## Run it locally

Open `index.html` in a browser. That's it.

Coded using Claude Code, reflections are my own
