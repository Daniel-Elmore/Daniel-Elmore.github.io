---
title: "Are MLB Teams Spending Smarter? (2012–2016)"
excerpt: "Produced a Moneyball-inspired ROI case study using SQL + Python to highlight how disciplined allocation can outpace financial scale."
layout: single
author_profile: true
toc: true
date: 2025-09-06
header:
  teaser: "/assets/images/projects/mlb-roi-hero.png"
---

## Overview
This project examined whether escalating payrolls in Major League Baseball were truly buying more wins in the post-Moneyball era. For a GM or front-office executive, the question matters because efficiency—not just scale—determines competitive advantage. I built a PostgreSQL + Python pipeline integrating the Lahman database with Neil Paine’s WAR dataset, and used OLS regression to measure payroll’s incremental effect on wins after controlling for roster quality. The headline finding: payroll explained only ~12% of team performance, and beyond ~$150M, each additional $10M bought less than half a win.

**Repo:** [GitHub](https://github.com/Daniel-Elmore/Are-MLB-Teams-Spending-Smarter)  
**Tech:** PostgreSQL; Python (pandas, statsmodels, matplotlib, seaborn); Jupyter  
**Seasons:** 2012–2016  
**Data:** Lahman database; Neil Paine WAR dataset  

## Problem
Payroll is often treated as a proxy for competitive strength, but money alone is a poor predictor of outcomes. I defined **efficiency** as wins relative to expected outcomes after controlling for roster WAR—essentially ROI on payroll dollars.

## Approach
- **Ingest & clean:** Built a relational schema in PostgreSQL linking salaries, rosters, and WAR; harmonized team IDs; de-duplicated records.  
- **Feature build:** Aggregated WAR totals by team-year; created derived metrics for WAR per $1M.  
- **Model:** `wins ~ payroll + WAR` estimated in OLS with robust SEs. Residuals formed the basis for efficiency scores.  
- **Validation:** Compared model fit with and without payroll; tested diminishing returns at high payrolls; examined positional ROI.  
- **Outputs:** League-wide trends, team ROI rankings, position-level comparisons.

## Key Results
- **Effect size:** Each additional $100M in payroll corresponded to only **+8 WAR** on average.  
- **Model fit:** Payroll explained just **~12% of performance variation (R² ≈ 0.12)**.  
- **ROI rankings:** Rays and A’s averaged ~0.49 WAR/$1M, nearly 3× more efficient than Yankees and Dodgers at ~0.18 WAR/$1M.  
- **Diminishing returns:** Above ~$150M, marginal gains per dollar declined sharply.  
- **Positional ROI:** Hitters outperformed pitchers by ~0.04 WAR/$1M; pitchers absorbed ~50% of payroll but generated weaker and more volatile returns.  
- **Trend:** ROI gaps narrowed by 2015–2016, but mainly because payroll inflation compressed outcomes toward the mean—not because teams became more efficient.

## Visuals
![](/assets/images/projects/slide06_payroll_vs_war_indexed.png)
*League payroll rose +28% while WAR rose only +4%, widening the efficiency gap.*

![](/assets/images/projects/slide09_payroll_vs_war_scatter.png)
*Rays and A’s turned <$80M payrolls into 30–40 WAR seasons, while Yankees and Dodgers spent $200M+ for similar results.*

![](/assets/images/projects/slide23_war_per_m_by_role_yearly.png)
*Hitters delivered consistently higher ROI (~0.04 WAR/$1M more than pitchers), exposing systemic overvaluation of arms.*

## Business Takeaways
- Position players provided higher and more stable returns than pitchers, indicating stronger marginal ROI.  
- Reallocate from pitching-heavy payrolls toward undervalued hitters and flexible positions (CF, SS, 3B).  
- ROI-based roster construction outperforms brute-force spending—discipline in allocation is more predictive than scale.  

## Limitations
- **Scope:** 2012–2016 sample, regular-season WAR only.  
- **Data constraints:** Injuries, aging curves, and postseason outcomes not modeled.  
- **Modeling:** Results depend on WAR source definitions; residual variance remains high.  

## How to Reproduce
Clone: `git clone https://github.com/Daniel-Elmore/Are-MLB-Teams-Spending-Smarter.git`  
Load the SQL schema from `/Data/SQL/` into PostgreSQL  
This creates the relational structure (players, salaries, teams, WAR, appearances).  
Import the Lahman CSVs and Neil Paine WAR dataset into the schema.  
Open `01_sql_to_pandas.ipynb` in Jupyter to run SQL queries and pull cleaned tables into pandas.  
Explore `02_exploratory_data_analysis.html` for descriptive findings.  
Run `03_refined_charts.ipynb` to generate final figures and ROI visuals.  
See `/Reports/` for the case study PDF and Word write-up.  
