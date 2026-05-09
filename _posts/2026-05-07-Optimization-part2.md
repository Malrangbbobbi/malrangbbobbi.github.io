---
title: "Deep Learning Optimization Part 2: Backpropagation, Gradient Descent, and Modern Optimizers"
date: 2026-05-07 09:00:00 +0900
categories: [AI, deep-learning]
tags: [optimization, backpropagation, gradient-descent, sgd, momentum, adam, skip-connection, resnet]
---

> In Part 1, we derived Cross-Entropy Loss from first principles. Now the harder question: how do we actually minimize it? This post covers the full training loop — from Forward Propagation through Backpropagation, Gradient Descent, modern Optimizers, and the architectural trick that makes deep networks trainable at all.

## Table of Contents

- [0. The Problem We're Trying to Solve](#0-the-problem-were-trying-to-solve)
- [1. Loss Landscape](#1-loss-landscape)
- [2. Forward and Backpropagation](#2-forward-and-backpropagation)
- [3. Gradient Descent and SGD](#3-gradient-descent-and-sgd)
- [4. Optimizers](#4-optimizers)
- [5. Skip-Connection and ResNet](#5-skip-connection-and-resnet)
- [6. References](#6-references)

---

## 0. The Problem We're Trying to Solve

In Part 1, we worked through the mathematical foundations of how a neural network measures its own error. Starting from Shannon's information theory, we derived Cross-Entropy Loss — a principled way to quantify the gap between what a model predicts and what actually happens.

But defining the gap is only half the problem.

A quant model that knows it's wrong by 4.6 nats still needs to figure out what to do about it. Which parameters are responsible for the error? By how much should each one change? In which direction?

This is the optimization problem. And it turns out to be considerably harder than it looks.

The Loss Landscape — the surface that maps every possible set of parameters to a corresponding loss value — is rarely a smooth bowl with a single obvious minimum. It's more like a mountain range: full of valleys, ridges, flat plateaus, and deceptive local dips that look like the bottom but aren't. Gradient Descent is our compass for navigating this terrain, Backpropagation is the mechanism that makes it computationally feasible, and modern Optimizers are the engineering refinements that make it work reliably at scale.

This post covers all three — in the order they actually happen during training.

---

## 1. Loss Landscape

### What the Loss Surface Actually Looks Like

Every set of model parameters $\theta$ maps to a scalar loss value $\mathcal{L}(\theta)$. Visualizing this mapping — even approximately — reveals why optimization is non-trivial.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 260" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="260" fill="none"/>

  <!-- Left: smooth landscape -->
  <rect x="20" y="10" width="300" height="220" rx="10" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="170" y="30" text-anchor="middle" font-size="12" font-weight="600" fill="#8b949e">Smooth Landscape</text>

  <!-- U자형 계곡 곡선 (minimum이 아래) -->
  <path d="M40,50 Q90,70 120,110 Q150,160 170,190 Q190,160 220,110 Q250,70 280,50"
    fill="none" stroke="#1D9E75" stroke-width="2.5" stroke-linejoin="round"/>
  <path d="M40,50 Q90,70 120,110 Q150,160 170,190 Q190,160 220,110 Q250,70 280,50 L280,210 L40,210 Z"
    fill="#1D9E75" fill-opacity="0.08"/>

  <!-- Global Min은 계곡 바닥 (y값 큰 아래쪽) -->
  <circle cx="170" cy="190" r="6" fill="#1D9E75"/>
  <text x="170" y="181" text-anchor="middle" font-size="10" fill="#1D9E75">Global Min</text>

  <!-- 내려가는 경로 (y값 증가 = 화면에서 아래로) -->
  <polyline points="60,58 80,78 105,108 135,148 158,178 168,188"
    fill="none" stroke="#EF9F27" stroke-width="1.5" stroke-dasharray="5,3" marker-end="url(#ap1)"/>

  <text x="170" y="215" text-anchor="middle" font-size="10" fill="#8b949e">Easy to navigate — one clear minimum</text>

  <!-- Right: rugged landscape -->
  <rect x="360" y="10" width="300" height="220" rx="10" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="510" y="30" text-anchor="middle" font-size="12" font-weight="600" fill="#8b949e">Rugged Landscape</text>

  <!-- 울퉁불퉁한 곡선 — 계곡들이 아래쪽, 봉우리들이 위쪽 -->
  <path d="M380,50 Q395,55 410,75 Q422,140 430,145 Q440,70 455,85 Q465,145 475,150 Q485,70 500,80 Q512,180 525,185 Q540,80 555,100 Q570,60 590,55 Q610,50 630,50"
    fill="none" stroke="#E24B4A" stroke-width="2.5" stroke-linejoin="round"/>
  <path d="M380,50 Q395,55 410,75 Q422,140 430,145 Q440,70 455,85 Q465,145 475,150 Q485,70 500,80 Q512,180 525,185 Q540,80 555,100 Q570,60 590,55 Q610,50 630,50 L630,210 L380,210 Z"
    fill="#E24B4A" fill-opacity="0.06"/>

  <!-- Local min — 얕은 계곡들 (y=145, y=150) -->
  <circle cx="430" cy="145" r="5" fill="#EF9F27"/>
  <text x="430" y="136" text-anchor="middle" font-size="9" fill="#EF9F27">Local</text>

  <circle cx="475" cy="150" r="5" fill="#EF9F27"/>
  <text x="475" y="141" text-anchor="middle" font-size="9" fill="#EF9F27">Local</text>

  <!-- Global min — 가장 깊은 계곡 (y=185) -->
  <circle cx="525" cy="185" r="6" fill="#1D9E75"/>
  <text x="525" y="176" text-anchor="middle" font-size="10" fill="#1D9E75">Global Min</text>

  <!-- Optimizer가 local min에 갇히는 경로 -->
  <polyline points="388,55 398,80 410,120 420,140 430,145"
    fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="5,3"/>
  <text x="385" y="195" text-anchor="start" font-size="10" fill="#E24B4A">Optimizer gets stuck</text>
  <text x="385" y="208" text-anchor="start" font-size="10" fill="#E24B4A">in local minimum</text>

  <defs>
    <marker id="ap1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#EF9F27" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 2. Three obstacles in the loss landscape: local minima (trapped at suboptimal solutions), saddle points (gradient vanishes), and plateaus (no useful gradient signal).
</figcaption>
</figure>

Understanding these challenges motivates every design decision that follows — why we need Backpropagation, why vanilla Gradient Descent isn't enough, and why modern optimizers look the way they do.

---

## 2. Forward and Backpropagation

### The Training Loop

Before diving into the mechanics, it helps to see the full training loop in one place.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 100" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="100" fill="none"/>

  <!-- Steps -->
  <rect x="10"  y="25" width="120" height="50" rx="8" fill="#1a3a5c" stroke="#378ADD" stroke-width="1.2"/>
  <text x="70"  y="48" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Forward</text>
  <text x="70"  y="63" text-anchor="middle" font-size="10" fill="#B5D4F4">Propagation</text>

  <rect x="160" y="25" width="120" height="50" rx="8" fill="#1a3a1a" stroke="#1D9E75" stroke-width="1.2"/>
  <text x="220" y="48" text-anchor="middle" font-size="11" font-weight="600" fill="#5DCAA5">Loss</text>
  <text x="220" y="63" text-anchor="middle" font-size="10" fill="#9FE1CB">Computation</text>

  <rect x="310" y="25" width="120" height="50" rx="8" fill="#3a1a1a" stroke="#E24B4A" stroke-width="1.2"/>
  <text x="370" y="48" text-anchor="middle" font-size="11" font-weight="600" fill="#F09595">Backward</text>
  <text x="370" y="63" text-anchor="middle" font-size="10" fill="#F7C1C1">Propagation</text>

  <rect x="460" y="25" width="120" height="50" rx="8" fill="#2a1a3a" stroke="#7F77DD" stroke-width="1.2"/>
  <text x="520" y="48" text-anchor="middle" font-size="11" font-weight="600" fill="#AFA9EC">Parameter</text>
  <text x="520" y="63" text-anchor="middle" font-size="10" fill="#CECBF6">Update</text>

  <!-- Arrows -->
  <line x1="130" y1="50" x2="158" y2="50" stroke="#555" stroke-width="1.2" marker-end="url(#aloop)"/>
  <line x1="280" y1="50" x2="308" y2="50" stroke="#555" stroke-width="1.2" marker-end="url(#aloop)"/>
  <line x1="430" y1="50" x2="458" y2="50" stroke="#555" stroke-width="1.2" marker-end="url(#aloop)"/>

  <!-- Feedback loop -->
  <path d="M580,75 Q580,92 340,92 Q100,92 100,75" fill="none" stroke="#555" stroke-width="1" stroke-dasharray="4,3" marker-end="url(#aloop)"/>
  <text x="340" y="100" text-anchor="middle" font-size="9" fill="#555">repeat until convergence</text>

  <defs>
    <marker id="aloop" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#555" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 3. The training loop. Forward propagation produces a prediction; loss computation measures the error; backpropagation computes gradients; parameter update applies them.
</figcaption>
</figure>

### Forward Propagation

Forward propagation is the straightforward part. Input data flows through each layer sequentially, with each layer applying a linear transformation followed by a nonlinear activation:

$$a^{(l)} = \sigma\left(W^{(l)} a^{(l-1)} + b^{(l)}\right)$$

For a stock prediction model: raw market features (price history, volume, volatility) enter as $x$, pass through hidden layers that learn increasingly abstract representations, and exit as a probability distribution over outcomes — up, down, or sideways — after Softmax.

The output of forward propagation is a prediction. The gap between that prediction and the true label is the Cross-Entropy Loss we computed in Part 1. The question is now: **which parameters caused the error, and by how much?**

### Backpropagation and the Chain Rule

Backpropagation answers this question by computing the gradient of the loss with respect to every parameter in the network. This sounds expensive — a modern network might have hundreds of millions of parameters — but the Chain Rule makes it tractable.

#### The Core Idea

Consider a simplified two-layer computation:

$$x \xrightarrow{f} u \xrightarrow{g} \mathcal{L}$$

We want $\frac{\partial \mathcal{L}}{\partial x}$, but there's no direct path. The Chain Rule gives us:

$$\frac{\partial \mathcal{L}}{\partial x} = \frac{\partial \mathcal{L}}{\partial u} \cdot \frac{\partial u}{\partial x}$$

The gradient flows backwards through each computation — from loss to output to hidden layers to input weights — multiplying local gradients at each step.

#### Node-Level Backpropagation

Every operation in a neural network is a node. Each node receives a gradient from the right (from the loss), computes its local gradient, and passes the result to the left. The type of operation determines how the gradient is transformed.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 220" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="220" fill="none"/>

  <!-- Example: f(x,y,z) = (x+y)*z, x=-2, y=5, z=-4 -->
  <text x="340" y="20" text-anchor="middle" font-size="12" font-weight="600" fill="#8b949e">f(x, y, z) = (x + y) · z,  x=−2, y=5, z=−4</text>

  <!-- Input nodes -->
  <rect x="20" y="50" width="70" height="36" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="55" y="65" text-anchor="middle" font-size="11" fill="#85B7EB">x = −2</text>
  <text x="55" y="79" text-anchor="middle" font-size="10" fill="#378ADD">∂f/∂x = −4</text>

  <rect x="20" y="110" width="70" height="36" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="55" y="125" text-anchor="middle" font-size="11" fill="#85B7EB">y = 5</text>
  <text x="55" y="139" text-anchor="middle" font-size="10" fill="#378ADD">∂f/∂y = −4</text>

  <rect x="20" y="170" width="70" height="36" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="55" y="185" text-anchor="middle" font-size="11" fill="#85B7EB">z = −4</text>
  <text x="55" y="199" text-anchor="middle" font-size="10" fill="#378ADD">∂f/∂z = +3</text>

  <!-- Add node -->
  <circle cx="230" cy="100" r="28" fill="#1a2a3a" stroke="#7F77DD" stroke-width="1.5"/>
  <text x="230" y="97" text-anchor="middle" font-size="16" fill="#AFA9EC">+</text>
  <text x="230" y="112" text-anchor="middle" font-size="9" fill="#7F77DD">q = 3</text>

  <!-- Multiply node -->
  <circle cx="430" cy="130" r="28" fill="#1a2a3a" stroke="#E24B4A" stroke-width="1.5"/>
  <text x="430" y="127" text-anchor="middle" font-size="16" fill="#F09595">×</text>
  <text x="430" y="142" text-anchor="middle" font-size="9" fill="#E24B4A">f = −12</text>

  <!-- Output -->
  <rect x="550" y="112" width="80" height="36" rx="6" fill="#1a3a1a" stroke="#1D9E75" stroke-width="1"/>
  <text x="590" y="127" text-anchor="middle" font-size="11" fill="#5DCAA5">Loss</text>
  <text x="590" y="141" text-anchor="middle" font-size="10" fill="#1D9E75">∂f/∂f = 1</text>

  <!-- Forward arrows -->
  <line x1="90" y1="68" x2="200" y2="90" stroke="#378ADD" stroke-width="1" marker-end="url(#afwd)"/>
  <line x1="90" y1="128" x2="200" y2="108" stroke="#378ADD" stroke-width="1" marker-end="url(#afwd)"/>
  <line x1="90" y1="188" x2="400" y2="148" stroke="#378ADD" stroke-width="1" marker-end="url(#afwd)"/>
  <line x1="258" y1="108" x2="400" y2="130" stroke="#378ADD" stroke-width="1" marker-end="url(#afwd)"/>
  <line x1="458" y1="130" x2="548" y2="130" stroke="#378ADD" stroke-width="1" marker-end="url(#afwd)"/>

  <!-- Backward gradient labels -->
  <text x="340" y="108" text-anchor="middle" font-size="10" fill="#E24B4A">∂f/∂q = z = −4</text>
  <text x="160" y="76" text-anchor="middle" font-size="10" fill="#7F77DD">passes −4</text>
  <text x="155" y="170" text-anchor="middle" font-size="10" fill="#E24B4A">∂f/∂z = q = +3</text>

  <!-- Legend -->
  <line x1="20" y1="210" x2="50" y2="210" stroke="#378ADD" stroke-width="1.5"/>
  <text x="55" y="213" font-size="9" fill="#8b949e">Forward</text>
  <line x1="110" y1="210" x2="140" y2="210" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="4,2"/>
  <text x="145" y="213" font-size="9" fill="#8b949e">Backward (gradient)</text>

  <defs>
    <marker id="afwd" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#378ADD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 4. Backpropagation through a simple computation graph. Forward pass computes values left-to-right; backward pass propagates gradients right-to-left using local derivatives.
</figcaption>
</figure>

Two local gradient rules cover most of what happens in practice:

**Addition node** — the incoming gradient passes through unchanged to both inputs. If the loss gradient arriving at the addition node is $-4$, both $x$ and $y$ receive $-4$.

**Multiplication node** — the incoming gradient is scaled by the *other* input's value. In $q = x \cdot z$, the gradient flowing to $x$ is multiplied by $z$, and vice versa.

#### Why This Matters at Scale

In a deep network with $L$ layers, the gradient for parameters in layer 1 involves a product of $L$ local gradients:

$$\frac{\partial \mathcal{L}}{\partial W^{(1)}} = \frac{\partial \mathcal{L}}{\partial a^{(L)}} \cdot \frac{\partial a^{(L)}}{\partial a^{(L-1)}} \cdots \frac{\partial a^{(2)}}{\partial a^{(1)}} \cdot \frac{\partial a^{(1)}}{\partial W^{(1)}}$$

Each factor is a local gradient. If those factors are consistently less than 1 — which is common with certain activation functions — the product shrinks exponentially with depth. This is the **Vanishing Gradient** problem, and we'll address it directly in Section 5.

---

## 3. Gradient Descent and SGD

### The Update Rule

Once we have gradients, the parameter update is straightforward. Move each parameter in the direction that reduces loss:

$$\theta_{t+1} = \theta_t - \alpha \cdot \nabla_\theta \mathcal{L}(\theta_t)$$

where $\alpha$ is the **learning rate** — the step size at each iteration.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 240" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="240" fill="none"/>

  <!-- Axes -->
  <line x1="60" y1="210" x2="620" y2="210" stroke="#555" stroke-width="1.2"/>
  <line x1="60" y1="20"  x2="60"  y2="210" stroke="#555" stroke-width="1.2"/>
  <text x="340" y="232" text-anchor="middle" font-size="12" fill="#aaa">Parameters θ</text>
  <text x="18" y="115" text-anchor="middle" font-size="12" fill="#aaa" transform="rotate(-90,18,115)">Loss</text>

  <!-- Loss curve -->
  <path d="M80,60 Q150,180 250,195 Q340,205 430,160 Q500,120 560,55 Q590,35 610,30"
    fill="none" stroke="#378ADD" stroke-width="2.5" stroke-linejoin="round"/>

  <!-- Current position -->
  <circle cx="180" cy="188" r="7" fill="#EF9F27"/>
  <text x="180" y="178" text-anchor="middle" font-size="10" fill="#EF9F27">θ_t</text>

  <!-- Gradient arrow (tangent direction going up) -->
  <line x1="180" y1="188" x2="140" y2="175" stroke="#E24B4A" stroke-width="1.5" marker-end="url(#agrad)"/>
  <text x="130" y="168" text-anchor="end" font-size="10" fill="#E24B4A">gradient direction</text>

  <!-- Step arrow (opposite direction) -->
  <line x1="180" y1="188" x2="230" y2="198" stroke="#1D9E75" stroke-width="2" marker-end="url(#astep)"/>
  <text x="240" y="192" font-size="10" fill="#1D9E75">−α·∇L (step)</text>

  <!-- Next position -->
  <circle cx="250" cy="195" r="7" fill="#1D9E75"/>
  <text x="270" y="195" font-size="10" fill="#1D9E75">θ_{t+1}</text>

  <!-- Learning rate annotation -->
  <line x1="180" y1="215" x2="250" y2="215" stroke="#7F77DD" stroke-width="1" marker-end="url(#alr)" marker-start="url(#alr)"/>
  <text x="215" y="228" text-anchor="middle" font-size="10" fill="#7F77DD">α (learning rate)</text>

  <defs>
    <marker id="agrad" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="astep" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#1D9E75" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="alr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#7F77DD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 5. One gradient descent step. The gradient points uphill; we move in the opposite direction by a distance proportional to the learning rate α.
</figcaption>
</figure>

### The Learning Rate Problem

The learning rate $\alpha$ is deceptively important. Set it too small, and training takes forever — or gets permanently stuck in a plateau. Set it too large, and updates overshoot the minimum, causing the loss to oscillate or diverge entirely.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 180" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="180" fill="none"/>

  <!-- Three panels -->
  <!-- Too small -->
  <rect x="10" y="10" width="200" height="155" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="110" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">α too small</text>
  <path d="M30,130 Q80,60 110,40 Q140,60 180,130" fill="none" stroke="#555" stroke-width="1.5"/>
  <circle cx="110" cy="40" r="4" fill="#1D9E75"/>
  <polyline points="40,128 50,122 60,117 70,112 80,106 90,99 100,90 108,76 110,65 110,55 110,45" fill="none" stroke="#EF9F27" stroke-width="1.5" stroke-dasharray="4,2"/>
  <text x="110" y="152" text-anchor="middle" font-size="10" fill="#EF9F27">Too slow to converge</text>

  <!-- Just right -->
  <rect x="240" y="10" width="200" height="155" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="340" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">α just right</text>
  <path d="M260,130 Q310,60 340,40 Q370,60 410,130" fill="none" stroke="#555" stroke-width="1.5"/>
  <circle cx="340" cy="40" r="4" fill="#1D9E75"/>
  <polyline points="270,128 290,100 310,72 330,52 340,43" fill="none" stroke="#1D9E75" stroke-width="1.5"/>
  <text x="340" y="152" text-anchor="middle" font-size="10" fill="#1D9E75">Converges efficiently</text>

  <!-- Too large -->
  <rect x="470" y="10" width="200" height="155" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="570" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">α too large</text>
  <path d="M490,130 Q540,60 570,40 Q600,60 640,130" fill="none" stroke="#555" stroke-width="1.5"/>
  <circle cx="570" cy="40" r="4" fill="#1D9E75"/>
  <polyline points="500,128 640,128 500,110 640,110 500,95 640,90 530,75 610,68 555,55 585,50 570,44" fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="4,2"/>
  <text x="570" y="152" text-anchor="middle" font-size="10" fill="#E24B4A">Oscillates, may diverge</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 6. The effect of learning rate. Too small: convergence is painfully slow. Just right: efficient descent. Too large: updates overshoot, causing oscillation or divergence.
</figcaption>
</figure>

### Batch GD vs SGD vs Mini-Batch

The update rule raises a practical question: which data do we use to compute the gradient?

**Batch Gradient Descent** computes the gradient over the entire dataset before taking a single step. For a market model trained on ten years of daily returns, this means processing 2,500+ data points before updating any weight. The gradient is accurate, but the process is slow — and the memory requirement can be prohibitive.

**Stochastic Gradient Descent (SGD)** computes the gradient from a single randomly selected data point and updates immediately. This is fast and memory-efficient, but the gradient estimate is noisy — a single trading day might not be representative of the broader market regime.

**Mini-Batch Gradient Descent** — which most practitioners mean when they say "SGD" — splits the data into small batches (typically 32–512 samples) and updates once per batch. This balances estimation accuracy against computational speed, and the noise from batch sampling has a useful side effect: it helps the optimizer escape shallow local minima.

| Method | Data per Update | Gradient Quality | Practical Use |
|---|---|---|---|
| Batch GD | Full dataset | Exact | Rarely — too slow |
| SGD | 1 sample | Very noisy | Rarely — too unstable |
| **Mini-Batch GD** | 32–512 samples | Good approximation | **Standard practice** |

---

## 4. Optimizers

### Why Vanilla SGD Isn't Enough

Vanilla SGD has three well-known failure modes that become apparent when training deep models on complex data.

**Slow convergence in flat regions.** When the loss surface is nearly flat, gradients are small, and updates are tiny regardless of how far we are from the minimum.

**Oscillation in narrow valleys.** When the loss surface curves sharply in one direction and gently in another — common in real networks — SGD oscillates back and forth across the steep axis while making slow progress along the gentle one.

**Sensitivity to learning rate.** A fixed learning rate that works well early in training often becomes too large as the model approaches a minimum. Manually tuning a learning rate schedule is tedious and problem-specific.

Each optimizer below addresses one or more of these problems.

### Momentum

Momentum introduces the concept of velocity — an exponentially decaying average of past gradients — to smooth out the update trajectory.

$$v_{t+1} = \beta v_t + \alpha \nabla_\theta \mathcal{L}(\theta_t)$$
$$\theta_{t+1} = \theta_t - v_{t+1}$$

The hyperparameter $\beta$ (typically 0.9) controls how much of the previous velocity is retained. Think of it as a ball rolling down a hill: it accumulates speed in consistent directions and resists being deflected by noisy gradients.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 200" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="200" fill="none"/>

  <!-- SGD panel -->
  <rect x="20" y="10" width="290" height="170" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="165" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">Vanilla SGD — oscillates</text>

  <!-- Elliptical contours -->
  <ellipse cx="165" cy="105" rx="120" ry="60" fill="none" stroke="#333" stroke-width="0.8"/>
  <ellipse cx="165" cy="105" rx="80"  ry="38" fill="none" stroke="#333" stroke-width="0.8"/>
  <ellipse cx="165" cy="105" rx="40"  ry="18" fill="none" stroke="#333" stroke-width="0.8"/>
  <circle  cx="165" cy="105" r="4" fill="#1D9E75"/>

  <!-- SGD path — zigzag -->
  <polyline points="60,75 130,95 80,105 140,110 100,110 155,108 165,105"
    fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="4,2"/>

  <!-- Momentum panel -->
  <rect x="370" y="10" width="290" height="170" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="515" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">Momentum — smooth path</text>

  <!-- Elliptical contours -->
  <ellipse cx="515" cy="105" rx="120" ry="60" fill="none" stroke="#333" stroke-width="0.8"/>
  <ellipse cx="515" cy="105" rx="80"  ry="38" fill="none" stroke="#333" stroke-width="0.8"/>
  <ellipse cx="515" cy="105" rx="40"  ry="18" fill="none" stroke="#333" stroke-width="0.8"/>
  <circle  cx="515" cy="105" r="4" fill="#1D9E75"/>

  <!-- Momentum path — smooth curve -->
  <path d="M410,75 Q450,85 480,92 Q500,98 510,102 Q515,104 515,105"
    fill="none" stroke="#1D9E75" stroke-width="2"/>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 7. SGD (left) oscillates across the narrow axis of an elliptical loss surface. Momentum (right) accumulates velocity in the consistent direction, producing a smoother and faster path to the minimum.
</figcaption>
</figure>

For a stock prediction model trained on multivariate time series, momentum is particularly valuable: market features often have very different scales and correlations, producing exactly the kind of anisotropic loss surface where SGD oscillates badly.

### Nesterov Accelerated Gradient (NAG)

NAG is a refinement of Momentum with one key modification: instead of computing the gradient at the current position, it computes the gradient at the *anticipated* next position.

$$v_{t+1} = \beta v_t + \alpha \nabla_\theta \mathcal{L}(\theta_t - \beta v_t)$$
$$\theta_{t+1} = \theta_t - v_{t+1}$$

The term $\theta_t - \beta v_t$ is a lookahead: where would we be if we applied the current velocity? By computing the gradient there, NAG corrects its trajectory before overshooting rather than after. In practice, NAG converges faster than standard Momentum, especially near the minimum where corrections matter most.

### Adagrad

Momentum adapts the trajectory but uses the same learning rate for all parameters. Adagrad takes a different approach: it adapts the **learning rate per parameter**, based on the history of its gradients.

$$G_t = G_{t-1} + \left(\nabla_\theta \mathcal{L}\right)^2$$
$$\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{G_t + \epsilon}} \cdot \nabla_\theta \mathcal{L}$$

Parameters that receive large gradients frequently (common features) get smaller learning rates. Parameters that receive small gradients rarely (rare features) get larger learning rates. For a market model, this means high-frequency signals like daily volume naturally receive smaller updates than rare regime indicators.

The problem with Adagrad is that $G_t$ only grows over time — the learning rate shrinks monotonically and eventually becomes so small that the model stops learning entirely.

### RMSprop

RMSprop fixes Adagrad's accumulation problem by using an **exponentially decaying average** of squared gradients rather than a cumulative sum:

$$E[g^2]_t = \gamma E[g^2]_{t-1} + (1-\gamma)\left(\nabla_\theta \mathcal{L}\right)^2$$
$$\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{E[g^2]_t + \epsilon}} \cdot \nabla_\theta \mathcal{L}$$

The decay factor $\gamma$ (typically 0.9) ensures that old gradients are forgotten gradually. The learning rate remains adaptive but doesn't collapse over time.

### Adam

Adam (Adaptive Moment Estimation) combines the best of Momentum and RMSprop. It maintains both a first moment estimate (mean of gradients, like Momentum) and a second moment estimate (variance of gradients, like RMSprop):

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) \nabla_\theta \mathcal{L}$$
$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) \left(\nabla_\theta \mathcal{L}\right)^2$$

Both estimates are biased toward zero early in training (since they're initialized at zero), so Adam applies bias correction:

$$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}$$

The final update:

$$\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

Default values of $\beta_1 = 0.9$, $\beta_2 = 0.999$, $\epsilon = 10^{-8}$ work well across a remarkably wide range of problems, which is why Adam has become the default optimizer for most deep learning applications.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 160" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="160" fill="none"/>

  <!-- Timeline comparison of optimizers -->
  <text x="340" y="18" text-anchor="middle" font-size="12" font-weight="600" fill="#8b949e">Optimizer Convergence Comparison (conceptual)</text>

  <!-- Axes -->
  <line x1="60" y1="130" x2="640" y2="130" stroke="#555" stroke-width="1"/>
  <line x1="60" y1="30"  x2="60"  y2="130" stroke="#555" stroke-width="1"/>
  <text x="350" y="148" text-anchor="middle" font-size="11" fill="#aaa">Training Steps</text>
  <text x="20" y="80" text-anchor="middle" font-size="11" fill="#aaa" transform="rotate(-90,20,80)">Loss</text>

  <!-- SGD curve — slow, noisy -->
  <path d="M60,110 Q150,95 250,80 Q350,70 450,62 Q550,56 630,52"
    fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="5,3"/>

  <!-- Momentum curve -->
  <path d="M60,110 Q130,85 220,68 Q310,55 420,47 Q530,43 630,41"
    fill="none" stroke="#EF9F27" stroke-width="1.5" stroke-dasharray="5,3"/>

  <!-- Adam curve — fast -->
  <path d="M60,110 Q100,65 160,48 Q240,38 350,34 Q480,32 630,31"
    fill="none" stroke="#1D9E75" stroke-width="2"/>

  <!-- Legend -->
  <line x1="360" y1="110" x2="390" y2="110" stroke="#E24B4A" stroke-width="1.5" stroke-dasharray="5,3"/>
  <text x="395" y="114" font-size="10" fill="#E24B4A">SGD</text>

  <line x1="420" y1="110" x2="450" y2="110" stroke="#EF9F27" stroke-width="1.5" stroke-dasharray="5,3"/>
  <text x="455" y="114" font-size="10" fill="#EF9F27">Momentum</text>

  <line x1="520" y1="110" x2="550" y2="110" stroke="#1D9E75" stroke-width="2"/>
  <text x="555" y="114" font-size="10" fill="#1D9E75">Adam</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 8. Conceptual convergence comparison. Adam typically reaches low loss faster than SGD or Momentum, thanks to adaptive learning rates and momentum combined.
</figcaption>
</figure>

| Optimizer | Adapts LR? | Uses Momentum? | Key Limitation |
|---|---|---|---|
| SGD | No | No | Slow, sensitive to LR |
| Momentum | No | Yes | Fixed LR for all params |
| NAG | No | Yes (lookahead) | Same as Momentum |
| Adagrad | Yes | No | LR decays to zero |
| RMSprop | Yes | No | No first moment |
| **Adam** | **Yes** | **Yes** | **Rarely — industry standard** |

---

## 5. Skip-Connection and ResNet

### When Depth Becomes a Liability

Stacking more layers should, in principle, allow a network to learn more complex representations. In practice, adding layers beyond a certain depth makes training *worse* — not just slower, but actively worse even on the training data.

This was demonstrated empirically in the work that motivated ResNet: a 56-layer plain network performed worse than a 20-layer network, on both training and test sets. The network wasn't overfitting — it was failing to learn at all.

The culprit is the Vanishing Gradient problem. Recall from Section 2 that the gradient for early layers involves a product of $L$ local gradients. For a 56-layer network with gradients around 0.9 per layer:

$$0.9^{56} \approx 0.0018$$

By the time the gradient reaches the first layer, it has shrunk by a factor of 500. The update for early parameters is effectively zero — those layers learn nothing.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 160" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="160" fill="none"/>

  <!-- Gradient magnitude across layers -->
  <line x1="60" y1="130" x2="620" y2="130" stroke="#555" stroke-width="1"/>
  <line x1="60" y1="20"  x2="60"  y2="130" stroke="#555" stroke-width="1"/>
  <text x="340" y="150" text-anchor="middle" font-size="11" fill="#aaa">Layer (output → input)</text>
  <text x="18" y="75" text-anchor="middle" font-size="11" fill="#aaa" transform="rotate(-90,18,75)">Gradient magnitude</text>

  <!-- Vanishing gradient curve -->
  <path d="M80,35 Q150,36 220,40 Q300,50 380,72 Q460,100 540,120 Q590,128 610,130"
    fill="none" stroke="#E24B4A" stroke-width="2"/>
  <path d="M80,35 Q150,36 220,40 Q300,50 380,72 Q460,100 540,120 Q590,128 610,130 L610,130 L80,130 Z"
    fill="#E24B4A" fill-opacity="0.06"/>

  <!-- Skip-connection gradient curve -->
  <path d="M80,35 Q200,36 340,37 Q480,38 610,40"
    fill="none" stroke="#1D9E75" stroke-width="2" stroke-dasharray="6,3"/>

  <!-- Labels -->
  <text x="200" y="30" font-size="10" fill="#1D9E75">With Skip-Connection — gradient preserved</text>
  <text x="400" y="115" font-size="10" fill="#E24B4A">Without — gradient vanishes</text>

  <!-- Layer markers -->
  <text x="80"  y="143" text-anchor="middle" font-size="9" fill="#888">L</text>
  <text x="610" y="143" text-anchor="middle" font-size="9" fill="#888">1</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 9. Gradient magnitude across layers. Without skip-connections, the gradient vanishes exponentially toward early layers. Skip-connections maintain a near-constant gradient signal throughout the network.
</figcaption>
</figure>

### The Skip-Connection Solution

The ResNet solution is elegant: add a direct path — a **skip-connection** or **identity shortcut** — that carries the input $x$ around each block of layers, and add it to the block's output.

$$\text{output} = \mathcal{F}(x) + x$$

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 200" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="200" fill="none"/>

  <!-- Plain network -->
  <rect x="20" y="10" width="280" height="170" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="160" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">Plain Network</text>

  <rect x="60"  y="45" width="160" height="35" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="140" y="67" text-anchor="middle" font-size="11" fill="#85B7EB">Weight Layer</text>

  <rect x="60"  y="100" width="160" height="35" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="140" y="122" text-anchor="middle" font-size="11" fill="#85B7EB">Weight Layer</text>

  <line x1="140" y1="40"  x2="140" y2="45"  stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>
  <line x1="140" y1="80"  x2="140" y2="100" stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>
  <line x1="140" y1="135" x2="140" y2="160" stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>
  <text x="140" y="178" text-anchor="middle" font-size="10" fill="#aaa">F(x)</text>
  <text x="140" y="190" text-anchor="middle" font-size="10" fill="#E24B4A">gradient can vanish</text>

  <!-- ResNet block -->
  <rect x="370" y="10" width="290" height="170" rx="8" fill="#0d1117" stroke="#30363d" stroke-width="1"/>
  <text x="515" y="28" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">ResNet Block</text>

  <rect x="410" y="45" width="160" height="35" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="490" y="67" text-anchor="middle" font-size="11" fill="#85B7EB">Weight Layer</text>

  <rect x="410" y="100" width="160" height="35" rx="6" fill="#1a3a5c" stroke="#378ADD" stroke-width="1"/>
  <text x="490" y="122" text-anchor="middle" font-size="11" fill="#85B7EB">Weight Layer</text>

  <!-- Main path -->
  <line x1="490" y1="40"  x2="490" y2="45"  stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>
  <line x1="490" y1="80"  x2="490" y2="100" stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>
  <line x1="490" y1="135" x2="490" y2="150" stroke="#555" stroke-width="1.2" marker-end="url(#asc)"/>

  <!-- Skip connection -->
  <path d="M390,40 Q385,95 390,155" fill="none" stroke="#1D9E75" stroke-width="2" stroke-dasharray="5,3" marker-end="url(#askip)"/>
  <text x="375" y="100" text-anchor="end" font-size="10" fill="#1D9E75">x</text>
  <text x="367" y="112" text-anchor="end" font-size="10" fill="#1D9E75">(identity)</text>

  <!-- Add node -->
  <circle cx="490" cy="155" r="10" fill="#2a1a3a" stroke="#7F77DD" stroke-width="1.5"/>
  <text x="490" y="160" text-anchor="middle" font-size="14" fill="#AFA9EC">+</text>

  <text x="490" y="180" text-anchor="middle" font-size="10" fill="#1D9E75">F(x) + x</text>
  <text x="490" y="192" text-anchor="middle" font-size="10" fill="#1D9E75">gradient always ≥ 1</text>

  <defs>
    <marker id="asc" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#555" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="askip" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#1D9E75" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 10. Plain network (left) vs ResNet block (right). The identity shortcut bypasses the weight layers and adds x directly to F(x), ensuring gradients can always flow backward.
</figcaption>
</figure>

### Why It Works

The gradient of the skip-connection output with respect to the input is:

$$\frac{\partial}{\partial x}\left(\mathcal{F}(x) + x\right) = \frac{\partial \mathcal{F}(x)}{\partial x} + 1$$

The $+1$ term guarantees that the gradient is always at least 1 — it cannot vanish, regardless of what $\frac{\partial \mathcal{F}(x)}{\partial x}$ does. Early layers receive a meaningful gradient signal even in networks with hundreds of layers.

The name "residual" in ResNet comes from reframing what the block learns. Rather than learning the full transformation from input to output, the block learns the **residual** — the difference between the desired output and the identity:

$$\mathcal{F}(x) = \text{desired output} - x$$

If the optimal transformation is close to the identity (a common situation early in training), this is much easier to learn than the full mapping.

### Effect on the Loss Landscape

Skip-connections don't just fix the gradient problem — they fundamentally smooth the loss landscape. A 56-layer plain network (VGG-style) produces a chaotic, jagged surface with many sharp local minima. Adding skip-connections to produce an equivalent ResNet-56 smooths the surface considerably. Increasing skip-connection density (as in DenseNet) smooths it further still.

This directly impacts optimizer performance: the techniques from Section 4 work best on smooth, well-conditioned surfaces. Skip-connections make the loss landscape the kind of terrain that gradient-based optimizers were designed for.

---

## 6. References

- Rumelhart, D. E., Hinton, G. E., & Williams, R. J. (1986). *Learning representations by back-propagating errors*. Nature, 323, 533–536.
- Kingma, D. P., & Ba, J. (2014). *Adam: A Method for Stochastic Optimization*. ICLR 2015.
- He, K., Zhang, X., Ren, S., & Sun, J. (2016). *Deep Residual Learning for Image Recognition*. CVPR 2016.
- Duchi, J., Hazan, E., & Singer, Y. (2011). *Adaptive Subgradient Methods for Online Learning and Stochastic Optimization*. JMLR.
- Nesterov, Y. (1983). *A method for solving the convex programming problem with convergence rate O(1/k²)*. Soviet Mathematics Doklady.
- Li, H. et al. (2018). *Visualizing the Loss Landscape of Neural Nets*. NeurIPS 2018.
- Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press. [deeplearningbook.org](https://www.deeplearningbook.org)
- Karpathy, A. et al. *CS231n: Convolutional Neural Networks for Visual Recognition*. Stanford University. [cs231n.github.io](https://cs231n.github.io)
