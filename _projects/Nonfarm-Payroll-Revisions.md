---
title: "Real-Time Reliability of First-Release Nonfarm Payroll Estimates"
excerpt: "Documented a structural break in U.S. payroll revision risk using interrupted time series and machine learning quantile models across 300 monthly observations from 2000–2024."
permalink: /projects/nonfarm-payroll-revisions/
layout: single
author_profile: true
toc: true
date: 2026-04-02
image:
  path: /assets/images/projects/nonfarm-payroll-hero.png
  thumbnail: /assets/images/projects/nonfarm-payroll-hero.png
  teaser: /assets/images/projects/nonfarm-payroll-hero.png
header:
  teaser: "/assets/images/projects/nonfarm-payroll-hero.png"
---

![](/assets/images/projects/nonfarm-payroll-hero.png)

## Overview

When the BLS released July 2025 payroll data, concurrent revisions erased 258,000 previously reported jobs, triggering market volatility and a politically charged debate over whether the episode was a statistical anomaly or evidence of something more durable. This project investigates that question systematically, using 25 years of vintage payroll data to characterize how revision risk behaves, when it structurally changes, and whether it can be forecast in real time.

Drawing on 300 monthly observations from 2000 to 2024, the analysis documents heavy-tailed revision distributions, identifies COVID-19 as a statistically significant and persistent structural break in measurement volatility, and tests the limits of machine learning for real-time tail-risk prediction.

**Repo:** [GitHub](https://github.com/Daniel-Elmore/Nonfarm-Payroll-Revisions)  
**Tech:** Python (pandas, NumPy, statsmodels, scikit-learn, matplotlib, seaborn); Jupyter  
**Methods:** Interrupted time series, event-study design, HAC-robust inference, quantile regression, rolling out-of-sample validation, leakage auditing  
**Artifacts:** [Presentation (26 slides)](/assets/docs/Nonfarm-Payroll-Revisions-Presentation.pdf) \| [Full Report (97 pages)](/assets/docs/Nonfarm-Payroll-Revisions-Report.pdf)

## Problem

Nonfarm payroll is among the most consequential data releases in U.S. macroeconomics. The Fed responds to it. Markets price it. But the first estimate is almost always revised — sometimes substantially — as more complete administrative data arrive. The standard assumption is that this revision process is stable over time: random noise that cancels out in expectation. If that assumption is wrong, then static confidence intervals mislead precisely when accurate uncertainty quantification matters most.

## Approach

The analysis proceeds in three stages:

**Distributional analysis:** Characterized revision magnitudes across short and long horizons, documented heavy tails and asymmetry, and stratified behavior across macro regimes (pre-GFC, post-GFC, COVID, recovery) and NBER recessions. Established that the revision distribution rejects classical white-noise framing — excess kurtosis of 2.6 and a 95th percentile of 201,000 jobs even outside COVID.

**Interrupted time series:** Applied an ITS design centered on March 2020 to test for a structural break in revision magnitude and tail-event probability. Included monthly fixed effects and HAC-robust inference. Validated with placebo breaks at 2016, 2017, and 2018 (all statistically insignificant), pre-trend tests, and a negative control using short-horizon revisions, which showed no comparable break.

**Machine learning quantile models:** Estimated quantile gradient boosting models targeting the 75th, 90th, and 95th percentiles of revision magnitude using only vintage-safe predictors available at the time of first release — lagged revisions, unemployment, recession indicators, rolling volatility, and first-estimate magnitude. Enforced strict leakage audits. Evaluated performance on rolling out-of-sample windows from 2010 to 2024, with explicit stress testing around COVID onset.

## Key Results

| Finding | Estimate |
|---|---|
| COVID level shift in revision magnitude | +168,000 jobs |
| Jump in tail-event probability at COVID onset | +39 percentage points |
| Months tail risk remained elevated post-shock | 57 months (nearly 5 years) |
| Pre-COVID 95th percentile absolute revision | 201,000 jobs |
| COVID-period 95th percentile absolute revision | 513,000 jobs |
| Placebo ITS level shifts (2016, 2017, 2018) | Statistically insignificant |

- **Heavy tails are a baseline feature, not just a COVID artifact.** Even outside the pandemic, the revision distribution has excess kurtosis of 2.6 and a 95th percentile of 201,000 jobs. Classical measurement error framing does not fit the data.
- **COVID introduced a durable structural break.** The ITS estimate shows a +168,000-job level shift at March 2020 with p < 0.001. Placebo tests at three alternative dates produce small, imprecise results, confirming the break is timing-specific.
- **Tail risk has not returned to its pre-COVID baseline.** After 57 post-COVID months, the probability of a 200,000+ job revision remains approximately 3.3 percentage points above the pre-COVID baseline of zero — inconsistent with a purely transitory shock.
- **ML models improve calibration in stable periods but fail under stress.** Tail misses cluster around the 2008–09 financial crisis and 2020–22 pandemic window. The failure reflects non-stationarity in the revision-generating process, not a fixable model deficiency.

## Visuals

![](/assets/images/projects/nonfarm-payroll-fig1.png)
*Figure 1. Distribution of net long-horizon payroll revisions, 2000–2024. The median sits near zero, but the right tail extends well beyond ±150,000 jobs with overflow bins capturing revisions exceeding −200,000 and +300,000. Excess kurtosis of 2.6 rejects the classical white-noise measurement error model.*

![](/assets/images/projects/nonfarm-payroll-fig2.png)
*Figure 2. Interrupted time series estimate of absolute revision magnitude around the March 2020 COVID onset. The red fitted line shows a flat pre-COVID trend, a sharp +168,000-job level shift at the break, and a gradual post-COVID decay that takes nearly four years to approach the pre-break range. The vertical gray line marks March 2020.*

![](/assets/images/projects/nonfarm-payroll-fig3.png)
*Figure 3. ML model tail-miss diagnostic, 2003–2024. The blue line shows the predicted 90th percentile upper bound; green dots mark months where realized revisions exceeded it. Tail misses are sparse before 2008, cluster around the financial crisis, then surge at COVID onset and persist through 2022–24 — confirming that model failures concentrate precisely at regime breaks.*

## Implications

**For the Federal Reserve:** Payroll is a key FOMC input, but its informational reliability is regime-dependent. Supplementing with alternative indicators (jobless claims, ADP, JOLTS) is most valuable when revision risk appears elevated — which this framework can help identify.

**For financial markets:** Options markets price elevated volatility around payroll releases but may not fully account for persistent, regime-dependent tail risk. Static confidence intervals understate uncertainty in post-shock environments.

**For the BLS:** Making existing sampling-error intervals more prominent and developing real-time quality indicators (response rates, imputation shares) would give data users better tools for assessing reliability at the time of release.

## Limitations

The ITS design supports timing-specific causal attribution but cannot isolate individual operational channels — survey response rate collapse, birth-death model breakdown, and seasonal adjustment failure are likely simultaneous contributors. ML models are evaluated strictly for forecasting accuracy under vintage constraints, not for identifying structural causes. The sample is restricted to U.S. CES; CPS household survey divergence and cross-country comparisons are not addressed.

## How to Reproduce
```bash
git clone https://github.com/Daniel-Elmore/Nonfarm-Payroll-Revisions.git
cd Nonfarm-Payroll-Revisions
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Run notebooks in order:
1. `Notebooks/01_setup.ipynb` — builds derived datasets from raw BLS inputs
2. `Notebooks/02_eda_revision_behavior.ipynb` — distributional analysis
3. `Notebooks/03_quasi_causal_revision_mechanisms.ipynb` — ITS and event-study diagnostics
4. `Notebooks/04_ml_revision_prediction.ipynb` — ML revision-risk models

Static HTML renders of all notebooks are available in `Notebooks/HTML Exports/` for viewing without running code.

**Updated:** April 2026
