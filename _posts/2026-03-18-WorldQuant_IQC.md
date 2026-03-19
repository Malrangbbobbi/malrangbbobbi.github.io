---
title: "WorldQuant IQC — What I Learned Placing 2nd in Korea"
date: 2026-03-18 09:00:00 +0900
categories: [quant, competition]
tags: [worldquant, iqc, alpha, brain, quant, competition, is-os, overfitting]
---

> When I was preparing for this competition, I had no seniors or experienced peers to turn to. I still remember how much I wished I could find even a small piece of useful information. I'm writing this in hopes that it reaches someone in that same position.

## Table of Contents

- [IQC 2026 Is Here](#iqc-2026-is-here)
- [What Is WorldQuant IQC?](#what-is-worldquant-iqc)
- [Competition Timeline](#competition-timeline-last-years-schedule--subject-to-change)
- [Early Game](#early-game--march-to-explore-april-to-execute)
- [Strategy 1 — IS/OS Structure](#strategy-1--understand-the-isos-structure-first)
- [Strategy 2 — The Thinking Process](#strategy-2--the-thinking-process-behind-building-an-alpha)
- [Strategy 3 — Know Your Data](#strategy-3--know-your-data-thoroughly)
- [Strategy 4 — Overfitting Checklist](#strategy-4--a-practical-checklist-for-catching-overfitting)
- [What This Post Doesn't Cover](#what-this-post-doesnt-cover)

---

I placed 2nd in Korea at last year's WorldQuant International Quant Championship (IQC).

I'm writing this as IQC 2026 is about to begin. There's a particular reason I felt compelled to. When I was preparing for the competition, I had no seniors or experienced peers to turn to for advice. I still remember how much I wished I could find even a small piece of useful information. So I'm putting this together as honestly as I can, in hopes that it reaches someone who's in that same position.

---

## IQC 2026 Is Here

**International Quant Championship 2026**, hosted by WorldQuant Brain, is now open.

The total prize pool is **$100,000 USD**. Outstanding participants may also receive opportunities to become a BRAIN Research Consultant, be considered for full-time researcher roles or internships, and — if selected for a national team — compete at the **International Final in Singapore in September 2026**.

For registration and full details, visit the official page:
👉 [IQC 2026 Official Page](https://worldquantbrain.com/iqc)

Korean-language webinars are available separately — I'd recommend registering early.

---

## What Is WorldQuant IQC?

It's a global quant competition hosted by WorldQuant. Participants design and submit alphas — predictive signals for stock returns — on the **WorldQuant Brain platform**. The core skill isn't coding ability; it's the capacity to **read financial data and understand market structure**.

---

## Competition Timeline (Last Year's Schedule — Subject to Change)

The schedule below is based on last year's competition. **The 2026 timeline may differ**, so always check official announcements.

- **Weekly Wednesday** online webinars (last year: 18:00–19:30 KST / this year I've heard it may shift to Tuesday or Wednesday at 20:00)
- Webinar recordings uploaded approximately 2 weeks later
- **Mid-May** — Preliminary round closes; first OS score release
- **Late June** — Main round ends
- **Mid-July** — Finals

The most critical moment in the timeline is the **OS score release in mid-May**. This is the point where your strategy needs to change entirely depending on where you stand.

---

## Early Game — March to Explore, April to Execute

The operators and data fields provided change every year. Reusing alphas from previous competitions is nearly impossible.

**March should be treated as a time for exploration and learning.** Experiment with operator combinations, understand the characteristics of data fields, and get a feel for correlational structures. Build that intuition first, then **shift into full alpha production from April onward**.

When I first opened the Brain platform, I genuinely didn't know where to start. Even using a single operator felt unfamiliar, and I had no sense of which data field combinations would produce meaningful signals. When my first alpha failed to meet even the basic submission criteria, I was honestly caught off guard. Rather than rushing forward, I spent time slowly working through how each operator actually functioned and how the data was structured.

More than trying to build a "good alpha" right away, **understanding what each piece of data is and how it behaves** turned out to be far more valuable.

---

## Strategy 1 — Understand the IS/OS Structure First

IQC scores are evaluated on two dimensions: **IS (In-Sample)** and **OS (Out-of-Sample)**.

If you think of the period used to train an alpha as the train window and the period used to validate it as the test window, the IS score shown during the competition is calculated over the IS period. OS is evaluated over a separate holdout window and is only revealed after submission.

A high IS score does not guarantee a high OS score. Optimizing purely for IS is a risky approach.

When OS scores are released in mid-May, you need to adjust your strategy based on where you stand.

### High IS, Low OS → "Volume Strategy"

The model is likely overfitting, and your ranking is probably lower than you'd like.

Rather than changing direction, the more realistic play is to **aggressively generate more alphas**. Explore a wider range of factors and try to win on volume. If you're already behind, reversing that with quality alone is difficult.

### Low IS, High OS → "Precision Strategy"

Your strategy direction is sound and the model is generalizing well.

In this case, do the opposite — **reduce your submission count** and focus on high-quality alphas. Forcing more submissions at this stage can actually hurt your score.

---

## Strategy 2 — The Thinking Process Behind Building an Alpha

When I built alphas, I didn't start by asking "which indicator should I use?" I started by asking **"why would this data have any relationship with future returns?"**

For example, rather than just plugging in a momentum indicator, I first thought about "what lookback period for momentum actually matters?" and "are there specific market environments where momentum tends to work?" That line of questioning became the idea, and the idea became the formula.

There were plenty of moments where I got stuck. When an idea I thought was solid didn't perform as expected on either the train or IS window, I spent a long time not knowing what was wrong. What helped wasn't swapping out indicators — it was **stepping back and reconsidering the structure itself**.

When you first start building alphas, there's a strong temptation to make them complex. Stacking multiple operators, layering in conditions, and fine-tuning the formula can feel like you're making something more precise. In practice, the alphas that actually worked tended to be built on **ideas that were simple and clear**. Adding complexity was often just another word for overfitting.

---

## Strategy 3 — Know Your Data Thoroughly

The Brain platform provides a wide variety of data fields, and each one has its own characteristics. An approach of "I used this field and the IS score looked good" won't hold up for long.

Before using any data field, I made sure to check the following.

**Data Coverage**
Coverage by data field is publicly available on the Brain platform. I prioritized fields that met a certain coverage threshold before using them. I also ran simulations and used various operators to directly analyze how much coverage was actually being captured in practice.

Taking the time to properly understand data before rushing to use it — I believe this habit ultimately determines the quality of your alphas.

---

## Strategy 4 — A Practical Checklist for Catching Overfitting

A good IS score doesn't mean a good alpha. I went through the following checks every time.

**Long/Short Balance**
Check whether the alpha is heavily skewed in one direction or overly dependent on market directionality. An alpha that is structurally betting on one side tends to be unstable over time.

**Period-by-Period Performance Decomposition**
Don't just look at overall returns. If performance is concentrated in a specific window, the alpha is likely environment-dependent. Separately examine periods of high volatility and periods with strong directional trends.

**Read Metrics, Don't Just Look at Them**
Don't treat Sharpe, Fitness, and Turnover as numbers to maximize. Ask yourself why these figures came out the way they did, whether the performance is structurally reproducible, and whether it reflects genuine edge or just luck.

What ultimately matters most in this competition is

> **not the ability to build good alphas, but the ability to identify them.**

---

## What This Post Doesn't Cover

Honestly, there are far more tips than what's written here. Detailed judgment calls around submission strategy, the intuition for reading an alpha, approaches that come into play toward the end of the competition — there are parts that are genuinely hard to put into words. Some of it can only be internalized through direct experience.

This post was written as lightly as possible, with the hope of helping first-time participants find their footing. The rest is yours to discover along the way 😄

Meeting people who are working toward the same goals is one of the more underrated parts of this competition. If there are any offline events or networking opportunities, I'd encourage you to show up. Simply talking with people who share the same questions often helps clarify your own thinking more than you'd expect.

I hope this post is useful to someone out there preparing for the competition. 🙂
