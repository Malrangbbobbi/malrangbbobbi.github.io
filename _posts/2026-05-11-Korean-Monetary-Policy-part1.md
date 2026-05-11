---
title: "Korea's Monetary Policy, Part 1: The Toolkit"
date: 2026-05-11 09:00:00 +0900
categories: [Economics, Monetary Policy]
tags: [bank-of-korea, monetary-policy, base-rate, open-market-operations, forward-guidance, quantitative-easing, interest-rate-corridor, repurchase-agreement]
---

> Eight times a year, seven people vote on a single number. That number — the Bank of Korea's base rate — sets the cost of overnight interbank borrowing, which cascades outward into mortgage rates, corporate bond yields, and the exchange rate. This post covers how those decisions get made, what instruments the Bank of Korea uses to enforce them, and what happens when the conventional toolkit runs out. The material draws throughout on the Bank of Korea's *[Monetary Policy in Korea](https://www.bok.or.kr/portal/bbs/P0000602/view.do?nttId=234114&menuNo=200459)* (2017 edition), the institution's own systematic account of its framework and operations.

## Table of Contents

- [0. Eight Times a Year](#0-eight-times-a-year)
- [1. The Decision Room](#1-the-decision-room)
- [2. Three Tools, One Goal](#2-three-tools-one-goal)
- [3. The Word Is Also a Tool](#3-the-word-is-also-a-tool)
- [4. When the Floor Disappears](#4-when-the-floor-disappears)
- [References](#references)

---

## 0. Eight Times a Year

The Bank of Korea's Monetary Policy Board (금융통화위원회, MPB) meets eight times per year to vote on the base rate. The calendar is fixed in advance: January, February, April, May, July, August, October, and November. In the alternating months — March, June, September, December — the Board convenes a separate Financial Stability Review that does not produce a rate decision. Emergency sessions outside the scheduled calendar are possible; the BOK held one in March 2020.

The Federal Reserve's FOMC also meets eight times per year. The structural similarity ends there.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 700 195" xmlns="http://www.w3.org/2000/svg" style="max-width:700px; width:100%;">
  <rect width="700" height="195" fill="none"/>

  <!-- Column background alternates for readability -->
  <rect x="75" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>
  <rect x="179" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>
  <rect x="283" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>
  <rect x="387" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>
  <rect x="491" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>
  <rect x="595" y="10" width="52" height="165" rx="0" fill="#0d1117" opacity="0.4"/>

  <!-- Month labels -->
  <text x="101" y="28" text-anchor="middle" font-size="10" fill="#555">Jan</text>
  <text x="153" y="28" text-anchor="middle" font-size="10" fill="#555">Feb</text>
  <text x="205" y="28" text-anchor="middle" font-size="10" fill="#555">Mar</text>
  <text x="257" y="28" text-anchor="middle" font-size="10" fill="#555">Apr</text>
  <text x="309" y="28" text-anchor="middle" font-size="10" fill="#555">May</text>
  <text x="361" y="28" text-anchor="middle" font-size="10" fill="#555">Jun</text>
  <text x="413" y="28" text-anchor="middle" font-size="10" fill="#555">Jul</text>
  <text x="465" y="28" text-anchor="middle" font-size="10" fill="#555">Aug</text>
  <text x="517" y="28" text-anchor="middle" font-size="10" fill="#555">Sep</text>
  <text x="569" y="28" text-anchor="middle" font-size="10" fill="#555">Oct</text>
  <text x="621" y="28" text-anchor="middle" font-size="10" fill="#555">Nov</text>
  <text x="673" y="28" text-anchor="middle" font-size="10" fill="#555">Dec</text>

  <!-- Horizontal separators -->
  <line x1="70" y1="40" x2="700" y2="40" stroke="#222" stroke-width="0.5"/>
  <line x1="70" y1="100" x2="700" y2="100" stroke="#222" stroke-width="0.5"/>
  <line x1="70" y1="160" x2="700" y2="160" stroke="#222" stroke-width="0.5"/>

  <!-- Row labels -->
  <text x="65" y="75" text-anchor="end" font-size="11" font-weight="600" fill="#8b949e">BOK</text>
  <text x="65" y="135" text-anchor="end" font-size="11" font-weight="600" fill="#8b949e">FOMC</text>

  <!-- BOK row — Monetary Policy (filled blue): Jan, Feb, Apr, May, Jul, Aug, Oct, Nov -->
  <circle cx="101" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="101" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <circle cx="153" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="153" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK row — Financial Stability (muted): Mar -->
  <circle cx="205" cy="70" r="11" fill="#1a1a1a" stroke="#3a3a3a" stroke-width="1.2"/>
  <text x="205" y="74" text-anchor="middle" font-size="8" fill="#444">FS</text>

  <!-- BOK Apr -->
  <circle cx="257" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="257" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK May -->
  <circle cx="309" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="309" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK Jun — FS -->
  <circle cx="361" cy="70" r="11" fill="#1a1a1a" stroke="#3a3a3a" stroke-width="1.2"/>
  <text x="361" y="74" text-anchor="middle" font-size="8" fill="#444">FS</text>

  <!-- BOK Jul -->
  <circle cx="413" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="413" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK Aug -->
  <circle cx="465" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="465" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK Sep — FS -->
  <circle cx="517" cy="70" r="11" fill="#1a1a1a" stroke="#3a3a3a" stroke-width="1.2"/>
  <text x="517" y="74" text-anchor="middle" font-size="8" fill="#444">FS</text>

  <!-- BOK Oct -->
  <circle cx="569" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="569" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK Nov -->
  <circle cx="621" cy="70" r="11" fill="#1a2d42" stroke="#378ADD" stroke-width="1.8"/>
  <text x="621" y="74" text-anchor="middle" font-size="9" font-weight="700" fill="#378ADD">MP</text>

  <!-- BOK Dec — FS -->
  <circle cx="673" cy="70" r="11" fill="#1a1a1a" stroke="#3a3a3a" stroke-width="1.2"/>
  <text x="673" y="74" text-anchor="middle" font-size="8" fill="#444">FS</text>

  <!-- FOMC row — Jan, Mar, May, Jun, Jul, Sep, Oct, Dec -->
  <circle cx="101" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="101" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Feb — no FOMC -->
  <circle cx="153" cy="130" r="11" fill="#111" stroke="#222" stroke-width="0.8" stroke-dasharray="2,2"/>

  <!-- Mar -->
  <circle cx="205" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="205" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Apr — no FOMC -->
  <circle cx="257" cy="130" r="11" fill="#111" stroke="#222" stroke-width="0.8" stroke-dasharray="2,2"/>

  <!-- May -->
  <circle cx="309" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="309" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Jun -->
  <circle cx="361" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="361" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Jul -->
  <circle cx="413" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="413" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Aug — no FOMC -->
  <circle cx="465" cy="130" r="11" fill="#111" stroke="#222" stroke-width="0.8" stroke-dasharray="2,2"/>

  <!-- Sep -->
  <circle cx="517" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="517" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Oct -->
  <circle cx="569" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="569" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Nov — no FOMC -->
  <circle cx="621" cy="130" r="11" fill="#111" stroke="#222" stroke-width="0.8" stroke-dasharray="2,2"/>

  <!-- Dec -->
  <circle cx="673" cy="130" r="11" fill="#132a1e" stroke="#1D9E75" stroke-width="1.8"/>
  <text x="673" y="134" text-anchor="middle" font-size="8" font-weight="700" fill="#1D9E75">◆</text>

  <!-- Legend -->
  <circle cx="160" cy="178" r="7" fill="#1a2d42" stroke="#378ADD" stroke-width="1.5"/>
  <text x="172" y="182" font-size="10" fill="#8b949e">BOK — Monetary Policy</text>

  <circle cx="320" cy="178" r="7" fill="#1a1a1a" stroke="#3a3a3a" stroke-width="1"/>
  <text x="332" y="182" font-size="10" fill="#555">BOK — Financial Stability</text>

  <circle cx="490" cy="178" r="7" fill="#132a1e" stroke="#1D9E75" stroke-width="1.5"/>
  <text x="502" y="182" font-size="10" fill="#8b949e">FOMC meeting</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 1. BOK and FOMC meeting schedules across the year. The BOK holds monetary policy meetings (MP) in eight months; in the remaining four, it holds Financial Stability reviews (FS) without a rate decision. The FOMC is distributed more evenly, skipping February, April, August, and November. Both institutions meet eight times annually for rate decisions.
</figcaption>
</figure>

FOMC sessions run across two days; the rate decision is announced on the afternoon of the second day, Eastern time. BOK MPB meetings are single-day sessions — the base rate is decided and announced the same morning, with the governor's press conference immediately following. The FOMC releases full minutes three weeks after each meeting; the BOK releases minutes two weeks after, on the second Tuesday following the session.

Both calendars matter together. Korean financial markets track FOMC decisions closely because US rate movements affect capital flows into and out of Korea, the dollar-won exchange rate, and the BOK's own room to maneuver. A Fed rate hike that strengthens the dollar puts depreciation pressure on the won, which can import inflation — creating conditions where the BOK may feel pressure to tighten even when domestic fundamentals don't clearly call for it.

The base rate itself is a *target*, not a directly transacted rate. The Board votes on where overnight interbank lending should price; the instruments described in the next section are what actually push market rates toward that target. The decision and its implementation are separate acts.

---

## 1. The Decision Room

The Monetary Policy Board has seven members. The BOK governor chairs and holds a casting vote in the event of a tie, though ties are rare in practice. The remaining six members — including the senior deputy governor and five externally-appointed members serving four-year terms — are nominated through separate channels across government and financial sector bodies. No single appointing authority controls a majority.

The week before a rate decision follows a structured sequence that is mostly invisible to the public. Working-level staff from the BOK's major departments hold internal briefings throughout the week. On the day preceding the Board meeting, a trends briefing (동향보고회의) presents an integrated review of domestic and international economic and financial conditions to all Board members. The Board meeting itself begins at 9 AM. After deliberation and a vote, the governor holds a press conference to explain the decision and its rationale. The full minutes — which record how each member voted, what data weighed heaviest in deliberation, and what dissenting views were raised — are published on the second Tuesday following the meeting.

For those tracking the BOK's reaction function, the minutes are often more informative than the rate decision itself. By the morning of the meeting, market consensus has usually anticipated the outcome. What the minutes reveal is the distribution of opinion within the Board, how close the vote actually was, and what indicators would need to shift for the majority position to change. A 5-2 decision to hold communicates something quite different about the next meeting than a 7-0 decision to hold.

Three principles run through how the BOK frames monetary policy governance: independence from political instruction, accountability for decisions taken, and transparency about the reasoning behind them. These are not independent of each other. Transparency creates accountability by making inadequate reasoning harder to hide; accountability reinforces independence by making it costly to appear responsive to political pressure. An institution that explains itself publicly in detail is harder to quietly lean on.

One thing that confused me early on was how formal BOK independence reconciles with the fact that the governor is a Presidential appointee. The answer is that appointment and instruction are separate things. The governor serves a fixed four-year term and cannot be removed for policy disagreements. The Ministry of Economy and Finance may send a non-voting observer to Board meetings, but cannot instruct the Board or veto its decisions. In practice, the separation is real but not impermeable — in periods of fiscal stress, the line between coordination with the government and direction by it becomes a matter of institutional culture rather than formal rule.

---

## 2. Three Tools, One Goal

When the Board sets the base rate at a particular level, it is setting a *target*. The overnight interbank rate — the rate at which commercial banks lend reserve balances to each other for one day — is what the BOK actually tries to pin to that target. Three instruments do the work: open market operations, standing facilities, and reserve requirements. In practice, the first carries almost all of the load.

### Open Market Operations

The BOK's primary instrument is the repurchase agreement (환매조건부채권, RP). An RP is a collateralized short-term transaction: the BOK purchases securities from a financial institution with an agreement to sell them back at a specified price on a specified date. The cash transfer in the initial leg raises the institution's reserve balance at the BOK, expanding reserve supply in the interbank market and pushing the overnight rate toward the target. RP sales do the reverse.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 680 210" xmlns="http://www.w3.org/2000/svg" style="max-width:680px; width:100%;">
  <rect width="680" height="210" fill="none"/>

  <defs>
    <marker id="arr-rp1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#378ADD" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="arr-rp2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#1D9E75" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="arr-rp3" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#E24B4A" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
    <marker id="arr-rp4" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#EF9F27" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>

  <!-- Section labels -->
  <text x="340" y="22" text-anchor="middle" font-size="11" font-weight="600" fill="#378ADD">RP Purchase (매입) — Liquidity Supply</text>
  <line x1="30" y1="105" x2="650" y2="105" stroke="#222" stroke-width="0.8" stroke-dasharray="4,3"/>
  <text x="340" y="122" text-anchor="middle" font-size="11" font-weight="600" fill="#E24B4A">RP Sale (매도) — Liquidity Absorption</text>

  <!-- BOK box (left) -->
  <rect x="30" y="35" width="110" height="50" rx="6" fill="#1a2d42" stroke="#378ADD" stroke-width="1.2"/>
  <text x="85" y="56" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Bank of</text>
  <text x="85" y="71" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Korea</text>

  <!-- FI box (right) — top section -->
  <rect x="540" y="35" width="110" height="50" rx="6" fill="#1a2d42" stroke="#378ADD" stroke-width="1.2"/>
  <text x="595" y="56" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Financial</text>
  <text x="595" y="71" text-anchor="middle" font-size="11" font-weight="600" fill="#85B7EB">Institution</text>

  <!-- Cash arrow: BOK → FI -->
  <line x1="145" y1="55" x2="536" y2="55" stroke="#1D9E75" stroke-width="1.5" marker-end="url(#arr-rp2)"/>
  <text x="340" y="49" text-anchor="middle" font-size="10" fill="#1D9E75">Cash injected into banking system</text>

  <!-- Securities arrow: FI → BOK -->
  <line x1="536" y1="70" x2="145" y2="70" stroke="#8b949e" stroke-width="1.2" marker-end="url(#arr-rp1)" stroke-dasharray="4,2"/>
  <text x="340" y="84" text-anchor="middle" font-size="10" fill="#555">Securities pledged as collateral (returned at maturity)</text>

  <!-- RP Sale section -->
  <!-- BOK box (left) bottom -->
  <rect x="30" y="130" width="110" height="50" rx="6" fill="#2a1a1a" stroke="#E24B4A" stroke-width="1.2"/>
  <text x="85" y="151" text-anchor="middle" font-size="11" font-weight="600" fill="#E07070">Bank of</text>
  <text x="85" y="166" text-anchor="middle" font-size="11" font-weight="600" fill="#E07070">Korea</text>

  <!-- FI box (right) bottom -->
  <rect x="540" y="130" width="110" height="50" rx="6" fill="#2a1a1a" stroke="#E24B4A" stroke-width="1.2"/>
  <text x="595" y="151" text-anchor="middle" font-size="11" font-weight="600" fill="#E07070">Financial</text>
  <text x="595" y="166" text-anchor="middle" font-size="11" font-weight="600" fill="#E07070">Institution</text>

  <!-- Securities arrow: BOK → FI (sale) -->
  <line x1="145" y1="150" x2="536" y2="150" stroke="#EF9F27" stroke-width="1.5" marker-end="url(#arr-rp4)" stroke-dasharray="4,2"/>
  <text x="340" y="144" text-anchor="middle" font-size="10" fill="#EF9F27">Securities sold to market</text>

  <!-- Cash arrow: FI → BOK (absorption) -->
  <line x1="536" y1="165" x2="145" y2="165" stroke="#E24B4A" stroke-width="1.5" marker-end="url(#arr-rp3)"/>
  <text x="340" y="180" text-anchor="middle" font-size="10" fill="#E24B4A">Cash withdrawn from banking system</text>

  <!-- Self-unwinding note -->
  <text x="340" y="200" text-anchor="middle" font-size="9" fill="#444">Both operations self-unwind at maturity — 7-day RP is the BOK's standard tenor</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 2. RP purchase (매입) injects reserves into the banking system by exchanging cash for collateral; RP sale (매도) absorbs reserves by selling securities into the market. Both transactions self-unwind at maturity. The BOK conducts 7-day RPs at the base rate as its primary liquidity management instrument.
</figcaption>
</figure>

The BOK runs 7-day RPs as its primary liquidity management instrument, with the RP rate set at the base rate. This design is self-reinforcing: banks can always obtain 7-day reserves from the BOK at the base rate, which means the overnight interbank rate cannot sustain a significant premium above the target without market participants substituting toward BOK facilities and arbitraging the spread away.

For draining structural excess liquidity — the accumulated balance sheet effect of years of net reserve injection — the BOK issues Monetary Stabilization Bonds (통화안정증권, MSB) with maturities from 91 days to two years. Unlike RPs, which self-unwind when the agreement matures, MSBs absorb liquidity until the bonds expire or the BOK conducts offsetting operations. RPs manage short-term fluctuations; MSBs manage the underlying structural level.

During financial crises, the operational scope of open market operations expands. Central banks globally have widened the range of eligible collateral — extending from government bonds to covered bonds, mortgage-backed securities, and corporate paper — and raised the loan-to-value ratios at which that collateral is accepted. At LTV 70%, a bank holding ₩100 billion in bonds can borrow ₩70 billion from the BOK; at LTV 90%, the same collateral supports ₩90 billion. This is not a change in the base rate. It is a relaxation of the terms under which banks access central bank liquidity, increasing the effective capacity of the system to absorb funding stress without adjusting the policy rate itself.

### Standing Facilities

The second instrument is the standing facility: two permanently-available windows through which banks can borrow from or lend to the BOK at rates fixed relative to the base rate. Institutions can borrow overnight from the lending facility (자금조정대출) at the base rate plus 100 basis points, and deposit excess reserves at the deposit facility (자금조정예금) at the base rate minus 100 basis points. These two rates form a corridor around the policy target.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 560 270" xmlns="http://www.w3.org/2000/svg" style="max-width:560px; width:100%;">
  <rect width="560" height="270" fill="none"/>

  <defs>
    <marker id="arr-cor1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#8b949e" stroke-width="1.5" stroke-linecap="round"/>
    </marker>
  </defs>

  <!-- Rate axis label -->
  <text x="20" y="135" text-anchor="middle" font-size="10" fill="#555" transform="rotate(-90 20 135)">Policy Rate</text>

  <!-- Corridor band (shaded) -->
  <rect x="80" y="60" width="380" height="170" rx="4" fill="#1a2d42" opacity="0.25"/>

  <!-- Ceiling line — Lending Facility -->
  <line x1="80" y1="65" x2="460" y2="65" stroke="#E24B4A" stroke-width="2"/>
  <rect x="460" y="52" width="90" height="26" rx="4" fill="#2a1212"/>
  <text x="505" y="64" text-anchor="middle" font-size="9" fill="#E24B4A">Lending</text>
  <text x="505" y="76" text-anchor="middle" font-size="9" fill="#E24B4A">Base + 1.00%</text>
  <text x="38" y="69" text-anchor="middle" font-size="11" font-weight="700" fill="#E24B4A">Ceiling</text>

  <!-- Base rate line -->
  <line x1="80" y1="150" x2="460" y2="150" stroke="#378ADD" stroke-width="2.5" stroke-dasharray="6,3"/>
  <rect x="460" y="137" width="90" height="26" rx="4" fill="#1a2d42"/>
  <text x="505" y="149" text-anchor="middle" font-size="9" fill="#378ADD">Base Rate</text>
  <text x="505" y="161" text-anchor="middle" font-size="9" fill="#378ADD">(Target)</text>
  <text x="38" y="154" text-anchor="middle" font-size="11" font-weight="700" fill="#378ADD">Target</text>

  <!-- Floor line — Deposit Facility -->
  <line x1="80" y1="230" x2="460" y2="230" stroke="#1D9E75" stroke-width="2"/>
  <rect x="460" y="217" width="90" height="26" rx="4" fill="#0d2a1e"/>
  <text x="505" y="229" text-anchor="middle" font-size="9" fill="#1D9E75">Deposit</text>
  <text x="505" y="241" text-anchor="middle" font-size="9" fill="#1D9E75">Base − 1.00%</text>
  <text x="38" y="234" text-anchor="middle" font-size="11" font-weight="700" fill="#1D9E75">Floor</text>

  <!-- Market rate band (floating zone) -->
  <rect x="110" y="125" width="320" height="50" rx="3" fill="#378ADD" opacity="0.12"/>
  <text x="270" y="148" text-anchor="middle" font-size="10" fill="#8b949e">Overnight interbank rate</text>
  <text x="270" y="162" text-anchor="middle" font-size="10" fill="#555">floats within corridor</text>

  <!-- Brace / bracket showing corridor width -->
  <line x1="72" y1="65" x2="72" y2="230" stroke="#333" stroke-width="1"/>
  <line x1="72" y1="65" x2="78" y2="65" stroke="#333" stroke-width="1"/>
  <line x1="72" y1="230" x2="78" y2="230" stroke="#333" stroke-width="1"/>
  <text x="62" y="152" text-anchor="middle" font-size="9" fill="#444" transform="rotate(-90 62 152)">±100 bp</text>

  <!-- Self-correction arrows (small) -->
  <text x="160" y="110" font-size="9" fill="#E24B4A">↑ rate exceeds ceiling</text>
  <text x="155" y="120" font-size="9" fill="#E24B4A">→ banks borrow from BOK → rate falls</text>
  <text x="155" y="196" font-size="9" fill="#1D9E75">↓ rate falls below floor</text>
  <text x="155" y="206" font-size="9" fill="#1D9E75">→ banks deposit at BOK → rate rises</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 3. The interest rate corridor formed by the BOK's standing facilities. The lending facility (base rate +100 bp) sets a ceiling; the deposit facility (base rate −100 bp) sets a floor. The overnight interbank rate is self-corrected toward the target whenever it approaches either bound. The BOK's corridor at ±100 bp is wider than the ECB's (±25 bp), giving the interbank market more room for price discovery at the cost of greater short-rate volatility.
</figcaption>
</figure>

The corridor enforces its ceiling and floor through a self-correcting mechanism. If the overnight rate were to rise above the lending facility rate, banks would substitute toward BOK borrowing rather than interbank funding — excess demand would shift to the standing facility, relieving pressure in the interbank market and pulling rates back down. If the rate fell below the deposit facility rate, banks would park excess reserves at the BOK rather than lend them out — funds would drain from the interbank market until rates recovered. The target holds inside the corridor through arbitrage, without the BOK needing to transact actively on each day.

Banks rarely use the standing facilities in practice. Accessing the lending window signals to counterparties that the institution needed emergency central bank funding, which can trigger exactly the adverse reputation effects that prompted the borrowing. This stigma effect means the facilities function primarily as a credible backstop: their existence shapes rate expectations even on days when no transactions occur.

### Reserve Requirements

The reserve requirement system mandates that deposit-taking institutions hold a minimum fraction of their deposits as reserves at the BOK. Required ratios vary by deposit type: 7% on demand deposits, 2% on time and savings deposits, and 1–7% on foreign currency deposits.

The textbook mechanism is the money multiplier. If banks must hold fraction $r$ of deposits in reserve, a unit of base money supports $1/r$ units of total deposits across the banking system. A 10% reserve ratio implies a multiplier of 10; raising the ratio to 20% compresses it to 5. Changes in the legal requirement should, in theory, directly adjust the credit-creation capacity of the banking system.

In practice, the instrument has lost most of its traction. The core problem is excess reserves. After 2008, banks in most major economies began holding reserve balances far in excess of the legal minimum, driven by uncertainty about funding conditions and, in some jurisdictions, by the introduction of interest on reserve balances. A change in the legal reserve requirement has no effect on institutions that are already holding far more than required. The requirement acts as a binding constraint only for banks at or near the legal minimum — and for most large institutions most of the time, that constraint is not binding. The BOK maintains the reserve requirement system, but it is not the channel through which monetary policy decisions transmit to the economy. It functions primarily as a structural stabilizer for the payment system.

---

## 3. The Word Is Also a Tool

The base rate decision, RP operations, and standing facilities all operate on the price and availability of money today. There is a fourth instrument that operates on a different variable: expectations about the price and availability of money in the future.

Forward guidance — announcing the expected future path of policy rates — became a recognized instrument in the post-2008 era, when the conventional policy rate had reached its lower bound. But the underlying mechanism predates the term. A credible commitment to hold rates at a particular level for a stated period shifts long-term rates immediately, without any change in the current overnight rate. The yield on a five-year government bond reflects, approximately, the expected average of overnight rates over that horizon plus a term premium. If the central bank can credibly shift the expected path of future short rates, it shifts medium- and long-term yields directly — without any transaction in the RP or bond markets.

Forward guidance comes in two forms. *Calendar-based* guidance specifies a time horizon: "the base rate will remain at its current level for at least eighteen months." *State-contingent* guidance specifies a trigger condition: "the base rate will remain at its current level until unemployment falls below 6 percent." Calendar-based guidance is simple to communicate but commits the institution to a fixed timeline regardless of how the economy develops. State-contingent guidance is more flexible — it communicates explicitly what would cause a policy reversal — but it requires the public to understand the stated threshold and trust the central bank's commitment to honor it when the threshold approaches.

Several researchers have raised concerns about whether forward guidance works as cleanly as the theory suggests. Mishkin (2004) and Goodhart (2001) argued that the public systematically misreads the conditionality embedded in central bank communications. A central bank that publishes a rate path is communicating a probabilistic forecast conditional on the current economic outlook. Much of the public, however, reads such statements as unconditional commitments. When conditions change and the central bank adjusts course — perfectly defensibly — public reaction can treat the revision as a failure of reliability rather than as appropriate policy flexibility.

Issing (2005) raised a distinct concern about institutional credibility. If a central bank publishes explicit forward guidance and deviates from it repeatedly — even for good economic reasons — markets eventually stop treating the guidance as informative. Once that credibility is exhausted, the instrument loses its effect, and the institution faces a situation arguably worse than silence: it has conditioned market participants to look for explicit rate path communication, but its own guidance no longer carries information value.

One thing that confused me about these critiques was whether they argue against forward guidance as a category or against specific implementations of it. The answer is closer to the latter. The concern is not that signaling future intentions is inherently harmful. It is that the instrument is fragile: overly specific guidance, poorly timed guidance, or guidance issued by an institution with a weak track record of explanation can undermine exactly the credibility the guidance was meant to reinforce. The preconditions for using it well are demanding.

---

## 4. When the Floor Disappears

Everything in the preceding sections operates under a tacit assumption: there is room to cut the base rate. The logic of RP operations, the corridor, and forward guidance all function in an environment where the policy rate can be moved downward in response to deteriorating conditions.

The 2007–2008 global financial crisis removed that assumption for most major central banks. The Federal Reserve reduced its target rate to effectively zero by December 2008. The ECB reached zero in 2014. The Bank of Japan had been near zero since the late 1990s. With conventional rate cuts exhausted, central banks needed different instruments.

<figure style="text-align:center; margin: 2rem 0;">
<svg viewBox="0 0 700 230" xmlns="http://www.w3.org/2000/svg" style="max-width:700px; width:100%;">
  <rect width="700" height="230" fill="none"/>

  <!-- Container -->
  <rect x="15" y="15" width="670" height="205" rx="8" fill="#111" stroke="#2a2a2a" stroke-width="1"/>

  <!-- Header row -->
  <rect x="15" y="15" width="670" height="32" rx="8" fill="#161b22" stroke="#2a2a2a"/>
  <text x="100" y="36" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Instrument</text>
  <text x="280" y="36" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">How it eases conditions</text>
  <text x="490" y="36" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">BOK deployed?</text>
  <text x="630" y="36" text-anchor="middle" font-size="10" font-weight="600" fill="#8b949e">Example</text>

  <!-- Vertical dividers -->
  <line x1="175" y1="15" x2="175" y2="220" stroke="#222" stroke-width="0.8"/>
  <line x1="385" y1="15" x2="385" y2="220" stroke="#222" stroke-width="0.8"/>
  <line x1="560" y1="15" x2="560" y2="220" stroke="#222" stroke-width="0.8"/>

  <!-- Row 1: Liquidity Support -->
  <line x1="15" y1="83" x2="685" y2="83" stroke="#1a1a1a" stroke-width="0.6"/>
  <text x="100" y="58" text-anchor="middle" font-size="9.5" fill="#8b949e">Expanded</text>
  <text x="100" y="71" text-anchor="middle" font-size="9.5" fill="#8b949e">Liquidity Support</text>
  <text x="280" y="58" text-anchor="middle" font-size="9" fill="#555">Wider collateral eligibility,</text>
  <text x="280" y="71" text-anchor="middle" font-size="9" fill="#555">higher LTV, longer maturities</text>
  <text x="490" y="64" text-anchor="middle" font-size="10" font-weight="700" fill="#1D9E75">Yes — 2008, 2020</text>
  <text x="630" y="58" text-anchor="middle" font-size="9" fill="#555">BOK repo</text>
  <text x="630" y="71" text-anchor="middle" font-size="9" fill="#555">expansion</text>

  <!-- Row 2: QE -->
  <line x1="15" y1="120" x2="685" y2="120" stroke="#1a1a1a" stroke-width="0.6"/>
  <text x="100" y="105" text-anchor="middle" font-size="9.5" fill="#8b949e">Quantitative Easing</text>
  <text x="280" y="98" text-anchor="middle" font-size="9" fill="#555">Large-scale asset purchases compress</text>
  <text x="280" y="111" text-anchor="middle" font-size="9" fill="#555">long-term yields via term premium</text>
  <text x="490" y="105" text-anchor="middle" font-size="10" font-weight="700" fill="#E24B4A">No</text>
  <text x="630" y="98" text-anchor="middle" font-size="9" fill="#555">Fed QE1-3,</text>
  <text x="630" y="111" text-anchor="middle" font-size="9" fill="#555">ECB APP</text>

  <!-- Row 3: Forward Guidance as commitment -->
  <line x1="15" y1="155" x2="685" y2="155" stroke="#1a1a1a" stroke-width="0.6"/>
  <text x="100" y="135" text-anchor="middle" font-size="9.5" fill="#8b949e">FG as Commitment</text>
  <text x="100" y="148" text-anchor="middle" font-size="9.5" fill="#555">(state-contingent)</text>
  <text x="280" y="135" text-anchor="middle" font-size="9" fill="#555">Explicit pledge shifts expected</text>
  <text x="280" y="148" text-anchor="middle" font-size="9" fill="#555">rate path, reducing long yields</text>
  <text x="490" y="141" text-anchor="middle" font-size="10" font-weight="700" fill="#EF9F27">Partial</text>
  <text x="630" y="135" text-anchor="middle" font-size="9" fill="#555">Fed "at least</text>
  <text x="630" y="148" text-anchor="middle" font-size="9" fill="#555">through 2014"</text>

  <!-- Row 4: Negative rates -->
  <line x1="15" y1="190" x2="685" y2="190" stroke="#1a1a1a" stroke-width="0.6"/>
  <text x="100" y="170" text-anchor="middle" font-size="9.5" fill="#8b949e">Negative</text>
  <text x="100" y="183" text-anchor="middle" font-size="9.5" fill="#8b949e">Policy Rate</text>
  <text x="280" y="170" text-anchor="middle" font-size="9" fill="#555">Charges banks for excess reserves,</text>
  <text x="280" y="183" text-anchor="middle" font-size="9" fill="#555">incentivizing lending over parking</text>
  <text x="490" y="177" text-anchor="middle" font-size="10" font-weight="700" fill="#E24B4A">No</text>
  <text x="630" y="170" text-anchor="middle" font-size="9" fill="#555">ECB −0.5%,</text>
  <text x="630" y="183" text-anchor="middle" font-size="9" fill="#555">BoJ −0.1%</text>

  <!-- Row 5: YCC -->
  <text x="100" y="206" text-anchor="middle" font-size="9.5" fill="#8b949e">Yield Curve Control</text>
  <text x="280" y="206" text-anchor="middle" font-size="9" fill="#555">Targets a specific maturity yield directly; unlimited purchases to defend</text>
  <text x="490" y="206" text-anchor="middle" font-size="10" font-weight="700" fill="#E24B4A">No</text>
  <text x="630" y="206" text-anchor="middle" font-size="9" fill="#555">BoJ 10yr = 0%</text>
</svg>
<figcaption style="font-size:0.85rem; color:#888; margin-top:0.5rem;">
  Figure 4. Non-conventional monetary policy instruments deployed after the 2008 global financial crisis. The BOK used expanded liquidity support facilities in both 2008 and 2020 but did not pursue quantitative easing, negative rates, or yield curve control — instruments extensively used by the Federal Reserve, ECB, and Bank of Japan.
</figcaption>
</figure>

**Liquidity support to financial institutions and credit markets** involves expanding the terms of ordinary central bank lending: wider collateral eligibility, longer maturities, higher LTV ratios. This is an extension of the OMO framework, not a departure from it. The BOK used expanded repo facilities during the 2008 crisis and the 2020 pandemic, accepting a broader range of securities to ease bank funding constraints.

**Quantitative easing** involves large-scale outright purchases of government bonds and other securities, expanding the central bank's balance sheet without a near-term commitment to reverse the purchases. The mechanism differs from RP in both direction and duration. An RP is short-term and self-unwinding; QE purchases are long-term and intended to compress the term premium on longer-dated securities. When a central bank buys 10-year government bonds, it removes duration from the market and injects reserves, pushing down long yields even when the overnight rate cannot fall further. The Federal Reserve, ECB, and Bank of Japan pursued large-scale QE; the BOK did not.

**Forward guidance as a commitment device** takes on a different character at the zero lower bound. A central bank that cannot cut rates further can still ease financial conditions by credibly committing to keep rates at zero for longer than markets currently expect — shifting the expected path of future short rates downward and reducing long-term yields in the present. This version of forward guidance is more aggressive than ordinary rate communication: it is a self-imposed constraint on future policy, not merely a forecast.

**Negative interest rates** push the policy rate below zero. The ECB introduced a negative deposit facility rate in June 2014; the Bank of Japan followed in January 2016. The direct mechanism is to charge banks for holding excess reserves at the central bank, providing an incentive to deploy those funds into loans rather than parking them. Whether negative rates pass through to retail depositors depends on whether banks are willing to impose that cost on household accounts — which most have not, given the political and reputational consequences. Negative rates have primarily been absorbed as a compression on bank margins rather than passed through to the household sector.

**Yield curve control** sets an explicit target for the yield at a specific maturity and commits to unlimited purchases to defend it. The Bank of Japan's 2016 announcement of a zero percent target for 10-year government bond yields is the most extensive implementation. Unlike QE, which specifies a quantity, YCC specifies a price — the central bank stands ready to buy whatever volume of bonds is needed to hold the yield at the stated level.

The BOK's posture through both 2008 and 2020 was conservative relative to the Federal Reserve or ECB. Korea's policy rate trough was 0.5 percent in 2020–2021, well above zero, and the BOK did not implement negative rates, formal QE, or yield curve control. This reflected both institutional preferences and structural constraints. As a small open economy with significant external financing needs, aggressive balance sheet expansion by the central bank risks putting downward pressure on the exchange rate and generating capital flow volatility that is harder to manage than in the US or eurozone. Conservatism and structural vulnerability pointed in the same direction.

Part 2 covers what happens after the rate decision is made: the six channels through which the base rate reaches economic outcomes, the transmission lags that make monetary policy inherently forward-looking, and the structural challenges — a flattening Phillips curve, demographic headwinds, and the constraints of financial stability management — that have made the toolkit progressively harder to use.

---

## References

- Bank of Korea (2017). *Monetary Policy in Korea*. Seoul: Bank of Korea. [bok.or.kr](https://www.bok.or.kr/portal/bbs/P0000602/view.do?nttId=234114&menuNo=200459)
- Mishkin, F. S. (2004). *Can Central Bank Transparency Go Too Far?* NBER Working Paper No. 10829. [nber.org/papers/w10829](https://www.nber.org/papers/w10829)
- Goodhart, C. A. E. (2001). *Monetary Transmission Lags and the Formulation of the Policy Decision on Interest Rates*. Federal Reserve Bank of St. Louis Review, 83(4), 165–186.
- Issing, O. (2005). *Communication, Transparency, Accountability: Monetary Policy in the Twenty-First Century*. Federal Reserve Bank of St. Louis Review, 87(2), 65–83.
- Borio, C., & Zhu, H. (2008). *Capital Regulation, Risk-Taking and Monetary Policy: A Missing Link in the Transmission Mechanism?* BIS Working Papers No. 268. [bis.org/publ/work268.htm](https://www.bis.org/publ/work268.htm)
