# Rapido Data Science Assessment — Captain Acquisition & Supply

## Overview

This repository contains my analysis for the **Rapido Data Science Assessment** focused on **Captain Acquisition and Airport Supply**.

The analysis covers:

- Captain onboarding funnel and Signup → Approval (A2O) conversion
- Identification and diagnosis of major onboarding leaks
- Evaluation of `CAMP_WA_002`
- Airport demand-supply mismatch
- Post-airport trip utilization and captain behavior proxies
- Recommendations for improving supply through funnel recovery, airport utilization, and targeted acquisition

---

## Executive Recommendation

The analysis supports a **RECOVER → FIX → ACQUIRE** approach:

1. **Recover** captains already in the onboarding funnel, with priority on RC, Fitness, and Insurance stages.
2. **Fix** late-night airport supply economics and post-airport utilization, especially during `21:00–03:00`.
3. **Acquire selectively** only for the residual airport supply gap after utilization improvements.

---

## Key Findings

### Captain Acquisition

- Matured signup cohort: **22,561**
- Approved captains: **3,969**
- Signup → Approval (A2O): **17.59%**
- RC drop-off: **5,433**
- Fitness drop-off: **4,481**
- Insurance drop-off: **3,309**

The largest sequential funnel leaks are at **RC, Fitness, and Insurance**.

Fitness has a particularly strong concentration of drop-off among **e-rickshaw captains**. Most e-rickshaw Fitness drop-offs have no recorded verification failure, so the underlying cause should be validated before attributing the issue directly to document verification.

### WhatsApp Campaign — `CAMP_WA_002`

- Sends: **8,673**
- Delivery rate: **92.91%**
- Click rate: **41.15%**
- Sends at RC stage: **89.17%**
- RC-stage A2O — WhatsApp exposed: **29.18%**
- RC-stage A2O — Not exposed: **25.03%**
- Observed difference: **+4.16 percentage points**

The campaign shows strong engagement and a positive association with activation.

However, the **+4.16 percentage-point difference is observational, not a causal estimate**. The recommended next step is a randomized holdout experiment among eligible RC-stage captains.

### Airport Supply

Airport terminals show a substantially higher demand-supply mismatch than non-airport zones.

- Airport requests: **136,814**
- Unfulfilled airport requests: **55,058**
- Airport unfulfilled rate: **40.24%**
- Non-airport unfulfilled rate: approximately **3%**
- Critical shortage window: **21:00–03:00**

The shortage is highly concentrated rather than being a general marketplace-wide supply problem.

### Post-Airport Utilization

- Airport trips: **60,000**
- No return fare within 20 minutes: **64.04%**
- No return fare during 21:00–03:00: **70.09%**
- Suburban trips with no return fare: **83.47%**

This indicates substantial post-airport utilization friction, particularly for **late-night and suburban trips**.

Because the airport trip data does not contain `captain_id`, individual captain retention or churn cannot be directly measured. `got_return_fare_within_20min` is therefore treated as a **post-trip utilization proxy**, rather than an individual captain retention metric.

---

## Recommended Priorities

### 1. Recover Existing Funnel Supply

Focus on the largest onboarding leaks:

- **Fitness:** Investigate the e-rickshaw-specific journey and determine whether the issue is missing documentation, requirements, upload workflow, verification, or operations.
- **RC:** Improve document capture, real-time quality feedback, and recovery journeys.
- **Insurance:** Reduce missing-document and verification friction.

Recovery scenarios in the analysis are illustrative and should be treated as scenario estimates rather than causal forecasts.

### 2. Improve Airport Utilization

Focus on the airport supply problem during **21:00–03:00**.

Recommended actions:

- Improve return-fare availability on late-night airport trips.
- Investigate suburban airport corridors with particularly low return-fare rates.
- Reduce late-night captain cancellations.
- Test targeted incentives or positioning mechanisms around high-shortage airport hours.

### 3. Run a Controlled WhatsApp Experiment

`CAMP_WA_002` should not simply be scaled based on the observed association.

Recommended experiment:

```text
Eligible RC-stage captains
          |
          +-- Treatment → WhatsApp
          |
          +-- Control   → No WhatsApp
```

Measure:

- Incremental A2O
- Cost per incremental approved captain
- Optimal message timing, particularly around `48–72 hours`
- Effect across acquisition channels and device tiers

### 4. Targeted Acquisition for the Residual Gap

Targeted airport acquisition is warranted, but it should **not be the first or only intervention**.

The recommended sequence is:

```text
RECOVER
Existing onboarding funnel
        |
        v
FIX
Airport utilization + late-night economics
        |
        v
ACQUIRE
Targeted acquisition for residual shortage
```

---

## Repository Structure

```text
Rapido_Data_Science_Assessment/
│
├── data/
│   ├── captains.csv
│   ├── doc_events.csv
│   ├── approvals.csv
│   ├── activation.csv
│   ├── nudges.csv
│   ├── airport_hourly.csv
│   └── airport_trips.csv
│
├── notebooks/
│   └── rapido_assessment.ipynb
│
├── outputs/
│   ├── charts/
│   └── tables/
│
├── dashboard/
│   └── Rapido_Data_science_Assesment.pbix
│
├── src/
│
├── README.md
├── requirements.txt
├── Rapido_Head_of_Supply_Memo.pdf
└── Rapido_Captain_Acquisition_Supply_Deck.pptx
```

---

## How to Run the Analysis

### 1. Clone the repository

```bash
git clone https://github.com/YashasD11/Rapido_Data_Science_Assessment.git
cd Rapido_Data_Science_Assessment
```

### 2. Create a virtual environment

#### Windows

```powershell
python -m venv .venv
.venv\Scripts\activate
```

#### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the notebook

```bash
jupyter notebook
```

Open:

```text
notebooks/rapido_assessment.ipynb
```

Run the notebook from the first cell through the final cell.

The notebook loads the raw CSV files from the `data/` directory and produces the analysis and supporting outputs.

---

## Deliverables

### Analysis Notebook

**`notebooks/rapido_assessment.ipynb`**

Contains the end-to-end analysis covering:

- Data loading and validation
- Data quality checks
- Captain onboarding funnel
- Funnel leak diagnosis
- `CAMP_WA_002` analysis
- Airport demand-supply analysis
- Post-airport trip utilization
- Scenario analysis
- Recommendations and conclusions

### Power BI Dashboard

**`dashboard/Rapido_Data_science_Assesment.pbix`**

The dashboard contains three pages:

1. **Captain Acquisition**
2. **Campaign & Acquisition**
3. **Airport Supply**

### Head of Supply Memo

**`Rapido_Head_of_Supply_Memo.pdf`**

A two-page executive memo summarizing:

- Key findings
- Funnel priorities
- Campaign assessment
- Airport supply diagnosis
- Recommended actions
- Assumptions and limitations

### Executive Deck

**`Rapido_Captain_Acquisition_Supply_Deck.pptx`**

A six-slide executive presentation summarizing the analysis, key evidence, and recommended action plan.

---

## Methodology & Assumptions

### Cohort Maturity

The signup → approval analysis uses a **15-day maturity buffer** from the assessment data cutoff of:

```text
2026-06-30 23:59:59 IST
```

This is an analytical maturity assumption and should not be interpreted as a stated business SLA.

### Onboarding Funnel

The onboarding funnel is constructed sequentially using the first successful verification pass for each required document.

The sequence analyzed is:

```text
Signup
  ↓
DL
  ↓
RC
  ↓
Aadhaar
  ↓
Permit
  ↓
Fitness
  ↓
Insurance
  ↓
Approval
```

Permit is applicable to Auto/Cab captains based on the assessment data.

### Campaign Analysis

`CAMP_WA_002` comparisons are observational unless otherwise stated.

Differences between WhatsApp-exposed and non-exposed captains may reflect selection, timing, or onboarding-progress differences. Therefore, the campaign analysis recommends randomized experimentation before scaling.

### Airport Analysis

Airport demand-supply analysis is performed at the zone-hour level.

For airport trips, `got_return_fare_within_20min` is used as a utilization proxy because the dataset does not contain `captain_id`.

---

## Limitations

- The assessment dataset is synthetic.
- The 15-day maturity window is an analytical assumption.
- Observational campaign comparisons do not establish causality.
- Airport trip data does not contain `captain_id`, preventing direct individual-level retention or churn analysis.
- Recovery and utilization scenarios represent potential outcomes under stated assumptions and are not causal forecasts.
- Root causes for some onboarding leaks, particularly e-rickshaw Fitness drop-off, require operational validation.

---

## Final Decision Framework

The recommended operating sequence is:

**RECOVER → FIX → ACQUIRE**

> Recover existing onboarding supply first, fix late-night airport utilization and economics second, and use targeted acquisition to close the remaining airport supply gap.

All major interventions should be validated through controlled experiments before scaling.