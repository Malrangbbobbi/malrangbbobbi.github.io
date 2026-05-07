---
title: "Deep Learning Optimization Part 1: From Information Theory to Cross-Entropy Loss"
date: 2026-03-18 09:00:00 +0900
categories: [AI, deep-learning]
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

**Probability** treats the model as fixed and asks what data it would generate. Given that we believe in this particular model $\theta$, how likely is it that we'd observe outcome $x$?

**Likelihood** flips the question entirely. The data is already observed, and you're asking which model best explains it. Given that the stock rose, how plausible is the model that assigned 70% to that outcome versus one that assigned 30%?

$$L(\theta \mid x) = P(x \mid \theta) \quad \leftarrow\ \text{same formula, } x \text{ is now fixed}$$

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 200" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <!-- Background -->
  <rect width="680" height="200" fill="none"/>

  <!-- Probability side -->
  <rect x="30" y="40" width="180" height="60" rx="10" fill="#1a3a5c" stroke="#378ADD" stroke-width="1.5"/>
  <text x="120" y="65" text-anchor="middle" font-size="13" font-weight="600" fill="#85B7EB">Model θ</text>
  <text x="120" y="85" text-anchor="middle" font-size="11" fill="#B5D4F4">Fixed</text>

  <rect x="30" y="130" width="180" height="48" rx="10" fill="#1a2a1a" stroke="#1D9E75" stroke-width="1.5"/>
  <text x="120" y="152" text-anchor="middle" font-size="13" font-weight="600" fill="#5DCAA5">Data x</text>
  <text x="120" y="168" text-anchor="middle" font-size="11" fill="#9FE1CB">Varies — P asks: how likely?</text>

  <!-- Arrow Probability -->
  <line x1="210" y1="70" x2="300" y2="100" stroke="#378ADD" stroke-width="1.5" marker-end="url(#arr)"/>
  <text x="248" y="78" text-anchor="middle" font-size="11" fill="#85B7EB">Probability</text>

  <!-- Center formula -->
  <rect x="290" y="76" width="100" height="44" rx="8" fill="#2a2a3a" stroke="#7F77DD" stroke-width="1.5"/>
  <text x="340" y="96" text-anchor="middle" font-size="13" font-weight="600" fill="#AFA9EC">P(x|θ)</text>
  <text x="340" y="112" text-anchor="middle" font-size="10" fill="#CECBF6">same formula</text>

  <!-- Arrow Likelihood -->
  <line x1="390" y1="100" x2="468" y2="70" stroke="#D85A30" stroke-width="1.5" marker-end="url(#arr2)"/>
  <text x="440" y="78" text-anchor="middle" font-size="11" fill="#F0997B">Likelihood</text>

  <!-- Likelihood side -->
  <rect x="470" y="40" width="180" height="60" rx="10" fill="#1a2a1a" stroke="#1D9E75" stroke-width="1.5"/>
  <text x="560" y="65" text-anchor="middle" font-size="13" font-weight="600" fill="#5DCAA5">Data x</text>
  <text x="560" y="85" text-anchor="middle" font-size="11" fill="#9FE1CB">Fixed</text>

  <rect x="470" y="130" width="180" height="48" rx="10" fill="#3a1a1a" stroke="#D85A30" stroke-width="1.5"/>
  <text x="560" y="152" text-anchor="middle" font-size="13" font-weight="600" fill="#F0997B">Model θ</text>
  <text x="560" y="168" text-anchor="middle" font-size="11" fill="#F5C4B3">Varies — L asks: how plausible?</text>

  <defs>
    <marker id="arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#378ADD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="arr2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#D85A30" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 1. The same expression P(x|θ) reads differently depending on what is fixed. Probability reasons from model to data; Likelihood reasons from data to model.
</figcaption>
</figure>

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

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 130" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="130" fill="none"/>

  <!-- Nodes -->
  <rect x="20"  y="40" width="110" height="50" rx="8" fill="#1a3a5c" stroke="#378ADD" stroke-width="1.2"/>
  <text x="75"  y="62" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Information</text>
  <text x="75"  y="78" text-anchor="middle" font-size="11" fill="#85B7EB">Source</text>

  <rect x="170" y="40" width="110" height="50" rx="8" fill="#1a3a5c" stroke="#378ADD" stroke-width="1.2"/>
  <text x="225" y="62" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Transmitter</text>
  <text x="225" y="78" text-anchor="middle" font-size="10" fill="#B5D4F4">(Encoder)</text>

  <rect x="320" y="40" width="110" height="50" rx="8" fill="#3a1a1a" stroke="#D85A30" stroke-width="1.2"/>
  <text x="375" y="62" text-anchor="middle" font-size="11" font-weight="600" fill="#F0997B">Noisy</text>
  <text x="375" y="78" text-anchor="middle" font-size="11" fill="#F0997B">Channel</text>

  <rect x="470" y="40" width="110" height="50" rx="8" fill="#1a3a5c" stroke="#378ADD" stroke-width="1.2"/>
  <text x="525" y="62" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Receiver</text>
  <text x="525" y="78" text-anchor="middle" font-size="10" fill="#B5D4F4">(Decoder)</text>

  <!-- Arrows -->
  <line x1="130" y1="65" x2="168" y2="65" stroke="#378ADD" stroke-width="1.2" marker-end="url(#a1)"/>
  <line x1="280" y1="65" x2="318" y2="65" stroke="#D85A30" stroke-width="1.2" marker-end="url(#a2)"/>
  <line x1="430" y1="65" x2="468" y2="65" stroke="#378ADD" stroke-width="1.2" marker-end="url(#a1)"/>

  <!-- Noise source -->
  <rect x="320" y="95" width="110" height="28" rx="6" fill="#2a1a2a" stroke="#993556" stroke-width="1"/>
  <text x="375" y="113" text-anchor="middle" font-size="10" fill="#ED93B1">Noise Source</text>
  <line x1="375" y1="95" x2="375" y2="90" stroke="#993556" stroke-width="1" stroke-dasharray="3,2"/>

  <defs>
    <marker id="a1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#378ADD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="a2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#D85A30" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 2. Shannon's general communication system. The same framework describes a neural network estimating an unknown distribution from noisy observed data.
</figcaption>
</figure>

Shannon's question was deceptively simple.

> **How much "information" does a message carry?**

### Self-Information — Why Rare Events Matter More

Think about financial news. A headline saying "Markets open today" carries essentially no information — it happens every weekday. A headline saying "Exchange trading halted due to systemic failure" carries an enormous amount of information precisely because it almost never happens.

We want a function $I(x)$ that satisfies three conditions.

**Condition 1.** Rare events carry more information than common ones.

$$P(x) \downarrow \implies I(x) \uparrow$$

**Condition 2.** A certain event carries no information.

$$I(1) = 0$$

**Condition 3.** Information from independent events should add up.

$$I(x,\ y) = I(x) + I(y)$$

Condition 3 is the key constraint. If two events are independent, their probabilities multiply — but we want their information to add. The only function that converts multiplication into addition is the logarithm. Combining with Condition 1 gives us the negative log

$$\boxed{I(x) = -\log P(x)}$$

One thing that confused me when I first saw this formula was the sign. $\log P(x)$ is always negative or zero since $P$ is between 0 and 1 — so why do we negate it? The reason is direction: we want information to *increase* as probability *decreases*, but $\log P(x)$ does the opposite. The negative sign flips the direction so that rare events correctly map to large positive information values.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 280" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="280" fill="none"/>

  <!-- Axes -->
  <line x1="80" y1="240" x2="620" y2="240" stroke="#555" stroke-width="1.2"/>
  <line x1="80" y1="20"  x2="80"  y2="240" stroke="#555" stroke-width="1.2"/>

  <!-- Axis labels -->
  <text x="350" y="268" text-anchor="middle" font-size="13" fill="#aaa">Probability P(x)</text>
  <text x="22" y="135" text-anchor="middle" font-size="13" fill="#aaa" transform="rotate(-90,22,135)">Information I(x)</text>

  <!-- X ticks -->
  <text x="80"  y="255" text-anchor="middle" font-size="10" fill="#888">0</text>
  <text x="188" y="255" text-anchor="middle" font-size="10" fill="#888">0.2</text>
  <text x="296" y="255" text-anchor="middle" font-size="10" fill="#888">0.4</text>
  <text x="404" y="255" text-anchor="middle" font-size="10" fill="#888">0.6</text>
  <text x="512" y="255" text-anchor="middle" font-size="10" fill="#888">0.8</text>
  <text x="610" y="255" text-anchor="middle" font-size="10" fill="#888">1.0</text>

  <!-- Y ticks -->
  <text x="70" y="244" text-anchor="end" font-size="10" fill="#888">0</text>
  <text x="70" y="194" text-anchor="end" font-size="10" fill="#888">1</text>
  <text x="70" y="144" text-anchor="end" font-size="10" fill="#888">2</text>
  <text x="70" y="94"  text-anchor="end" font-size="10" fill="#888">3</text>
  <text x="70" y="44"  text-anchor="end" font-size="10" fill="#888">4</text>

  <!-- Grid lines -->
  <line x1="80" y1="194" x2="620" y2="194" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="144" x2="620" y2="144" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="94"  x2="620" y2="94"  stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="44"  x2="620" y2="44"  stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>

  <!-- -log(p) curve: p from 0.01 to 1.0, I = -ln(p) * 50, x = 80 + p*530 -->
  <polyline fill="none" stroke="#378ADD" stroke-width="2.2" stroke-linejoin="round"
    points="
      85,240
      91,227
      102,213
      134,194
      188,166
      242,148
      296,134
      350,122
      404,112
      458,103
      512,95
      566,88
      610,82
    "
  />

  <!-- Annotated points -->
  <!-- P=0.01 → I≈4.6 -->
  <circle cx="86" cy="10" r="4" fill="#E24B4A"/>
  <text x="96" y="14" font-size="10" fill="#E24B4A">P=0.01 → I≈4.6 (rare)</text>

  <!-- P=0.5 → I≈0.69 -->
  <circle cx="350" cy="206" r="4" fill="#EF9F27"/>
  <text x="358" y="202" font-size="10" fill="#EF9F27">P=0.5 → I≈0.69</text>

  <!-- P=1.0 → I=0 -->
  <circle cx="610" cy="240" r="4" fill="#1D9E75"/>
  <text x="550" y="232" font-size="10" fill="#1D9E75">P=1.0 → I=0 (certain)</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 3. Self-information $I(x) = -\log P(x)$. As probability approaches zero, information grows without bound. Certain events carry zero information.
</figcaption>
</figure>

| Event | Probability | Information $-\log P$ |
|---|---|---|
| Major index moves less than 0.1% in a day | 0.95 | 0.05 |
| A stock moves more than 5% in a day | 0.10 | 2.30 |
| A blue-chip stock drops 30% in a single session | 0.001 | 6.91 |

### Entropy — Average Surprise Across a Distribution

Self-information tells us how surprising a single event is. But in machine learning, we're usually dealing with entire distributions. What we want is a measure of how unpredictable a distribution is on average.

$$H(X) = \mathbb{E}_{x \sim P}[I(x)] = -\sum_{x} P(x) \log P(x)$$

This is a weighted average of self-information values — each event's surprise is weighted by how often it occurs. A stock that always goes up has zero entropy. A stock whose daily direction is essentially a coin flip has maximum entropy.

For a binary distribution with parameter $q$:

$$H(q) = -q \log q - (1-q)\log(1-q)$$

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 280" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="280" fill="none"/>

  <!-- Axes -->
  <line x1="80" y1="240" x2="620" y2="240" stroke="#555" stroke-width="1.2"/>
  <line x1="80" y1="20"  x2="80"  y2="240" stroke="#555" stroke-width="1.2"/>

  <!-- Labels -->
  <text x="350" y="268" text-anchor="middle" font-size="13" fill="#aaa">q (probability of rising)</text>
  <text x="22" y="135" text-anchor="middle" font-size="13" fill="#aaa" transform="rotate(-90,22,135)">Entropy H(q)</text>

  <!-- X ticks -->
  <text x="80"  y="255" text-anchor="middle" font-size="10" fill="#888">0</text>
  <text x="188" y="255" text-anchor="middle" font-size="10" fill="#888">0.2</text>
  <text x="296" y="255" text-anchor="middle" font-size="10" fill="#888">0.4</text>
  <text x="350" y="255" text-anchor="middle" font-size="10" fill="#EF9F27">0.5</text>
  <text x="404" y="255" text-anchor="middle" font-size="10" fill="#888">0.6</text>
  <text x="512" y="255" text-anchor="middle" font-size="10" fill="#888">0.8</text>
  <text x="610" y="255" text-anchor="middle" font-size="10" fill="#888">1.0</text>

  <!-- Y ticks -->
  <text x="70" y="244" text-anchor="end" font-size="10" fill="#888">0</text>
  <text x="70" y="130" text-anchor="end" font-size="10" fill="#888">0.5</text>
  <text x="70" y="44"  text-anchor="end" font-size="10" fill="#888">1.0</text>

  <!-- Grid -->
  <line x1="80" y1="130" x2="620" y2="130" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="44"  x2="620" y2="44"  stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="350" y1="20" x2="350" y2="240" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>

  <!-- H(q) curve — bell shape peaking at q=0.5 -->
  <polyline fill="none" stroke="#7F77DD" stroke-width="2.2" stroke-linejoin="round"
    points="
      80,240
      134,186
      188,152
      242,128
      296,112
      350,44
      404,112
      458,128
      512,152
      566,186
      610,240
    "
  />
  <path d="M80,240 Q134,186 188,152 Q242,128 296,112 Q323,78 350,44 Q377,78 404,112 Q458,128 512,152 Q566,186 610,240"
    fill="#7F77DD" fill-opacity="0.08" stroke="none"/>

  <!-- Peak annotation -->
  <circle cx="350" cy="44" r="5" fill="#EF9F27"/>
  <text x="365" y="40" font-size="11" fill="#EF9F27">Maximum entropy</text>
  <text x="365" y="54" font-size="10" fill="#EF9F27">(q=0.5, completely uncertain)</text>

  <!-- Zero annotations -->
  <circle cx="80"  cy="240" r="4" fill="#1D9E75"/>
  <circle cx="610" cy="240" r="4" fill="#1D9E75"/>
  <text x="88"  y="228" font-size="10" fill="#1D9E75">H=0</text>
  <text x="570" y="228" font-size="10" fill="#1D9E75">H=0</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 4. Binary entropy $H(q)$. A model that is completely uncertain about tomorrow's direction (q=0.5) has maximum entropy. Certainty in either direction brings entropy to zero.
</figcaption>
</figure>

| $q$ | Scenario | Entropy |
|---|---|---|
| 0.01 | Model is almost certain the stock will fall | Near 0 — very predictable |
| **0.50** | Model has no idea | **Maximum** — completely uncertain |
| 0.99 | Model is almost certain the stock will rise | Near 0 — very predictable |

A key insight here is that low entropy doesn't mean the model is *right* — it just means it's *confident*. A model can be confidently wrong. This is exactly why the loss function needs to penalize confident wrong predictions more heavily than uncertain ones, which we'll see concretely in Section 4.

---

## 3. KL Divergence

### Measuring the Gap Between Two Distributions

We now have tools to describe a single distribution's uncertainty. But what we actually need in deep learning is a way to measure the distance between two distributions: the true label distribution $P$ and the model's predicted distribution $Q$.

Entropy alone won't work — it describes a single distribution, not the relationship between two. KL Divergence fills this gap.

$$D_{KL}(P \| Q) = \mathbb{E}_{x \sim P}\left[\log \frac{P(x)}{Q(x)}\right] = \sum_x P(x)\left[\log P(x) - \log Q(x)\right]$$

Think of it as the **extra cost** incurred when a portfolio manager uses an approximate model $Q$ to make decisions about a market that actually follows $P$. When $Q = P$, there's no extra cost. The further $Q$ drifts from $P$, the higher the cost.

### Key Properties

| Property | Description |
|---|---|
| $D_{KL}(P \| Q) \geq 0$ | Always non-negative |
| $D_{KL}(P \| Q) = 0$ | If and only if $P = Q$ |
| $D_{KL}(P \| Q) \neq D_{KL}(Q \| P)$ | **Asymmetric** — order matters |

The asymmetry surprises most people. It's tempting to treat KL Divergence as a distance metric, but it isn't — distance metrics are symmetric by definition. The asymmetry reflects a real difference: $D_{KL}(P \| Q)$ measures the cost of approximating $P$ with $Q$, which is fundamentally different from the reverse.

### Forward KL vs Reverse KL

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 200" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="200" fill="none"/>

  <!-- Forward KL panel -->
  <rect x="20" y="10" width="300" height="175" rx="10" fill="#1a1a2e" stroke="#534AB7" stroke-width="1"/>
  <text x="170" y="32" text-anchor="middle" font-size="12" font-weight="600" fill="#AFA9EC">Forward KL — D_KL(P‖Q)</text>

  <!-- P curve (bimodal) -->
  <polyline fill="none" stroke="#378ADD" stroke-width="2" stroke-linejoin="round"
    points="40,150 60,148 75,130 90,80 105,130 120,148 135,148 150,130 165,80 180,130 195,148 210,150"/>
  <text x="125" y="168" text-anchor="middle" font-size="10" fill="#378ADD">True P (bimodal)</text>

  <!-- Q curve (spread) -->
  <polyline fill="none" stroke="#EF9F27" stroke-width="2" stroke-dasharray="5,3" stroke-linejoin="round"
    points="40,148 75,135 110,110 145,100 180,110 210,135 240,148"/>
  <text x="175" y="100" text-anchor="middle" font-size="10" fill="#EF9F27">Q spreads to cover all</text>
  <text x="50" y="185" text-anchor="start" font-size="10" fill="#7F77DD">Mean-seeking behavior</text>

  <!-- Reverse KL panel -->
  <rect x="360" y="10" width="300" height="175" rx="10" fill="#1a1a2e" stroke="#993556" stroke-width="1"/>
  <text x="510" y="32" text-anchor="middle" font-size="12" font-weight="600" fill="#ED93B1">Reverse KL — D_KL(Q‖P)</text>

  <!-- P curve (bimodal) -->
  <polyline fill="none" stroke="#378ADD" stroke-width="2" stroke-linejoin="round"
    points="380,150 400,148 415,130 430,80 445,130 460,148 475,148 490,130 505,80 520,130 535,148 550,150"/>
  <text x="465" y="168" text-anchor="middle" font-size="10" fill="#378ADD">True P (bimodal)</text>

  <!-- Q curve (one mode only) -->
  <polyline fill="none" stroke="#EF9F27" stroke-width="2" stroke-dasharray="5,3" stroke-linejoin="round"
    points="380,150 400,148 415,140 430,80 445,140 460,148 475,150 490,150 505,150 520,150 550,150"/>
  <text x="430" y="60" text-anchor="middle" font-size="10" fill="#EF9F27">Q locks onto one mode</text>
  <text x="390" y="185" text-anchor="start" font-size="10" fill="#993556">Mode-seeking behavior</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 5. Forward KL (left) forces Q to cover all regions where P has mass — mean-seeking. Reverse KL (right) lets Q collapse onto one mode and ignore the rest — mode-seeking.
</figcaption>
</figure>

**Forward KL** — $D_{KL}(P \| Q)$ — penalizes any region where $P > 0$ but $Q \approx 0$. To minimize it, $Q$ must assign non-zero probability everywhere $P$ does. This leads to **mean-seeking** behavior.

**Reverse KL** — $D_{KL}(Q \| P)$ — penalizes any region where $Q > 0$ but $P \approx 0$. The minimizer concentrates on one region where $P$ is high and ignores the rest. This leads to **mode-seeking** behavior.

In supervised classification, minimizing Cross-Entropy Loss corresponds to minimizing the Forward KL — the model is forced to cover all cases where the true label has mass.

---

## 4. Cross-Entropy Loss

### Where Everything Connects

Cross-Entropy is the sum of the true distribution's entropy and the KL Divergence between them:

$$H(P, Q) = H(P) + D_{KL}(P \| Q)$$

Expanding and simplifying:

$$H(P, Q) = -\mathbb{E}_{x \sim P}[\log P(x)] + \mathbb{E}_{x \sim P}[\log P(x) - \log Q(x)]$$

$$\boxed{H(P, Q) = -\sum_x P(x) \log Q(x)}$$

The $\log P(x)$ terms cancel completely, leaving only $Q$ — the model's prediction. This is not a coincidence. It's what makes Cross-Entropy tractable as a loss function.

A common confusion here is whether Cross-Entropy and KL Divergence are the same thing. They're not — but during training, they're equivalent objectives. Since $H(P)$ is constant (the true labels don't change), minimizing Cross-Entropy is identical to minimizing KL Divergence:

$$\min_\theta H(P, Q) = \underbrace{H(P)}_{\text{constant}} + \min_\theta D_{KL}(P \| Q)$$

**Minimizing Cross-Entropy = closing the gap between the model's predictions and the true distribution.**

### One-Hot Labels and the Connection to NLL

In practice, classification labels are one-hot vectors. For a model predicting three market regimes — bull, bear, sideways — if the true state is "bull":

$$P = [1,\ 0,\ 0]$$

Plugging into the Cross-Entropy formula:

$$H(P, Q) = -(1 \cdot \log Q(\text{bull}) + 0 \cdot \log Q(\text{bear}) + 0 \cdot \log Q(\text{sideways}))$$

$$= -\log Q(\text{bull})$$

All zero-probability terms vanish. Only the log-probability assigned to the correct class remains. This is exactly the NLL from Section 1. For classification with one-hot labels, **Cross-Entropy Loss and NLL are the same thing** — they just arrive from different directions.

### Softmax + Cross-Entropy

Raw model outputs (logits) are unconstrained real numbers. Softmax converts them into a valid probability distribution:

$$\text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}$$

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 110" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="110" fill="none"/>

  <!-- Logits box -->
  <rect x="20" y="20" width="130" height="70" rx="8" fill="#1a2a1a" stroke="#555" stroke-width="1"/>
  <text x="85" y="38" text-anchor="middle" font-size="11" font-weight="600" fill="#aaa">Logits</text>
  <text x="85" y="54" text-anchor="middle" font-size="10" fill="#888">[2.1, 5.3, 0.7]</text>
  <text x="85" y="68" text-anchor="middle" font-size="10" fill="#888">unconstrained</text>
  <text x="85" y="82" text-anchor="middle" font-size="10" fill="#888">real numbers</text>

  <!-- Arrow + Softmax -->
  <line x1="150" y1="55" x2="218" y2="55" stroke="#7F77DD" stroke-width="1.5" marker-end="url(#as)"/>
  <rect x="218" y="36" width="90" height="38" rx="8" fill="#2a1a3a" stroke="#7F77DD" stroke-width="1.2"/>
  <text x="263" y="52" text-anchor="middle" font-size="11" font-weight="600" fill="#AFA9EC">Softmax</text>
  <text x="263" y="66" text-anchor="middle" font-size="9" fill="#CECBF6">e^zi / Σe^zj</text>

  <!-- Arrow -->
  <line x1="308" y1="55" x2="378" y2="55" stroke="#7F77DD" stroke-width="1.5" marker-end="url(#as)"/>

  <!-- Probabilities box -->
  <rect x="378" y="20" width="130" height="70" rx="8" fill="#1a2a1a" stroke="#1D9E75" stroke-width="1"/>
  <text x="443" y="38" text-anchor="middle" font-size="11" font-weight="600" fill="#5DCAA5">Probabilities</text>
  <text x="443" y="54" text-anchor="middle" font-size="10" fill="#9FE1CB">[0.05, 0.92, 0.03]</text>
  <text x="443" y="68" text-anchor="middle" font-size="10" fill="#9FE1CB">sums to 1.0</text>
  <text x="443" y="82" text-anchor="middle" font-size="10" fill="#9FE1CB">all positive</text>

  <!-- Arrow -->
  <line x1="508" y1="55" x2="578" y2="55" stroke="#E24B4A" stroke-width="1.5" marker-end="url(#ac)"/>

  <!-- Loss box -->
  <rect x="578" y="30" width="90" height="50" rx="8" fill="#3a1010" stroke="#E24B4A" stroke-width="1.2"/>
  <text x="623" y="50" text-anchor="middle" font-size="11" font-weight="600" fill="#F09595">CE Loss</text>
  <text x="623" y="66" text-anchor="middle" font-size="9" fill="#F7C1C1">-log Q(correct)</text>

  <defs>
    <marker id="as" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#7F77DD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="ac" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 6. The standard pipeline: raw logits → Softmax → probabilities → Cross-Entropy Loss.
</figcaption>
</figure>

### Binary vs Categorical Cross-Entropy

**Binary Cross-Entropy** applies when the output is a single probability — predicting whether a stock will rise or fall:

$$H = -\left[y \log q + (1-y)\log(1-q)\right]$$

**Categorical Cross-Entropy** applies for multi-class problems — predicting bull, bear, or sideways:

$$H = -\sum_{c=1}^{C} y_c \log q_c$$

Binary Cross-Entropy is simply the two-class special case of Categorical Cross-Entropy.

### Why the Logarithm Matters

The log function's nonlinearity is the whole point.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 280" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="280" fill="none"/>

  <!-- Axes -->
  <line x1="80" y1="240" x2="620" y2="240" stroke="#555" stroke-width="1.2"/>
  <line x1="80" y1="20"  x2="80"  y2="240" stroke="#555" stroke-width="1.2"/>

  <!-- Labels -->
  <text x="350" y="268" text-anchor="middle" font-size="13" fill="#aaa">Predicted probability q (correct class)</text>
  <text x="22" y="135" text-anchor="middle" font-size="13" fill="#aaa" transform="rotate(-90,22,135)">Loss = -log(q)</text>

  <!-- X ticks -->
  <text x="80"  y="255" text-anchor="middle" font-size="10" fill="#888">0</text>
  <text x="188" y="255" text-anchor="middle" font-size="10" fill="#888">0.2</text>
  <text x="296" y="255" text-anchor="middle" font-size="10" fill="#888">0.4</text>
  <text x="404" y="255" text-anchor="middle" font-size="10" fill="#888">0.6</text>
  <text x="512" y="255" text-anchor="middle" font-size="10" fill="#888">0.8</text>
  <text x="610" y="255" text-anchor="middle" font-size="10" fill="#888">1.0</text>

  <!-- Y ticks -->
  <text x="70" y="244" text-anchor="end" font-size="10" fill="#888">0</text>
  <text x="70" y="194" text-anchor="end" font-size="10" fill="#888">1</text>
  <text x="70" y="144" text-anchor="end" font-size="10" fill="#888">2</text>
  <text x="70" y="94"  text-anchor="end" font-size="10" fill="#888">3</text>
  <text x="70" y="44"  text-anchor="end" font-size="10" fill="#888">4</text>

  <!-- Grid -->
  <line x1="80" y1="194" x2="620" y2="194" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="144" x2="620" y2="144" stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="94"  x2="620" y2="94"  stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>
  <line x1="80" y1="44"  x2="620" y2="44"  stroke="#333" stroke-width="0.5" stroke-dasharray="4,3"/>

  <!-- -log(q) curve -->
  <polyline fill="none" stroke="#E24B4A" stroke-width="2.2" stroke-linejoin="round"
    points="
      86,235
      102,213
      134,194
      188,166
      242,148
      296,134
      350,122
      404,112
      458,103
      512,95
      566,88
      610,240
    "
  />
  <!-- Correct endpoint at q=1 -->
  <circle cx="610" cy="240" r="5" fill="#1D9E75"/>
  <text x="560" y="232" font-size="10" fill="#1D9E75">q=0.99 → Loss≈0.01 ✓</text>

  <!-- Wrong endpoint -->
  <circle cx="86" cy="235" r="5" fill="#E24B4A"/>
  <text x="96" y="225" font-size="10" fill="#E24B4A">q=0.01 → Loss≈4.6 ✗</text>

  <!-- Midpoint -->
  <circle cx="350" cy="122" r="4" fill="#EF9F27"/>
  <text x="358" y="118" font-size="10" fill="#EF9F27">q=0.5 → Loss≈0.69</text>

  <!-- Shaded danger zone -->
  <rect x="80" y="20" width="160" height="220" fill="#E24B4A" fill-opacity="0.05" rx="4"/>
  <text x="160" y="36" text-anchor="middle" font-size="10" fill="#E24B4A">Danger zone</text>
  <text x="160" y="48" text-anchor="middle" font-size="10" fill="#E24B4A">(confident & wrong)</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 7. Cross-Entropy loss $-\log(q)$. The loss is near zero when the model correctly assigns high probability to the true class, but explodes when the model is confidently wrong.
</figcaption>
</figure>

| Predicted Probability | Loss $= -\log q$ | Verdict |
|---|---|---|
| 0.99 — confident and correct | **0.01** | ✅ |
| 0.50 — uncertain | **0.69** | |
| 0.10 — leaning wrong | **2.30** | |
| 0.01 — confident and wrong | **4.60** | ❌ |

Consider two models predicting a market crash. Model A assigns 51% probability and is wrong. Model B assigns 99% probability and is wrong. Both are wrong — but Model B should be penalized far more severely for its false confidence. The logarithm enforces this: the loss grows not linearly but explosively as the model's confidence in the wrong answer increases.

This connects directly back to the information-theoretic interpretation: a confident wrong prediction is maximally surprising under the true distribution — highest possible information content — so it receives the highest possible loss.

---

## 5. References

- Shannon, C. E. (1948). *A Mathematical Theory of Communication*. Bell System Technical Journal, 27(3), 379–423.
- Kullback, S., & Leibler, R. A. (1951). *On Information and Sufficiency*. The Annals of Mathematical Statistics, 22(1), 79–86.
- Bishop, C. M. (2006). *Pattern Recognition and Machine Learning*. Springer.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. [deeplearningbook.org](https://www.deeplearningbook.org)
- Karpathy, A. et al. *CS231n: Convolutional Neural Networks for Visual Recognition*. Stanford University. [cs231n.github.io](https://cs231n.github.io)
- Murphy, K. P. (2022). *Probabilistic Machine Learning: An Introduction*. MIT Press.
