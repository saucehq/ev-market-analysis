# ev-market-analysis
US EV demand analysis from Tesla's strategic standpoint — OLS regression with two-way fixed effects, Google Trends alt data, and 14-state registration panel
# EV Market Analysis: Tesla Strategic Intelligence

**BUS 658 — Information & Data Systems | Chapman University | Spring 2026**  
**Author:** Alexxis Saucedo | [LinkedIn](https://linkedin.com/in/alexxissaucedo) | [GitHub](https://github.com/saucehq)

---

## Overview

A data-driven analysis of US electric vehicle demand from Tesla's strategic standpoint, examining how Chinese EV competition, federal tariff policy, and autonomous driving capabilities jointly shape Tesla's market position. Built as a consulting-style deliverable using public registration data, Google Trends alt data, and econometric modeling.

---

## Research Question

> How do Chinese EV competition, federal tariffs, and autonomous driving capabilities jointly shape US EV demand — and what does that suggest for Tesla's competitive position?

---

## Key Findings

| Finding | Result |
|---|---|
| Chinese EV consumer awareness | BYD: 0 search weeks out of 262. Zero Chinese EV passenger registrations across 14 states and 26M records |
| Post-tariff Tesla share change | −8.9 percentage points (coef: −0.089, p=0.040) within-state |
| FSD v12 attention spike | +166% (mean 12.5 → 33.3, index hit 100) — largest signal in dataset |
| Primary domestic competitors | Ford (truck segment) + Hyundai/Kia (sub-$40k sedan segment) |

> **Note:** Results are directional strategic signals, not causal estimates. Google Trends is used as a consumer attention proxy. See identification challenges in the analysis for full methodological limitations.

---

## Data Sources

| Dataset | What It Measures | Source | Access |
|---|---|---|---|
| EV Registrations | BEV adoption by state and make, 2020–2025 | Atlas EV Hub | Free download |
| Gas Prices | Monthly US national average ($/gal) | EIA APIv2 | Free API key |
| Charging Infrastructure | Tesla Supercharger locations + open dates | AFDC / NREL API | Free API key |
| Google Trends — Competition | Brand attention: Tesla, BYD, NIO, Rivian, Lucid | pytrends | No key required |
| Google Trends — Demand | Purchase intent: buy Tesla, Model Y, EV incentive | pytrends | No key required |
| Google Trends — Autonomy | FSD search interest: Tesla FSD, autopilot, self driving | pytrends | No key required |
| Policy Events | IRA (Aug 2022), Tariff (May 2024), FSD releases | Hardcoded | N/A |

**Sample:** 14 states · 298 state-quarter observations · 2020Q1–2025Q4

---

## Methodology

### Panel Structure
- Unit of observation: US state × quarter
- 14 states: CO, CT, ME, MN, MT, NC, NJ, NM, NY, OR, TN, TX, VA, VT
- 24 quarters: 2020Q1 through 2025Q4
- Dependent variable: Tesla BEV registrations / Total BEV registrations

Tesla Share(i,t) = β₁ Gas Price(t) + β₂ Rivian Search(t) +
β₃ Post-IRA(t) + β₄ Post-Tariff(t) +
α(i) + γ(t) + ε(i,t)

Where α(i) = state fixed effects, γ(t) = quarter fixed effects

### Results

| Variable | Coefficient | p-value | Interpretation |
|---|---|---|---|
| Post-Tariff (2024Q2+) | −0.089 | 0.040 ** | Share fell ~9pp post-tariff — competitive erosion continued |
| Gas Price ($/gal) | −0.099 | 0.018 * | Higher gas prices drove EV demand broadly but not toward Tesla |
| Post-IRA (2022Q3+) | −0.059 | 0.009 ** | Share fell ~6pp — subsidies benefited broader market |
| Rivian Search Interest | +0.241 | 0.152 | Not significant |

**Model fit:** R² = 0.863 · Adj. R² = 0.844 · N = 298

> The high R² reflects two-way fixed effects absorbing stable cross-state differences. Without fixed effects R² = 0.197. The model identifies within-state variation over time, not cross-state comparisons.

### Alt Data Analysis
- **Tariff event study:** 12-month window around May 2024 announcement — Tesla search +6.3% post-tariff (64.6 → 68.7), BYD flat at zero throughout
- **FSD demand signal:** FSD v12 (March 2024) generated +166% attention spike, hitting index maximum of 100 — largest signal in the entire dataset

---

## Identification Challenges

This analysis uses observational data and search proxies. Results should be interpreted as directional signals, not causal estimates.

- **Proxy validity** — Google Trends measures attention, not purchases
- **Confounded timing** — tariff coincided with Rivian R2 launch and macro EV slowdown
- **Reverse causality** — higher Tesla share could drive more Tesla search interest
- **Omitted variables** — interest rates, financing costs, Musk sentiment not controlled
- **External validity** — California (~35-40% of US EV sales) excluded due to annual vs. quarterly data mismatch
- **Parallel trends** — not formally tested

---

## Strategic Implications

1. **Address Ford + Hyundai/Kia — not Chinese EVs.** Registration data shows 9 out of 10 new EV buyers in this sample went to non-Tesla brands. Ford wins the truck segment; Hyundai/Kia win the sub-$40k sedan segment. These are the actual competitive gaps.

2. **Treat FSD releases as demand events.** The +166% attention spike at FSD v12 is the largest signal in this dataset — larger than the IRA, larger than the tariff. Autonomy events are Tesla's strongest observed demand lever.

3. **Build a cost structure that survives tariff reduction.** BYD Seagull starts at ~$10k in China. At 100% tariff → $20k landed. At 25% → $12.5k. Tesla has no product at those price points. The $25k vehicle platform is a tariff hedge, not just a market expansion play.

---

## Repository Structure
ev-market-analysis/
├── notebooks/
│   ├── 01_data_collection.ipynb    # API pulls, Google Trends, event dates
│   ├── 02_eda.ipynb                # Registration cleaning, EDA, visualizations
│   └── 03_demand_model.ipynb       # OLS regression, fixed effects, robustness checks
├── data/
│   └── cleaned/
│       ├── ev_registrations_clean.csv   # 14-state panel, state × quarter
│       └── model_dataset.csv            # Final merged dataset for regression
├── outputs/
│   └── figures/                    # All visualizations
└── README.md
> **Note:** `data/raw/` is excluded from this repo (file size + API keys). To reproduce, run `01_data_collection.ipynb` with your own free API keys from eia.gov/opendata and developer.nrel.gov.

---

## Tech Stack
Python 3.13
pandas · numpy · statsmodels · pytrends
matplotlib · seaborn · requests
Jupyter · VS Code
---

## How to Reproduce

1. Clone the repo
2. Install dependencies: `pip install pandas numpy statsmodels pytrends matplotlib seaborn requests`
3. Get free API keys:
   - EIA: [eia.gov/opendata](https://www.eia.gov/opendata/)
   - NREL: [developer.nrel.gov](https://developer.nrel.gov/)
4. Add your keys to `01_data_collection.ipynb` where indicated
5. Download 14-state EV registration CSVs from [Atlas EV Hub](https://www.atlasevhub.com/materials/state-ev-registration-data/) and place in `data/raw/`
6. Run notebooks in order: 01 → 02 → 03

---

## Context

This project was built as a final deliverable for BUS 658 — Information & Data Systems at Chapman University (Spring 2026). It is also intended as a portfolio piece targeting analytics roles in automotive, energy, and technology sectors.

The analysis attends the 2025 Entertainment Analytics Conference in Los Angeles where similar alt data methodologies were discussed by analysts from Disney, Netflix, and Sony.
### Regression Model
Two-way fixed effects OLS:
