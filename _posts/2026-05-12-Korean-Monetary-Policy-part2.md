---
title: "Korea's Monetary Policy, Part 2: Transmission and New Challenges"
date: 2026-05-12 09:00:00 +0900
categories: [Economics, Monetary Policy]
tags: [transmission-mechanism, phillips-curve, financial-stability, bank-of-korea, macroprudential-policy, neutral-rate, lean-vs-clean, risk-taking-channel, credit-channel]
---

> Part 1 covered the Bank of Korea's decision-making process and the instruments it uses to enforce the base rate. This post addresses the harder question: what actually happens after the rate decision is made. How does a change in the overnight interbank rate eventually alter the cost of a mortgage, the price of an apartment, and the aggregate level of consumer prices — and why does that process take one to two years to run its full course?

## Table of Contents

- [0. The Post-Decision Question](#0-the-post-decision-question)
- [1. Six Paths from Rate to Reality](#1-six-paths-from-rate-to-reality)
- [2. The Timing Problem](#2-the-timing-problem)
- [3. Monetary Policy Doesn't Work Alone](#3-monetary-policy-doesnt-work-alone)
- [4. Three Challenges That Make the Toolkit Harder](#4-three-challenges-that-make-the-toolkit-harder)
- [References](#references)

---

## 0. The Post-Decision Question

The phrase "monetary policy transmission mechanism" describes the process by which a decision made in the Bank of Korea's boardroom — a 25 basis point change in the target rate — eventually shows up in the price of a bag of rice or the rate quoted on a construction loan. The causal chain is real and well-documented in aggregate, but the route is neither direct nor fast. The BOK's own report describes the mechanism as a "black box," and the phrase captures something genuine: the relative weight of each transmission channel and the speed at which effects accumulate vary across countries, across time, and across the state of the financial system at the moment the policy is applied.

Korea's financial structure shapes which channels dominate. As a small open economy with a bank-centered financial system, a large export sector, and one of the world's most heavily owner-occupied housing markets, Korea's transmission landscape differs from the United States or the eurozone in ways that matter for how the BOK thinks about its policy choices.

---

## 1. Six Paths from Rate to Reality

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 235" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="235" fill="none"/>

  <!-- Outer container -->
  <rect x="15" y="12" width="650" height="215" rx="8" fill="#111" stroke="#2a2a2a" stroke-width="1"/>

  <!-- Header -->
  <rect x="15" y="12" width="650" height="30" rx="8" fill="#161b22" stroke="#2a2a2a"/>
  <text x="100" y="32" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Channel</text>
  <text x="305" y="32" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Core mechanism</text>
  <text x="502" y="32" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Speed of effect</text>
  <text x="615" y="32" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Korea relevance</text>

  <!-- Vertical dividers -->
  <line x1="175" y1="12" x2="175" y2="227" stroke="#222" stroke-width="0.8"/>
  <line x1="430" y1="12" x2="430" y2="227" stroke="#222" stroke-width="0.8"/>
  <line x1="565" y1="12" x2="565" y2="227" stroke="#222" stroke-width="0.8"/>

  <!-- Row separators -->
  <line x1="15" y1="75" x2="665" y2="75" stroke="#1a1a1a" stroke-width="0.6"/>
  <line x1="15" y1="108" x2="665" y2="108" stroke="#1a1a1a" stroke-width="0.6"/>
  <line x1="15" y1="141" x2="665" y2="141" stroke="#1a1a1a" stroke-width="0.6"/>
  <line x1="15" y1="174" x2="665" y2="174" stroke="#1a1a1a" stroke-width="0.6"/>
  <line x1="15" y1="207" x2="665" y2="207" stroke="#1a1a1a" stroke-width="0.6"/>

  <!-- Row 1: Interest rate -->
  <text x="100" y="56" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">① Interest Rate</text>
  <text x="305" y="52" text-anchor="middle" font-size="9" fill="#555">Rate change → bank lending/deposit rates</text>
  <text x="305" y="65" text-anchor="middle" font-size="9" fill="#555">→ cost of borrowing → investment, consumption</text>
  <text x="502" y="58" text-anchor="middle" font-size="9" fill="#8b949e">Months</text>
  <text x="615" y="58" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">★★★ High</text>

  <!-- Row 2: Asset price -->
  <text x="100" y="92" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">② Asset Price</text>
  <text x="305" y="88" text-anchor="middle" font-size="9" fill="#555">Rate → equity, real estate prices → wealth effect,</text>
  <text x="305" y="101" text-anchor="middle" font-size="9" fill="#555">Tobin's q → consumption, investment</text>
  <text x="502" y="94" text-anchor="middle" font-size="9" fill="#8b949e">Months–1 year</text>
  <text x="615" y="94" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">★★★ High</text>

  <!-- Row 3: Exchange rate -->
  <text x="100" y="125" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">③ Exchange Rate</text>
  <text x="305" y="121" text-anchor="middle" font-size="9" fill="#555">Rate differential → capital flows → won/dollar</text>
  <text x="305" y="134" text-anchor="middle" font-size="9" fill="#555">→ export competitiveness, import prices</text>
  <text x="502" y="127" text-anchor="middle" font-size="9" fill="#8b949e">Fast (weeks)</text>
  <text x="615" y="127" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">★★★ High</text>

  <!-- Row 4: Expectations -->
  <text x="100" y="158" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">④ Expectations</text>
  <text x="305" y="154" text-anchor="middle" font-size="9" fill="#555">Credible commitment → inflation expectations</text>
  <text x="305" y="167" text-anchor="middle" font-size="9" fill="#555">→ wage/price-setting behavior → realized inflation</text>
  <text x="502" y="160" text-anchor="middle" font-size="9" fill="#8b949e">Immediate</text>
  <text x="615" y="160" text-anchor="middle" font-size="10" font-weight="700" fill="#EF9F27">★★ Medium</text>

  <!-- Row 5: Credit -->
  <text x="100" y="191" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">⑤ Credit</text>
  <text x="305" y="187" text-anchor="middle" font-size="9" fill="#555">Bank lending channel + balance sheet channel</text>
  <text x="305" y="200" text-anchor="middle" font-size="9" fill="#555">→ credit supply, external finance premium → lending</text>
  <text x="502" y="193" text-anchor="middle" font-size="9" fill="#8b949e">Months</text>
  <text x="615" y="193" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">★★★ High</text>

  <!-- Row 6: Risk-taking -->
  <text x="100" y="221" text-anchor="middle" font-size="10" font-weight="600" fill="#378ADD">⑥ Risk-Taking</text>
  <text x="305" y="217" text-anchor="middle" font-size="9" fill="#555">Prolonged low rates → search for yield → leverage</text>
  <text x="305" y="228" text-anchor="middle" font-size="9" fill="#555">→ asset bubbles, financial imbalances</text>
  <text x="502" y="221" text-anchor="middle" font-size="9" fill="#8b949e">Years</text>
  <text x="615" y="221" text-anchor="middle" font-size="10" font-weight="700" fill="#EF9F27">★★ Medium</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 1. The six transmission channels through which the BOK's base rate reaches economic outcomes. All six operate simultaneously; their relative weights depend on financial structure, the state of expectations, and the duration of the policy stance. Korea's bank-centered financial system and large export sector amplify the interest rate, credit, and exchange rate channels relative to capital market-heavy economies.
</figcaption>
</figure>

### The Interest Rate Channel

The most direct route from a rate decision to the real economy runs through the cost of borrowing. A base rate cut reduces the BOK's RP rate, which lowers the cost at which banks fund themselves in the overnight market. Banks pass part of that reduction through to their lending rates — mortgage rates, corporate loan rates, consumer credit — and raise their deposit rates less, or not at all. Lower borrowing costs reduce the hurdle for investment projects that were marginally unprofitable at the prior rate, and reduce the monthly payment on variable-rate loans already outstanding.

How much of this channel runs through market rates versus bank rates depends on the financial system's structure. In the United States, large corporations routinely finance themselves by issuing bonds and commercial paper directly into capital markets; the relevant rate for these firms is the corporate bond yield, not the bank lending rate. A Fed rate cut pushes down Treasury yields, which compress corporate bond spreads, which lower the cost of capital for large borrowers — with bank rates playing a secondary role. Korea's financial structure inverts this priority. Korean corporations, and especially small and medium enterprises, depend heavily on bank lending rather than direct capital market access. The interest rate channel here runs primarily through the banking system, which is why the BOK watches changes in bank lending rates and the spread between the base rate and commercial loan rates as closely as it watches any market indicator.

### The Asset Price Channel

Lower interest rates increase the present value of future cash flows, which pushes up the prices of stocks, bonds, and real estate. The mechanism operates through two distinct effects. The *wealth effect* works through household balance sheets: when apartment prices rise, homeowners feel wealthier and are more willing to consume, even if their labor income hasn't changed. The *Tobin's q effect* works through corporate investment: when the market value of a firm exceeds the replacement cost of its capital stock ($q > 1$), it is cheaper to build new capacity than to acquire it through the market, which incentivizes new investment.

In Korea, the housing market amplifies this channel substantially. Owner-occupancy rates are high, and housing represents a disproportionate share of household wealth relative to most OECD economies. A BOK rate cut that pushes apartment prices higher in Seoul or Busan is not a peripheral side effect — it is a primary transmission mechanism, affecting the consumption of millions of households through the wealth effect. This is also why the risk-taking channel (discussed below) and macroprudential regulation have become so central to how the BOK thinks about the longer-run consequences of monetary easing.

### The Exchange Rate Channel

For a small open economy that exports roughly 40–45% of its GDP, the exchange rate is not merely a financial variable — it is a direct price in the goods markets that determine output and inflation. A BOK rate cut relative to the US rate reduces the expected return on won-denominated assets, which causes capital outflows, which weakens the won against the dollar and other currencies. Won depreciation has two distinct effects: it makes Korean exports cheaper in foreign markets, which boosts export demand; and it raises the price of imported intermediate goods and consumer products, which pushes domestic inflation upward.

The exchange rate channel is among the fastest-acting of the six. Capital flows respond to interest rate differentials within days; the exchange rate adjusts quickly. The downstream effects on export orders and import prices take longer — shipping schedules, pricing contracts, and import substitution patterns create lags of months — but the initial financial market response is nearly immediate. This speed makes the channel valuable for short-run stabilization, and it also means that the BOK cannot ignore the FOMC. When the Federal Reserve raises rates aggressively, the resulting dollar strength puts downward pressure on the won regardless of domestic Korean conditions, creating imported inflation that the BOK may be forced to respond to even when domestic demand doesn't warrant tightening.

### The Expectations Channel

Inflation expectations are not merely a forecast — they are a cause. If firms expect prices to rise by 3% next year, they build that expectation into their pricing decisions this year, which contributes to the inflation they were expecting. If workers expect a 3% wage increase to compensate for expected inflation, their wage demands contribute to costs that firms then pass on to prices. Inflation is, in a meaningful sense, self-fulfilling. This is the expectations channel: the central bank's credible commitment to a particular inflation path influences the expectations that help generate that outcome.

The BOK has operated under an explicit inflation targeting regime since 1998, with the current target set at 2% (±0.5%) for the consumer price index. Inflation targeting works primarily through this channel. The target provides a coordination mechanism: if businesses, consumers, and wage-setters all believe the BOK will hold inflation near 2% over the medium term, their decisions will tend to produce inflation near 2% — reducing the amount of economic disruption required to maintain the target. When credibility is strong, monetary policy becomes partly self-executing.

The expectations channel is why the BOK's communication matters as much as its rate decisions. Forward guidance, press conference tone, and the governor's public statements all shape where expectations are anchored. An institution that loses credibility — because it misses its target repeatedly, or appears to respond to political pressure — faces a world where inflation expectations are unmoored, and where achieving any particular inflation outcome requires much larger policy movements than a credible institution would need.

### The Credit Channel

The credit channel describes how monetary policy affects the *supply* of credit, not just its price. It operates through two distinct sub-mechanisms that are worth distinguishing carefully.

The *bank lending channel* operates on the supply side of bank balance sheets. When the BOK raises rates, banks' funding costs increase — both because the cost of overnight reserves rises and because the rate the bank must pay to attract retail deposits adjusts upward. Banks respond by tightening their lending standards and reducing credit supply, independent of any effect on loan demand. Borrowers who would have obtained credit at the prior rate may find it unavailable at the new rate.

The *balance sheet channel* operates on the demand side, through the financial health of borrowers. When the BOK raises rates, asset prices fall. A firm or household whose collateral is worth less is perceived as a riskier borrower; lenders demand higher spreads to compensate for the increased default probability. The key concept here is the *external finance premium* — the additional cost, above the risk-free rate, that a borrower must pay to obtain external funding relative to what it would cost to finance internally. When borrower balance sheets are strong, this premium is small; when they deteriorate following a rate hike, the premium expands, amplifying the tightening effect of the rate move itself. The credit channel is procyclical by nature: it amplifies rate hikes during downturns, when borrower balance sheets are already under stress, and it amplifies rate cuts during expansions, when collateral values are rising.

Korea's large SME sector and its historic dependence on bank financing make both sub-channels significant. Small firms that cannot access capital markets, and that depend on their business assets as collateral, are especially exposed to balance sheet channel dynamics.

### The Risk-Taking Channel

The final channel was largely overlooked in monetary policy frameworks before the 2008 global financial crisis. A prolonged period of low interest rates compresses the returns available on conventional safe assets — government bonds, high-grade corporate debt — to the point where investors and financial institutions find themselves unable to meet return targets, actuarial assumptions, or profitability benchmarks without accepting more risk. The response is *search for yield*: a systematic migration toward higher-risk assets, higher leverage, and longer-duration positions. Asset prices inflate, credit spreads compress, and financial imbalances accumulate.

The risk-taking channel connects monetary policy directly to financial stability. A rate cut that was intended to stimulate consumption and investment also, over a long enough horizon, builds the financial excesses that generate the next crisis. The empirical relevance of this channel in Korea is visible in the housing market dynamics of 2020–2021: the BOK's pandemic-era rate cuts to 0.5% — the lowest in the institution's history — coincided with one of the sharpest housing price surges on record in Seoul and other major cities. The rate eventually normalized; the financial imbalances proved more durable.

---

## 2. The Timing Problem

A decision made in the boardroom today will not reach the price level for one to two years. That lag creates a fundamental challenge: the monetary policy stance appropriate for conditions twelve months from now must be decided before anyone can observe what those conditions will be.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 155" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="155" fill="none"/>

  <defs>
    <marker id="arr-lag" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#378ADD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>

  <!-- Timeline spine -->
  <line x1="60" y1="75" x2="640" y2="75" stroke="#333" stroke-width="1.5" marker-end="url(#arr-lag)"/>

  <!-- Stage 1: Decision -->
  <circle cx="80" cy="75" r="14" fill="#1a2d42" stroke="#378ADD" stroke-width="2"/>
  <text x="80" y="79" text-anchor="middle" font-size="10" font-weight="700" fill="#378ADD">①</text>
  <text x="80" y="108" text-anchor="middle" font-size="9.5" font-weight="600" fill="#8b949e">Rate Decision</text>
  <text x="80" y="121" text-anchor="middle" font-size="9" fill="#555">Day 0</text>

  <!-- Stage 2: Financial markets -->
  <circle cx="230" cy="75" r="14" fill="#1a2d42" stroke="#378ADD" stroke-width="2"/>
  <text x="230" y="79" text-anchor="middle" font-size="10" font-weight="700" fill="#378ADD">②</text>
  <text x="230" y="108" text-anchor="middle" font-size="9.5" font-weight="600" fill="#8b949e">Financial Markets</text>
  <text x="230" y="121" text-anchor="middle" font-size="9" fill="#555">Days – weeks</text>
  <text x="230" y="46" text-anchor="middle" font-size="8.5" fill="#555">exchange rate,</text>
  <text x="230" y="57" text-anchor="middle" font-size="8.5" fill="#555">bond yields, equities</text>

  <!-- Stage 3: Credit & asset prices -->
  <circle cx="400" cy="75" r="14" fill="#1a2d42" stroke="#378ADD" stroke-width="2"/>
  <text x="400" y="79" text-anchor="middle" font-size="10" font-weight="700" fill="#378ADD">③</text>
  <text x="400" y="108" text-anchor="middle" font-size="9.5" font-weight="600" fill="#8b949e">Credit & Asset Prices</text>
  <text x="400" y="121" text-anchor="middle" font-size="9" fill="#555">Months – 1 year</text>
  <text x="400" y="46" text-anchor="middle" font-size="8.5" fill="#555">bank lending rates,</text>
  <text x="400" y="57" text-anchor="middle" font-size="8.5" fill="#555">housing prices, credit supply</text>

  <!-- Stage 4: Real economy & prices -->
  <circle cx="570" cy="75" r="14" fill="#1a2d42" stroke="#1D9E75" stroke-width="2"/>
  <text x="570" y="79" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">④</text>
  <text x="570" y="108" text-anchor="middle" font-size="9.5" font-weight="600" fill="#8b949e">Output & Inflation</text>
  <text x="570" y="121" text-anchor="middle" font-size="9" fill="#555">1 – 2 years</text>
  <text x="570" y="46" text-anchor="middle" font-size="8.5" fill="#555">consumption, investment,</text>
  <text x="570" y="57" text-anchor="middle" font-size="8.5" fill="#555">price level</text>

  <!-- Connecting spans -->
  <text x="155" y="88" text-anchor="middle" font-size="8" fill="#333">immediate</text>
  <text x="315" y="88" text-anchor="middle" font-size="8" fill="#333">weeks–months</text>
  <text x="485" y="88" text-anchor="middle" font-size="8" fill="#333">months–1 year</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 2. The four-stage transmission lag from rate decision to price level. Financial markets respond in days to weeks; credit conditions and asset prices adjust over months; the full effect on output and inflation takes one to two years. Monetary policy must therefore be set based on forecasts of future conditions, not on current observations.
</figcaption>
</figure>

The four-stage lag creates a fundamental mismatch. By the time the price level responds to a rate cut taken today, the economic conditions that justified the cut may have completely reversed. A central bank that waits until inflation is clearly too high before raising rates will see the impact of those hikes arrive when the inflationary episode is already easing — adding contractionary force at the wrong moment and amplifying rather than smoothing the cycle. This is the *pro-cyclical* failure mode: policy that follows rather than leads economic conditions ends up reinforcing them.

The prescription is *preemptive* policy: the MPB votes on where it expects conditions to be in twelve to eighteen months, not on where they are today. This is easier to state as a principle than to execute. Forecasting economic conditions eighteen months ahead involves serious uncertainty even with the BOK's full modeling infrastructure; the relationship between the current policy rate and future inflation is itself uncertain and variable. Different MPB members may hold different views about the lag structure, the current output gap, or the appropriate inflation forecast — which is precisely what the minutes are designed to make visible.

One thing that confused me early on was the difference between *preemptive* and *premature* as descriptions of the same policy action. They are, literally, the same decision viewed from different positions in time. A rate hike taken before inflation has materialized looks preemptive if inflation eventually appears and is contained; it looks premature if the economy turns down and the inflation risk evaporates. The distinction is not analytical — it is retrospective. This is what makes central banking genuinely difficult: the decisions are made under uncertainty that cannot be resolved at the time of the decision, and the quality of the judgment is visible only after the lag has run its course.

---

## 3. Monetary Policy Doesn't Work Alone

A single interest rate is not a precise instrument. It affects every borrower and every asset class simultaneously, without distinguishing between borrowers who pose systemic risk and those who don't, or between asset price appreciation that reflects genuine productivity growth and appreciation driven by leverage. Two other policy frameworks do the finer-grained work: fiscal policy sets the government's taxing and spending position, and macroprudential policy targets specific financial stability risks that the policy rate cannot address surgically.

### The Policy Mix

The interaction between monetary and fiscal policy is conventionally summarized in four combinations.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 480 240" xmlns="http://www.w3.org/2000/svg" style="max-width:480px; width:100%;">
  <rect width="480" height="240" fill="none"/>

  <!-- Grid -->
  <rect x="50" y="40" width="180" height="90" rx="4" fill="#1a2d1a" stroke="#1D9E75" stroke-width="1"/>
  <rect x="250" y="40" width="180" height="90" rx="4" fill="#2a1a1a" stroke="#E24B4A" stroke-width="1"/>
  <rect x="50" y="140" width="180" height="90" rx="4" fill="#1a2220" stroke="#378ADD" stroke-width="1"/>
  <rect x="250" y="140" width="180" height="90" rx="4" fill="#1a1a1a" stroke="#444" stroke-width="1"/>

  <!-- Axis labels -->
  <text x="140" y="25" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">Fiscal Expansion</text>
  <text x="340" y="25" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e">Fiscal Contraction</text>

  <!-- Y-axis labels -->
  <text x="38" y="90" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e" transform="rotate(-90 38 90)">Monetary Easy</text>
  <text x="38" y="190" text-anchor="middle" font-size="11" font-weight="600" fill="#8b949e" transform="rotate(-90 38 190)">Monetary Tight</text>

  <!-- Q1: Easy + Expansion -->
  <text x="140" y="72" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">Strong stimulus</text>
  <text x="140" y="88" text-anchor="middle" font-size="9" fill="#555">Growth ↑, inflation risk ↑</text>
  <text x="140" y="103" text-anchor="middle" font-size="9" fill="#555">Useful in deep recessions</text>
  <text x="140" y="118" text-anchor="middle" font-size="9" fill="#555">Korea: 2009, 2020</text>

  <!-- Q2: Tight + Expansion -->
  <text x="340" y="72" text-anchor="middle" font-size="10" font-weight="700" fill="#E24B4A">Fiscal dominance risk</text>
  <text x="340" y="88" text-anchor="middle" font-size="9" fill="#555">Rate ↑ crowds out private</text>
  <text x="340" y="103" text-anchor="middle" font-size="9" fill="#555">spending; debt risk ↑</text>
  <text x="340" y="118" text-anchor="middle" font-size="9" fill="#555">Conflicting signals</text>

  <!-- Q3: Easy + Contraction -->
  <text x="140" y="172" text-anchor="middle" font-size="10" font-weight="700" fill="#378ADD">Private sector stimulus</text>
  <text x="140" y="188" text-anchor="middle" font-size="9" fill="#555">Low rates boost investment</text>
  <text x="140" y="203" text-anchor="middle" font-size="9" fill="#555">while government deleverages</text>
  <text x="140" y="218" text-anchor="middle" font-size="9" fill="#555">Structural reform window</text>

  <!-- Q4: Tight + Contraction -->
  <text x="340" y="172" text-anchor="middle" font-size="10" font-weight="700" fill="#8b949e">Hard landing risk</text>
  <text x="340" y="188" text-anchor="middle" font-size="9" fill="#555">Inflation suppressed but</text>
  <text x="340" y="203" text-anchor="middle" font-size="9" fill="#555">growth falls sharply</text>
  <text x="340" y="218" text-anchor="middle" font-size="9" fill="#555">Rarely sustainable</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 3. The four monetary-fiscal policy combinations and their typical macroeconomic effects. The optimal mix depends on the position in the business cycle, the level of public debt, and the degree of economic slack. Central bank independence is a precondition for the monetary stance to be set on economic rather than fiscal grounds.
</figcaption>
</figure>

The four combinations are not equivalently available to policymakers. A government with high debt and limited fiscal space cannot easily choose fiscal expansion even when the cycle calls for it; a central bank whose independence is in question may not be able to hold a contractionary stance when a fiscally dominant government prefers lower rates. Korea's fiscal position has historically been conservative by OECD standards — a relatively low public debt-to-GDP ratio has preserved fiscal space that some peer economies have exhausted — which has made the coordinated easy-easy combination during the 2009 and 2020 downturns genuinely expansionary rather than merely offsetting.

### Macroprudential Policy

Macroprudential tools address what monetary policy cannot: targeted risks in specific segments of the financial system. Where the base rate affects all borrowers simultaneously, instruments like the loan-to-value ratio (LTV), the debt service ratio (DSR), and the countercyclical capital buffer (CCyB) can be applied selectively to the parts of the system where risk is accumulating.

The LTV cap limits the fraction of a property's value that can be financed with debt. In Korea, LTV rules have been actively adjusted throughout the 2000s and 2010s to manage housing market cycles, with stricter caps imposed in areas designated as speculative zones during periods of rapid price appreciation. The DSR — which replaced the narrower DTI measure — caps the fraction of annual income that can be absorbed by all debt service obligations, including mortgages, personal loans, and card balances. Unlike LTV, which targets the asset side, DSR targets the income constraint of the borrower, which makes it a more comprehensive measure of repayment capacity.

The CCyB is the most explicitly countercyclical of the three. Banks are required to accumulate additional capital buffers during credit expansions — which limits credit growth at the margin and builds resilience — and can release those buffers during downturns to support continued lending. The BOK and the Financial Services Commission coordinate on these instruments, which are formally separate from monetary policy but interact with it directly: a period of low interest rates that stimulates credit growth may trigger LTV tightening or a positive CCyB requirement, partially offsetting the monetary easing through the credit channel.

---

## 4. Three Challenges That Make the Toolkit Harder

The instruments described in this post and its predecessor were designed for an economic environment that has gradually shifted beneath them. Three structural developments have made the toolkit progressively harder to use.

### The Flattening Phillips Curve

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 560 280" xmlns="http://www.w3.org/2000/svg" style="max-width:560px; width:100%;">
  <rect width="560" height="280" fill="none"/>

  <!-- Axes -->
  <line x1="60" y1="250" x2="520" y2="250" stroke="#333" stroke-width="1.2"/>
  <line x1="60" y1="250" x2="60" y2="25" stroke="#333" stroke-width="1.2"/>

  <!-- X axis label -->
  <text x="290" y="270" text-anchor="middle" font-size="11" fill="#555">Unemployment rate →</text>

  <!-- Y axis label -->
  <text x="18" y="140" text-anchor="middle" font-size="11" fill="#555" transform="rotate(-90 18 140)">Inflation →</text>

  <!-- X axis ticks -->
  <line x1="135" y1="248" x2="135" y2="252" stroke="#444" stroke-width="1"/>
  <text x="135" y="262" text-anchor="middle" font-size="9" fill="#444">3%</text>
  <line x1="210" y1="248" x2="210" y2="252" stroke="#444" stroke-width="1"/>
  <text x="210" y="262" text-anchor="middle" font-size="9" fill="#444">4%</text>
  <line x1="248" y1="248" x2="248" y2="252" stroke="#444" stroke-width="1"/>
  <text x="248" y="262" text-anchor="middle" font-size="9" fill="#444">4.5%</text>
  <line x1="360" y1="248" x2="360" y2="252" stroke="#444" stroke-width="1"/>
  <text x="360" y="262" text-anchor="middle" font-size="9" fill="#444">6%</text>
  <line x1="435" y1="248" x2="435" y2="252" stroke="#444" stroke-width="1"/>
  <text x="435" y="262" text-anchor="middle" font-size="9" fill="#444">7%</text>

  <!-- Y axis ticks -->
  <line x1="58" y1="195" x2="62" y2="195" stroke="#444" stroke-width="1"/>
  <text x="50" y="199" text-anchor="end" font-size="9" fill="#444">2%</text>
  <line x1="58" y1="140" x2="62" y2="140" stroke="#444" stroke-width="1"/>
  <text x="50" y="144" text-anchor="end" font-size="9" fill="#444">4%</text>
  <line x1="58" y1="85" x2="62" y2="85" stroke="#444" stroke-width="1"/>
  <text x="50" y="89" text-anchor="end" font-size="9" fill="#444">6%</text>

  <!-- Natural rate vertical dashed line -->
  <line x1="248" y1="30" x2="248" y2="250" stroke="#555" stroke-width="0.8" stroke-dasharray="4,3"/>
  <text x="248" y="22" text-anchor="middle" font-size="9" fill="#555">Uₙ = 4.5%</text>

  <!-- Target inflation horizontal dashed line at p=2% (y=195) -->
  <line x1="62" y1="195" x2="520" y2="195" stroke="#EF9F27" stroke-width="0.8" stroke-dasharray="4,3"/>
  <text x="525" y="199" text-anchor="start" font-size="9" fill="#EF9F27">2% target</text>

  <!-- Steep curve (traditional): M 98,58 Q 248,140 473,236 -->
  <path d="M 98,52 Q 248,132 480,228" fill="none" stroke="#E24B4A" stroke-width="2.2"/>

  <!-- Flat curve (recent): M 98,154 Q 248,181 473,209 -->
  <path d="M 98,158 Q 248,185 480,208" fill="none" stroke="#378ADD" stroke-width="2.2"/>

  <!-- Labels for curves -->
  <text x="88" y="44" font-size="10" font-weight="600" fill="#E24B4A">Traditional SRPC</text>
  <text x="88" y="56" font-size="9" fill="#E24B4A">(pre-2000)</text>
  <text x="88" y="170" font-size="10" font-weight="600" fill="#378ADD">Flattened SRPC</text>
  <text x="88" y="182" font-size="9" fill="#378ADD">(post-2000)</text>

  <!-- Annotation at natural rate -->
  <!-- Steep at Un: y≈132, p≈(250-132)/27.5≈4.3% -->
  <!-- Flat at Un: y≈185, p≈(250-185)/27.5≈2.4% -->
  <circle cx="248" cy="132" r="4" fill="#E24B4A"/>
  <circle cx="248" cy="185" r="4" fill="#378ADD"/>

  <!-- Double-headed arrow showing the gap at Un -->
  <line x1="260" y1="133" x2="260" y2="184" stroke="#8b949e" stroke-width="0.8"/>
  <line x1="255" y1="133" x2="265" y2="133" stroke="#8b949e" stroke-width="0.8"/>
  <line x1="255" y1="184" x2="265" y2="184" stroke="#8b949e" stroke-width="0.8"/>
  <text x="310" y="155" text-anchor="middle" font-size="8.5" fill="#8b949e">At natural rate:</text>
  <text x="310" y="167" text-anchor="middle" font-size="8.5" fill="#E24B4A">Traditional → ~4.3% inflation</text>
  <text x="310" y="179" text-anchor="middle" font-size="8.5" fill="#378ADD">Flattened → ~2.4% inflation</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 4. Phillips curve flattening. The traditional short-run Phillips curve (SRPC) implied that achieving full employment would push inflation well above the 2% target, giving the central bank room to tighten before the economy overheated. The flattened curve means that even at the natural unemployment rate, inflation barely clears the target — requiring far larger adjustments in unemployment to produce the same change in the price level, and making the central bank's job of anchoring inflation substantially harder.
</figcaption>
</figure>

The Phillips curve — the empirical relationship between unemployment and inflation — has become flatter over the past three decades. In the 1960s and 1970s, a significant decline in unemployment reliably produced a significant rise in inflation. Since the 2000s, that relationship has weakened: unemployment can fall to historic lows without inflation responding proportionately. During Korea's post-pandemic recovery and in the US expansion of 2015–2019, labor markets tightened substantially without generating the inflation surge that older models predicted.

The causes are not fully resolved. Globalization has made tradeable goods prices partly a function of global supply chains rather than domestic demand, weakening the link between local labor market tightness and consumer prices. Technological change has compressed margins and reduced pricing power in large parts of the economy. And — somewhat circularly — the credibility of inflation targeting regimes has anchored inflation expectations so firmly that firms and workers no longer adjust their pricing behavior as aggressively when unemployment falls.

The practical consequence is asymmetric. A flat Phillips curve means that achieving a given inflation increase requires a much larger reduction in unemployment than a steep curve would imply. For a central bank trying to push inflation up to its 2% target — as the BOK found itself trying to do in the mid-2010s — this means more aggressive monetary easing, held for longer, with less assurance that the effect will materialize. And for a central bank trying to reduce inflation, the cost in unemployment terms is higher than traditional models suggested.

### Demographic Headwinds and the Declining Neutral Rate

The *neutral rate* — the real interest rate consistent with the economy operating at potential with stable inflation — has been declining across major economies for decades. Korea, with one of the world's fastest population aging trajectories, faces this problem in an acute form.

The mechanism runs through savings and investment. An aging population, anticipating retirement, tends to save more and invest less in new productive capacity. Higher aggregate saving relative to investment reduces the real return to capital in equilibrium. If the neutral rate falls toward zero — or into negative territory — the BOK's conventional tools have less room to work. A nominal rate cut that would have been 100 basis points below neutral in 2000 may now be only 50 basis points below neutral, with a smaller stimulative effect. And when the next recession arrives, the distance from the current rate to the zero lower bound is shorter, making it more likely that the BOK would exhaust conventional space and need to consider the non-conventional instruments discussed in Part 1.

Measuring the neutral rate in real time is inherently uncertain — it is not directly observable and must be inferred from structural models that themselves depend on contested assumptions about potential output and the natural unemployment rate. But the direction of the trend in Korea is not seriously disputed: demographic pressure is structurally biasing the neutral rate downward, and that trend is likely to continue for at least another two decades.

### Global Spillovers and the Limits of Domestic Policy

A base rate set in Seoul is not insulated from decisions made in Washington, Frankfurt, or Tokyo. Korea is a small open economy with substantial external financing needs, deep integration into global capital markets, and an export sector that makes the exchange rate a first-order policy consideration. When the Federal Reserve raises rates aggressively — as it did in 2022–2023 — dollar-denominated assets become more attractive globally, capital flows out of emerging markets, and currencies like the won come under depreciation pressure. That depreciation raises import prices, which pushes domestic inflation upward, which may compel the BOK to tighten even in the absence of excess domestic demand.

The asymmetry is significant. The BOK cannot avoid responding to FOMC decisions that generate imported inflation; but it also cannot coordinate formally with the Fed in setting its own rate. What coordination exists runs through informal channels — BOK-Fed communication, G20 frameworks, and the bilateral currency swap lines that have become a standard part of the international monetary safety net. During the 2008 crisis and the 2020 pandemic, the Fed extended swap lines to the BOK, providing dollar liquidity to stabilize Korean financial markets when dollar funding dried up globally. The swap lines are a backstop against acute dollar shortage, not a mechanism for aligning policy stances, but their existence reflects how deeply interconnected the two institutions' operating environments have become.

The combination of these three challenges — a flatter Phillips curve, a lower neutral rate, and tighter coupling to global monetary conditions — means that the BOK's toolkit, already constrained by the zero lower bound in extreme circumstances, faces a more difficult operating environment than the generation of policymakers who designed the inflation targeting framework in 1998 would have anticipated. The instruments remain what they were: the base rate, open market operations, macroprudential tools, and communication. What has changed is the terrain on which they operate.

---

## References

- Bank of Korea (2017). *Monetary Policy in Korea*. Seoul: Bank of Korea. [bok.or.kr](https://www.bok.or.kr/portal/bbs/P0000602/view.do?nttId=234114&menuNo=200459)
- Bernanke, B. S., & Gertler, M. (1995). *Inside the Black Box: The Credit Channel of Monetary Policy Transmission*. Journal of Economic Perspectives, 9(4), 27–48.
- Borio, C., & Zhu, H. (2008). *Capital Regulation, Risk-Taking and Monetary Policy: A Missing Link in the Transmission Mechanism?* BIS Working Papers No. 268. [bis.org/publ/work268.htm](https://www.bis.org/publ/work268.htm)
- Laubach, T., & Williams, J. C. (2003). *Measuring the Natural Rate of Interest*. Review of Economics and Statistics, 85(4), 1063–1070.
- Obstfeld, M., Shambaugh, J. C., & Taylor, A. M. (2005). *The Trilemma in History: Tradeoffs among Exchange Rates, Monetary Policies, and Capital Mobility*. Review of Economics and Statistics, 87(3), 423–438.
- Svensson, L. E. O. (2017). *Cost-Benefit Analysis of Leaning Against the Wind*. Journal of Monetary Economics, 90, 193–213.
- IMF (2013). *The Interaction of Monetary and Macroprudential Policies*. IMF Policy Paper. [imf.org](https://www.imf.org/external/np/pp/eng/2013/012913.pdf)
