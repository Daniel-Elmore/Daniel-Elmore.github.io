---
title: "Are MLB Teams Spending Smarter? (2012–2016)"
excerpt: "SQL + Python case study showing payroll explained only ~12% of wins and efficiency gaps of 10+ wins between teams."
layout: single
author_profile: true
toc: true
---

## Overview
This project asked whether escalating payrolls were truly buying more wins in the post-Moneyball era. For a GM or front-office executive, the question matters because efficiency—not just scale—determines competitive advantage. I built a SQL + Python pipeline integrating the Lahman database with Neil Paine’s WAR dataset, and used OLS regression to isolate payroll’s incremental effect on wins after controlling for roster quality. The headline finding: payroll explained only about 12% of performance variation, and beyond ~$150M, each additional $10M purchased fewer than half a win.

**Repo:** [GitHub](https://github.com/Daniel-Elmore/Are-MLB-Teams-Spending-Smarter)  
**Tech:** SQL; Python (pandas, statsmodels, matplotlib, seaborn)  
**Seasons:** 2012–2016  
**Data:** Lahman database; Neil Paine WAR dataset; USA Today payrolls

## Problem
Payroll is often treated as a proxy for competitive strength, but money alone is a poor predictor of outcomes. I defined **efficiency** as wins relative to expected outcomes after controlling for roster WAR and covariates—essentially ROI on payroll dollars.

## Approach
- **Ingest & clean:** Built a relational schema in SQL linking salaries, rosters, and WAR; harmonized team IDs; removed duplicates; adjusted for inflation.  
- **Feature build:** Aggregated WAR totals by team-year; added prior-year wins and division dummies.  
- **Model:** `wins ~ payroll + WAR` estimated in OLS with robust SEs. Residuals formed the basis for efficiency scores.  
- **Validation:** Compared R² with and without payroll; residual diagnostics confirmed robustness.  
- **Outputs:** Team ROI rankings, year-over-year trends, and position-level efficiency charts.

## Key Results
- **Effect size:** +$10M payroll → **+0.8 wins** (95% CI [0.5, 1.1]).  
- **Incremental fit:** **ΔR² = 0.12** when adding payroll to WAR-only model.  
- **ROI rankings:** Top: Rays, Athletics, Royals (+6–8 wins vs expected). Bottom: Yankees, Dodgers, Phillies (−5 to −7).  
- **Stability:** ROI rankings regressed toward the mean by 2015–2016, suggesting inflation diluted ROI differences more than teams improving allocation.

## Visual
![Payroll efficiency vs. expected wins (2012–2016)](/assets/images/projects/mlb-roi-hero.png)

## Business Takeaways
- Position players delivered ~0.04 WAR per $1M more than pitchers, implying higher marginal ROI.  
- Reallocate from pitching-heavy payrolls to versatile hitters; pitchers absorbed ~50% of payroll but underperformed.  
- Embed ROI-adjusted roster construction—balance upside positions (CF, SS, 3B) with stability and avoid overpaying for volatile arms.

## Limitations
- Data scope: WAR source differences, postseason excluded, sample limited to 2012–2016.  
- Omitted variables: injuries, aging curves, schedule strength.

## How to Reproduce
1. Clone: `git clone https://github.com/Daniel-Elmore/Are-MLB-Teams-Spending-Smarter.git`  
2. Load the SQL schema from `/Data/SQL/` into PostgreSQL
   - This creates the relational structure (players, salaries, teams, WAR, appearances).  
   - Import the Lahman CSVs and Neil Paine WAR dataset into the schema.  
3. Open `01_sql_to_pandas.ipynb` in Jupyter to run SQL queries and pull cleaned tables into pandas.
4. Explore `02_exploratory_data_analysis.html` for descriptive findings.  
5. Run `03_refined_charts.ipynb` to generate final figures and ROI visuals.  
6. See `/Reports/` for the case study PDF and Word write-up.  

