# sa-electricity-market-investment-thesis
# SA Capture Price Analysis

> Quantitative analysis of capture prices, revenue dynamics, and risk-adjusted returns across 15 South Australian generators (Jul 2022 – Jun 2024) — investment thesis for a renewable energy developer acquisition in the Australian National Electricity Market.

![Period](https://img.shields.io/badge/Period-Jul%202022%20–%20Jun%202024-5C2D91)
![Region](https://img.shields.io/badge/Region-SA1%20(NEM)-5C2D91)
![Stack](https://img.shields.io/badge/Built%20with-Python%20%7C%20pandas%20%7C%20matplotlib-blue)
![Status](https://img.shields.io/badge/Status-ForecastingAppending-green)

---

## Overview

South Australia operates the most renewable-penetrated grid in the National Electricity Market, with renewable share peaking above 80% on individual months and the state targeting 100% net renewable by 2027. This analysis examines how that transition has reshaped the economics of generation across five technologies: **diesel, natural gas, wind, solar, and battery storage** with a specific focus on the dispatch-weighted prices each technology actually captures.

The analysis is built around a single underlying question:

> *Where should new capital be deployed in the SA generation market, and which technologies offer the strongest risk-adjusted returns over a 10–15 year investment horizon?*

The dataset comprises 5-minute resolution spot prices and dispatch volumes for **15 generators** (3 each across the five technologies) over a **two-year window**, totalling approximately 3.1 million observations.

---

## Key Findings

**1. Capture prices span 17× across SA technologies.** Median dispatch-weighted prices range from ~$25/MWh (solar) to ~$430/MWh (diesel), making technology selection the dominant revenue driver in the SA market.

**2. The dispatchable premium is widening, not narrowing.** Gas and battery capture factors have roughly doubled over the two-year period. This contradicts the assumption that flexible thermal economics decline with renewable penetration.

**3. Solar capture factor is on a measurable downward trend** (–0.026/year), confirming structural cannibalisation. Compounded over a 15-year project life, this implies roughly 40 percentage points of additional capture price erosion.

**4. Battery storage offers the best risk-adjusted return profile**, with mean revenue per MW competitive with gas peakers but at materially lower volatility (CV ~0.55–0.60 vs ~1.0–1.2 for gas).

**5. The early-mover window for storage is finite.** Battery capture factors begin compressing at the right edge of the dataset, suggesting saturation effects as more storage capacity enters the SA system.

**Investment recommendation:** A 60/40 wind-storage hybrid portfolio sits on the efficient frontier with the deployable scale that institutional capital requires.

---

## Reports

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

This project was prepared as part of the **BUSA8031 Business Analytics Project** at Macquarie Business School, supervised by Professor Stefan Trueck. It applies the analytical conventions of energy finance consulting and the visual conventions of the AEMO Quarterly Energy Dynamics report series.

The analysis is intended to demonstrate quantitative analytical capability in the energy finance domain. It is not investment advice.

### Author

**Quynh Huong Nguyen (Sylvie)**
Macquarie Business School
[LinkedIn](https://www.linkedin.com/in/sylvia-quin/) · [Email](huongquynh04.vn@gmail.com)

### Acknowledgements

- AEMO for public dispatch and price data
- DCCEEW for emissions factor publications
- The AEMO Quarterly Energy Dynamics team for the visual and structural conventions referenced in this work

---
