# sa-electricity-market-investment-thesis
# SA Capture Price Analysis

> Quantitative analysis of capture prices, revenue dynamics, and risk-adjusted returns across 15 South Australian generators (Jul 2022 – Jun 2024), investment thesis for a renewable energy developer acquisition in the Australian National Electricity Market.

![Period](https://img.shields.io/badge/Period-Jul%202022%20–%20Jun%202024-5C2D91)
![Region](https://img.shields.io/badge/Region-SA1%20(NEM)-5C2D91)
![Stack](https://img.shields.io/badge/Built%20with-Python%20%7C%20pandas%20%7C%20matplotlib-blue)
![Status](https://img.shields.io/badge/Status-ForecastingAppending-green)

---

### Overview

South Australia operates the most renewable-penetrated grid in the National Electricity Market, with renewable share peaking above 80% on individual months and the state targeting 100% net renewable by 2027. This analysis go through across five technologies: **diesel, natural gas, wind, solar, and battery storage** with a specific focus on the dispatch-weighted prices each technology actually captures.

---

### The core problem 

South Australia runs on renewable energy more than almost anywhere else in the world, especially wind and solar covering over 100% of the state's electricity demand. But it's turned out that generating green electricity and actually making money from it are two very different things:

- When every solar farm switches on at noon and every wind turbine spins up overnight, prices don't rise to meet them, they collapse.
- The market is witnessing electricity oversupply, and the very renewable resource abundance that makes South Australia **a renewable success story** is the same force quietly squeezing generator revenues.

Therefore, the report is built around a single underlying question that matter:

> **Which generation technology produces returns, not just electricity on its own ?**

And go along with me through the project, we're trying to figure out the answers for the question why renewable investore are quietly losing sleep. 

---

### What I did
- Built a dispatch-weighted price (capture price) model and capture factor metric to compare actual generator revenue against time-weighted market averages
- Modeled battery storage revenue using a rule-based arbitrage strategy (charge on the cheapest 12 intervals/day, discharge on the most expensive, 0.85 round-trip efficiency)
- Applied rolling 90-day windows to isolate structural trends from short-term price noise
- Produced two report formats: a 12-page investment-client report and a 1-page public infographic <-> from the same analytical base

---

### Key Findings

| Area | Key metric | Insight | Takeaway |
|---|---|---|---|
| **Capture price spread** | $25/MWh (solar) → $430/MWh (diesel) — **17× range** | Technology selection is the single dominant revenue driver — bigger than any operational lever | Pick the wrong technology and no amount of execution fixes it |
| **Hourly price mechanics** | Midday ~$0/MWh vs 6pm ~$240/MWh — **240:1 daily swing** | Solar dispatches at peak supply and minimum price, every day, by physics — not bad luck | No operational fix exists within solar itself; only co-located storage or firm contracts resolve it |
| **Solar structural decline** | Capture factor trending at **−0.026/year** | A project capturing 60% of market average today captures only ~20% by 2040 if the trend holds | Standalone merchant solar is not investable in SA on a 15-year horizon |
| **Wind rising resilience** | Capture factor trending at **+0.106/year**, exceeding market average in late 2023 | Wind generates into evening and overnight windows when prices are highest — an advantage solar cannot replicate | Wind is the scalable renewable with momentum on its side |
| **Battery risk-adjusted return** | ~$18,000/MW/month at CV **~0.55–0.60** vs gas CV ~1.0–1.2 | Competitive returns at the lowest revenue volatility of any technology in the dataset | Storage is the efficient frontier — best return per unit of risk |
| **Portfolio optimum** | 60/40 wind–storage at ~**$17,000/MW/month**, CV ~0.55 | Pure storage sits on the efficient frontier but is too small-scale for institutional capital | The investable version of the optimal portfolio for meaningful capital allocation |

**Investment recommendation:** A 60/40 wind-storage hybrid portfolio sits on the efficient frontier with the deployable scale that institutional capital requires.

---
## Recommendation Summary & Expected Impact

> Forward validation draws on AEMO QED Q2 2026, Modo Energy, Aurora Energy
> Research, and SA FERM Tender 1 results (May 2026).

| Recommendation | What the market did | Revised position |
|---|---|---|
| **Battery storage deploy aggressively within 24–36 months** | Arbitrage spreads collapsed 85% YoY ($342 → $51/MWh). Net NEM battery revenue fell 56%. Grid-scale fleet passed 9,000MW. SA FERM Tender 1 awarded 517MW / 4,136MWh of contracted capacity | **Contracted exposure only**.|
|  **Wind is build for deployable scale** | Wind supplies 44.5% of SA electricity (2024–25). AEMO 2026 ISP targets 26GW nationally by 2030. Capital costs stabilising (CSIRO: −5% projected 2025–26).| **Maintain and accelerate** |
|  **Avoid standalone merchant solar** | Curtailment reached 25% for some assets. Rooftop PV overtook gas as SA's second-largest source (22.4%).| **Avoid case strengthened.**|
| **Gas is tactical, exit-defined position** |  Gas share declined to 20.8%. Gas prices fell 9–17% YoY. Eraring Power Station extended to **April 2029** (from 2027); AEMO cited insufficient replacement capacity and slow renewables build-out | **Model April 2029 as the revised exit window.** |

---

### Reports

Two report versions are available, calibrated to different audiences:

| Report | Audience | Length | Focus |
|---|---|---|---|
| [`reports/investment_report.pdf`](SA_Investment_report.pdf) | Investment client | 12 pages | Recommendation-led, decision-oriented |
| [`infographic/investment_infographic.pdf`](sa-electricity-market-investment-thesis/SA_electricity_investment_infographic.html.pdf) | Public | 1 pages | Visual presentation, Quick underestanding  |

Both reports draw from the same analytical foundation but differ in framing, depth of recommendation, and visual conventions.

---

## Methodology

### Capture Price (Dispatch-Weighted Price)

For each generator *s* over period *T*, the capture price is computed as:

$$
DWP_T^s = \frac{\sum_{t=1}^{T} P_t \times Q_t^s}{\sum_{t=1}^{T} Q_t^s}
$$

where *P_t* is the SA1 spot price at 5-minute interval *t*, and *Q_t^s* is the dispatch of station *s* at interval *t* in MWh.

### Capture Factor

The ratio of a generator's DWP to the time-weighted average spot price over the same window:

$$
CF_T^s = \frac{DWP_T^s}{TWP_T}
$$

A capture factor above 1.0 indicates the generator earns above market average; below 1.0 indicates structural undervaluation. Rolling 90-day windows are used to smooth short-duration noise.

### Battery Treatment

Battery revenue is calculated as **net revenue** (discharge revenue minus charging cost) using a rule-based arbitrage model:

- Charge during the bottom 12 daily 5-minute intervals (lowest prices)
- Discharge during the top 12 intervals (highest prices)
- Round-trip efficiency = 0.85

Battery capacity factor and DWP are presented separately from energy-driven generators where methodologically appropriate, reflecting the structural difference between arbitrage-driven and energy-production-driven revenue.


---

## Data Sources

| Source | Use |
|---|---|
| AEMO 5-minute dispatch and price data | Primary dataset (SA1 region) |
| AEMO Generation Information | Nameplate capacities |
| AEMO Quarterly Energy Dynamics (Q2 2023, Q3 2025) | Industry context, validation |
| AEMO 2025 Electricity Statement of Opportunities | Forward demand and pipeline context |
| DCCEEW NGER | Emissions intensity factors |

All data is used in accordance with the source publishers' terms. AEMO data is © Australian Energy Market Operator Limited.

---

## Limitations

This analysis has several limitations that should inform any forward use of the findings:

- **Window length.** Two years is short for forward inference, and the period contains an outlier event (Q3 2022 gas price shock) that materially distorts averages. Sensitivity tests are recommended for any forward modelling.
- **Excluded data.** The dataset does not include forward project pipeline, contracted revenue mechanisms (PPAs, CIS contracts), curtailment causation data, or Marginal Loss Factors.
- **Battery model.** The rule-based arbitrage model is a first-order approximation. Actual battery operations involve FCAS revenue stacking, system strength services, and contract structures not captured here.
- **Single-region scope.** SA1 dynamics are not directly transferable to other NEM regions; SA is the most VRE-penetrated region and may foreshadow rather than represent broader NEM conditions.

---

## About

The analysis is intended to demonstrate quantitative analytical capability in the energy finance domain. It is not investment advice.

### Author

**Quynh Huong Nguyen (Sylvie)**

Macquarie Business School

[LinkedIn](https://www.linkedin.com/in/sylvia-quin/) · 📧 [Email](huongquynh04.vn@gmail.com)


### Acknowledgements

- AEMO for public dispatch and price data
- DCCEEW for emissions factor publications
- The AEMO Quarterly Energy Dynamics team for the visual and structural conventions referenced in this work

---
