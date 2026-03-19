---
title: "Deep Learning Optimization Part 1: From Information Theory to Cross-Entropy Loss"
date: 2026-03-18 09:00:00 +0900
categories: [optimization, deep-learning]
tags: [optimization, cross-entropy, information-theory, kl-divergence, loss-function, mle, entropy]
---

> Most deep learning practitioners use Cross-Entropy Loss without questioning why it works. This post traces that question back to its roots — from Shannon's 1948 communication theory to the loss function you call every day.

## Table of Contents

- [0. The Problem We're Trying to Solve](#0-the-problem-were-trying-to-solve)
- [1. Probability vs Likelihood](#1-probability-vs-likelihood)
- [2. Information Theory](#2-information-theory)
- [3. KL Divergence](#3-kl-divergence)
- [4. Cross-Entropy Loss](#4-cross-entropy-loss)
- [5. References](#5-references)

---

## 0. The Problem We're Trying to Solve

Imagine you're building a model to predict whether a stock will go up or down tomorrow. You train it, evaluate it, and at some point you call `nn.CrossEntropyLoss()` without thinking twice. But why Cross-Entropy? Why not mean squared error? Why not something you invent yourself?

This question turns out to be more interesting than it looks. The answer isn't arbitrary — Cross-Entropy Loss has a precise mathematical justification rooted in information theory, a field developed in the 1940s for an entirely different purpose.

Training a neural network is fundamentally an optimization problem. You have a model parameterized by $\theta$, and you want to find the configuration that makes predictions as close to correct as possible. To search in any meaningful direction, you need a function that collapses the difference between prediction and ground truth into a single number.

$$\theta^* = \arg\min_\theta\ \mathcal{L}(\theta)$$

The design of that function — the loss function — is not a minor implementation detail. It determines what the model is actually optimizing for. Get it wrong, and the model learns the wrong thing, no matter how good your architecture is.

---

## 1. Probability vs Likelihood

### The Same Formula, Two Different Questions

Here's a scenario. A quant model assigns a 70% probability to a stock rising tomorrow. The stock rises. Great — but what does that actually mean?

There are two completely different ways to interpret that 70%, and confusing them is one of the most common conceptual errors in applied ML.

$$P(x \mid \theta)$$

**Probability** treats the model as fixed and asks what data it would generate. Given that we believe in this particular model $\theta$, how likely is it that we'd observe outcome $x$? This is a forward-looking question — you already have the model, and you're reasoning about what it predicts.

**Likelihood** flips the question entirely. The data is already observed, and you're asking which model best explains it. Given that the stock rose, how plausible is the model that assigned 70% to that outcome versus one that assigned 30%?

$$L(\theta \mid x) = P(x \mid \theta) \quad \leftarrow\ \text{same formula, } x \text{ is now fixed}$$

| | Probability | Likelihood |
|---|---|---|
| **Fixed** | Model $\theta$ | Observed data $x$ |
| **Varies** | Data $x$ | Parameters $\theta$ |
| **Question** | What data does this model predict? | Which model best explains this data? |
| **Direction** | Model → Data | Data → Model |

The formula is identical. The interpretation is opposite. This distinction matters because the entire training process in deep learning is a likelihood problem — we observe data and ask which model parameters best explain it.

### Maximum Likelihood Estimation

Suppose our quant model observes a sequence of daily market outcomes: up, up, down, up, down. We want to find the model parameters that make this sequence most probable. This is Maximum Likelihood Estimation.

For $n$ independent observations, the likelihood is

$$L(\theta) = \prod_{i=1}^{n} P(x_i \mid \theta)$$

and MLE finds the parameters that maximize it

$$\hat{\theta}_{MLE} = \arg\max_\theta\ L(\theta)$$

In practice, multiplying thousands of small probabilities together causes numerical underflow almost immediately. The standard fix is to take the log first. Since log is monotonically increasing, the maximizer doesn't change — we just work in a more numerically stable space.

$$\log L(\theta) = \sum_{i=1}^{n} \log P(x_i \mid \theta)$$

Deep learning frameworks are built around minimization, not maximization, so we flip the sign and minimize the **Negative Log-Likelihood (NLL)**

$$\text{NLL} = -\sum_{i=1}^{n} \log P(x_i \mid \theta)$$

This NLL is where the story connects to Cross-Entropy. But to see exactly how, we need to take a detour through information theory.

---

## 2. Information Theory

### Shannon's Original Problem

In 1948, Claude Shannon was working on a practical engineering problem at Bell Labs: how much information can a noisy telephone line reliably transmit? His answer — published as *"A Mathematical Theory of Communication"* — turned out to be far more general than anyone anticipated.

![Shannon's general communication system](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b7/Shannon_communication_system.svg/640px-Shannon_communication_system.svg.png)
*Figure 1. Shannon's communication system. Information from a source is encoded, transmitted through a noisy channel, and decoded at the destination. The same framework describes any system that estimates an unknown distribution from observed data.*

The key question Shannon had to answer first was deceptively simple.

> **How much information does a message carry?**

### Self-Information — Why Rare Events Matter More

Think about financial news. A headline saying "Markets open today" carries essentially no information — it happens every weekday. A headline saying "Exchange trading halted due to systemic failure" carries an enormous amount of information precisely because it almost never happens.

This intuition — that rarer events carry more information — needs to be turned into a mathematical formula. We want a function $I(x)$ that satisfies three conditions.

**Condition 1.** Rare events carry more information than common ones.

$$P(x) \downarrow \implies I(x) \uparrow$$

**Condition 2.** A certain event carries no information.

$$I(1) = 0$$

**Condition 3.** Information from independent events should add up.

$$I(x,\ y) = I(x) + I(y)$$

Condition 3 is the key constraint. If two events are independent, their probabilities multiply — but we want their information to add. The only function that converts multiplication into addition is the logarithm. Combining this with Condition 1 (we need $I$ to increase as $P$ decreases) gives us the negative log:

$$\boxed{I(x) = -\log P(x)}$$

![Self-information as a function of probability](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Binary_entropy_plot.svg/500px-Binary_entropy_plot.svg.png)
*Figure 2. Self-information $I(x) = -\log P(x)$. As probability approaches zero, information grows without bound. A certain event ($P = 1$) carries zero information.*

| Event | Probability | Information $-\log P$ |
|---|---|---|
| Major index moves less than 0.1% in a day | 0.95 | 0.05 |
| A stock moves more than 5% in a day | 0.10 | 2.30 |
| A blue-chip stock drops 30% in a single session | 0.001 | 6.91 |

One thing that confused me when I first saw this formula was the sign. $\log P(x)$ is always negative or zero (since $P$ is between 0 and 1), so why do we need to negate it to get information? The reason is direction: we want information to *increase* as probability *decreases*, but $\log P(x)$ does the opposite — it decreases toward $-\infty$ as $P \to 0$. The negative sign flips the direction so that rare events correctly map to large positive information values.

### Entropy — Average Surprise Across a Distribution

Self-information tells us how surprising a single event is. But in machine learning, we're usually dealing with entire distributions, not single events. What we want is a measure of how unpredictable a distribution is on average.

That's exactly what entropy captures.

$$H(X) = \mathbb{E}_{x \sim P}[I(x)] = -\sum_{x} P(x) \log P(x)$$

This is a weighted average of self-information values — each event's surprise is weighted by how often it occurs. A stock that always goes up has zero entropy (completely predictable). A stock whose daily direction is essentially a coin flip has maximum entropy (completely unpredictable).

![Binary entropy function](https://upload.wikimedia.org/wikipedia/commons/thumb/2/22/Binary_entropy_function.svg/500px-Binary_entropy_function.svg.png)
*Figure 3. Binary entropy $H(q) = -q\log q - (1-q)\log(1-q)$. Entropy peaks at $q = 0.5$ (maximum uncertainty) and falls to zero at the extremes (certainty).*

Consider a simplified model predicting whether a stock will rise tomorrow, with probability $q$ of rising:

$$H(q) = -q \log q - (1-q)\log(1-q)$$

| $q$ | Scenario | Entropy |
|---|---|---|
| 0.01 | Model is almost certain it will fall | Near 0 — very predictable |
| **0.50** | Model has no idea | **Maximum** — completely uncertain |
| 0.99 | Model is almost certain it will rise | Near 0 — very predictable |

A key insight here is that entropy being low doesn't mean the model is *right* — it just means it's *confident*. A model can be confidently wrong. This is exactly why the loss function needs to penalize confident wrong predictions more heavily than uncertain ones, which we'll come back to in Section 4.

---

## 3. KL Divergence

### Measuring the Gap Between Two Distributions

We now have the tools to talk about a single distribution's uncertainty. But what we actually need in deep learning is a way to measure the distance between two distributions: the true label distribution $P$ and the model's predicted distribution $Q$.

Entropy alone won't work here — it describes a single distribution, not the relationship between two. What we need is KL Divergence.

$$D_{KL}(P \| Q) = \mathbb{E}_{x \sim P}\left[\log \frac{P(x)}{Q(x)}\right] = \sum_x P(x)\left[\log P(x) - \log Q(x)\right]$$

Think of it this way. Suppose a portfolio manager uses a simplified model $Q$ to make decisions about a market that actually follows distribution $P$. The KL Divergence quantifies the extra cost — in terms of suboptimal decisions — incurred by using the wrong model. When $Q = P$, there's no extra cost. The further $Q$ drifts from $P$, the higher the cost.

### Key Properties

| Property | Description |
|---|---|
| $D_{KL}(P \| Q) \geq 0$ | Always non-negative |
| $D_{KL}(P \| Q) = 0$ | If and only if $P = Q$ |
| $D_{KL}(P \| Q) \neq D_{KL}(Q \| P)$ | **Asymmetric** — order matters |

The asymmetry is the part that surprises most people. It's tempting to think of KL Divergence as a distance metric, but it isn't — distance metrics are symmetric by definition. The asymmetry reflects a real difference in what's being measured: $D_{KL}(P \| Q)$ measures the cost of approximating $P$ with $Q$, which is fundamentally different from the cost of approximating $Q$ with $P$.

### Forward KL vs Reverse KL

This asymmetry has practical consequences that are easy to overlook.

**Forward KL** — $D_{KL}(P \| Q)$ — penalizes any region where $P > 0$ but $Q \approx 0$. To minimize it, $Q$ must assign non-zero probability everywhere $P$ does. This leads to **mean-seeking** behavior: the approximation spreads out to cover all modes of $P$.

**Reverse KL** — $D_{KL}(Q \| P)$ — penalizes any region where $Q > 0$ but $P \approx 0$. To minimize it, $Q$ concentrates on one region where $P$ is high and ignores the rest. This leads to **mode-seeking** behavior.

In supervised classification, minimizing Cross-Entropy Loss corresponds to minimizing the Forward KL — the model is forced to cover all cases where the true label has mass, rather than collapsing onto a single confident prediction.

---

## 4. Cross-Entropy Loss

### Where Everything Connects

Cross-Entropy is defined as the sum of the true distribution's entropy and the KL Divergence between them:

$$H(P, Q) = H(P) + D_{KL}(P \| Q)$$

Expanding each term and simplifying:

$$H(P, Q) = -\mathbb{E}_{x \sim P}[\log P(x)] + \mathbb{E}_{x \sim P}[\log P(x) - \log Q(x)]$$

$$= -\mathbb{E}_{x \sim P}[\log Q(x)]$$

$$\boxed{H(P, Q) = -\sum_x P(x) \log Q(x)}$$

The $\log P(x)$ terms cancel out completely, leaving only $Q$ — the model's prediction. This cancellation is not a coincidence. It's why Cross-Entropy is so convenient as a loss function: we only need the model's output, not the true distribution's self-information.

A common point of confusion here is whether Cross-Entropy and KL Divergence are the same thing. They're not — but during training, they're equivalent objectives. Since $H(P)$ is fixed (the true labels don't change during training), minimizing Cross-Entropy is the same as minimizing KL Divergence:

$$\min_\theta H(P, Q) = \underbrace{H(P)}_{\text{constant}} + \min_\theta D_{KL}(P \| Q)$$

**Minimizing Cross-Entropy = closing the gap between the model's predictions and the true distribution.**

### One-Hot Labels and the Connection to NLL

In practice, classification labels are one-hot vectors. For a model predicting three market regimes — bull, bear, sideways — if the true state is "bull":

$$P = [1,\ 0,\ 0]$$

Plugging into the Cross-Entropy formula:

$$H(P, Q) = -(1 \cdot \log Q(\text{bull}) + 0 \cdot \log Q(\text{bear}) + 0 \cdot \log Q(\text{sideways}))$$

$$= -\log Q(\text{bull})$$

All zero-probability terms vanish, leaving only the log-probability assigned to the correct class. This is exactly the NLL from Section 1. For classification with one-hot labels, **Cross-Entropy Loss and NLL are the same thing** — they just arrive from different directions.

### Softmax + Cross-Entropy

Raw model outputs (logits) are unconstrained real numbers. They don't sum to one and can be negative, so they can't be used directly as probabilities. Softmax converts them into a valid probability distribution:

$$\text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

The exponential ensures all values are positive, and dividing by the sum normalizes them. Importantly, the largest logit gets amplified disproportionately — which is why a confident model produces a peaked distribution after softmax.

### Binary vs Categorical Cross-Entropy

The form of Cross-Entropy adapts depending on the output structure.

**Binary Cross-Entropy** applies when the output is a single probability — for example, predicting whether a stock will rise or fall:

$$H = -\left[y \log q + (1-y)\log(1-q)\right]$$

**Categorical Cross-Entropy** applies for multi-class problems — predicting bull, bear, or sideways:

$$H = -\sum_{c=1}^{C} y_c \log q_c$$

Binary Cross-Entropy is simply the two-class special case of Categorical Cross-Entropy.

### Why the Logarithm Matters

The log function's nonlinearity is the whole point.

![Cross-Entropy loss: -log(q) vs predicted probability](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b4/Logarithm.svg/480px-Logarithm.svg.png)
*Figure 4. The $-\log(q)$ curve. Loss is near zero when the model is confidently correct, but grows without bound as the predicted probability of the correct class approaches zero.*

| Predicted Probability for Correct Class | Loss $= -\log q$ |
|---|---|
| 0.99 — model is confident and correct | **0.01** ✅ |
| 0.50 — model is uncertain | **0.69** |
| 0.10 — model is leaning toward the wrong answer | **2.30** |
| 0.01 — model is confident and wrong | **4.60** ❌ |

Consider two portfolio models. Model A says there's a 51% chance of a market crash and is wrong. Model B says there's a 99% chance of a market crash and is wrong. Both are wrong — but Model B should be penalized far more severely, because it was confidently wrong. The logarithm is what enforces this: the loss grows not linearly but explosively as the model's confidence in the wrong answer increases.

This connects directly back to the information-theoretic interpretation. A confident wrong prediction is maximally surprising under the true distribution — it carries the highest possible information content — so it receives the highest possible loss.

---

## 5. References

- Shannon, C. E. (1948). *A Mathematical Theory of Communication*. Bell System Technical Journal, 27(3), 379–423.
- Kullback, S., & Leibler, R. A. (1951). *On Information and Sufficiency*. The Annals of Mathematical Statistics, 22(1), 79–86.
- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*. Springer.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. [deeplearningbook.org](https://www.deeplearningbook.org)
- Karpathy, A. et al. *CS231n: Convolutional Neural Networks for Visual Recognition*. Stanford University. [cs231n.github.io](https://cs231n.github.io)
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*. MIT Press.
