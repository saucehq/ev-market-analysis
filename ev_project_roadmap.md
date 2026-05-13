# EV Market Analysis: Tesla Strategic Intelligence Project
### BUS 696 Final Project | Alexxis Saucedo | Due: 1 Week

---

## Project Goal

Build a data-driven analysis of US EV demand from Tesla's strategic standpoint, examining how Chinese EV competition, federal tariff policy, and autonomous driving capabilities jointly shape Tesla's market position. The output is a consulting-style deliverable — not just an academic exercise.

**Core research question:**
*How do Chinese EV competition, federal tariffs, and autonomous driving capabilities jointly shape US EV demand, and what does that mean for Tesla's competitive position?*

**The story you're telling:**
Chinese EV brands are building consumer awareness in the US even without direct market access. The 2024 tariffs provide temporary protection. FSD is Tesla's most defensible long-term moat. Together, these three forces define Tesla's strategic window.

---

## Deliverables

- [ ] Jupyter Notebook — clean, commented code showing full analysis pipeline
- [ ] Written Report — research question, methodology, findings, strategic implications
- [ ] Visualizations — charts embedded in notebook and report
- [ ] Slide Deck — non-technical summary of findings and recommendations

---

## Folder Structure

```
ev_project/
├── data/
│   ├── raw/          ← everything pulled from APIs
│   └── cleaned/      ← processed, merged datasets
├── notebooks/
│   ├── 01_data_collection.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_demand_model.ipynb
│   ├── 04_tariff_analysis.ipynb
│   └── 05_autonomy.ipynb
└── outputs/
    └── figures/      ← saved charts
```

---

## Data Sources

| Dataset | What It Measures | Source | Access |
|---|---|---|---|
| EV registrations | Adoption by state and make | Atlas EV Hub | Free download |
| Gas prices | Monthly by state | EIA API | Free key |
| Income / demographics | Median HH income by state | Census ACS | `census` Python library |
| Charging infrastructure | Supercharger count + open dates | AFDC API | Free key at developer.nrel.gov |
| Brand search interest | Consumer awareness of Tesla vs. Chinese EVs | Google Trends via `pytrends` | No key needed |
| Purchase intent signals | "Buy Tesla," "EV tax credit," etc. | Google Trends via `pytrends` | No key needed |
| FSD demand signals | Autonomy-related search interest | Google Trends via `pytrends` | No key needed |
| Tariff event dates | Policy intervention timestamps | Hardcoded | N/A |

**Key event dates to hardcode:**
- `2021-01-01` — COVID recovery inflection
- `2022-08-16` — IRA signed into law
- `2024-05-14` — Biden 100% tariff on Chinese EVs announced
- `2024-03-01` — FSD v12 release
- `2024-10-01` — FSD unsupervised launch

---

## Week-by-Week Plan

### Day 1–2: Data Collection
**Goal:** All raw data pulled and saved to `data/raw/`

- [ ] Set up folder structure in VS Code
- [ ] Install libraries: `pip install pytrends pandas matplotlib seaborn statsmodels requests census`
- [ ] Run Google Trends pull — competition, demand intent, FSD signals
- [ ] Register for AFDC API key at developer.nrel.gov
- [ ] Register for EIA API key at eia.gov/opendata
- [ ] Pull EIA gas prices
- [ ] Pull AFDC Supercharger data
- [ ] Confirm Atlas EV Hub registration data availability
- [ ] Hardcode event dates and save to CSV

**Output:** 5–6 raw CSVs saved and readable

---

### Day 3: Cleaning + EDA
**Goal:** One clean panel dataset, first visualizations

- [ ] Align all datasets to quarterly time series (2020–2024)
- [ ] Merge on state + quarter
- [ ] Check for missing values and outliers
- [ ] Plot competition trends over time with event lines
- [ ] Plot purchase intent signals over time
- [ ] Plot FSD search interest with version release markers

**Output:** `data/cleaned/ev_panel.csv` + 3 EDA charts saved to `outputs/figures/`

---

### Day 4: Demand Model + Tariff Analysis
**Goal:** Core analytical sections complete

**Demand model:**
- [ ] Run baseline OLS regression: EV adoption ~ gas prices + income + charging density + Chinese EV search index
- [ ] Interpret coefficients — what moves the needle on EV adoption?
- [ ] Add competition index and check if it adds explanatory power

**Tariff analysis:**
- [ ] Define treatment: post = quarters after May 2024 tariff announcement
- [ ] Run event study — plot Tesla vs. Chinese EV search interest before and after
- [ ] Test whether tariff shifted relative consumer interest toward Tesla

**Output:** Regression table + event study chart

---

### Day 5: Autonomy Analysis + Strategic Synthesis
**Goal:** Third analytical pillar complete, full narrative drafted

**Autonomy premium:**
- [ ] Mark FSD release dates on timeline
- [ ] Test whether FSD events spike Tesla-specific search interest relative to broader EV market
- [ ] Run simple before/after comparison around each major FSD release

**Strategic synthesis:**
- [ ] Write the "so what" — what do three pillars tell Tesla collectively?
- [ ] Draft strategic implications section: pricing vulnerability, tariff dependency, FSD as moat
- [ ] Outline slide deck structure

**Output:** Autonomy chart + strategic implications draft

---

### Day 6: Polish + Finalize
**Goal:** Submission-ready deliverables

- [ ] Clean and comment all notebook cells
- [ ] Finalize all visualizations (consistent style, labeled axes, titles)
- [ ] Write full report — intro, methodology, findings, implications
- [ ] Build slide deck — 8–12 slides, non-technical audience
- [ ] Proofread everything

---

### Day 7: Buffer
Hold for anything that ran over or needs a second pass.

---

## Three Findings to Walk Away With

1. **Chinese EV awareness is rising** in the US despite zero direct market access — search interest in BYD and NIO has grown, signaling latent demand for lower-cost EVs that the tariff is suppressing, not eliminating.

2. **The 2024 tariff created a measurable shift** in relative consumer interest — it is a real but temporary shield for Tesla's price position.

3. **FSD release events spike Tesla-specific demand signals** — autonomy is a genuine differentiator in consumer perception and Tesla's most defensible long-term moat.

**Strategic conclusion:** Tesla's price competitiveness is vulnerable if tariffs ease or Chinese brands find market entry. FSD is the moat that matters. The window to build it is now.

---

## Libraries You Need

```bash
pip install pytrends pandas numpy matplotlib seaborn statsmodels requests census
```

---

## Key Risks to Manage

**Data availability (risk: Day 1–2)**
Atlas EV Hub granularity may be limited for free tier. Confirm what's available before building your pipeline around it. If registration data by make is unavailable, Google Trends serves as your primary demand proxy.

**Scope creep (risk: Day 4–5)**
Three analytical sections is ambitious solo. If something isn't working cleanly by Day 4, cut it rather than half-finish it. One strong section beats three weak ones.

**pytrends rate limiting (risk: Day 1)**
Google rate limits the API. If you hit a 429 error, add this to your TrendReq call:
```python
pytrends = TrendReq(hl='en-US', tz=360, timeout=(10,25), retries=2, backoff_factor=0.1)
```
