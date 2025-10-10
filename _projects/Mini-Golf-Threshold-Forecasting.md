---
title: "Threshold Forecasting Under Rare Events: Mini-Golf Milestone Prediction"
excerpt: "Applied Bayesian survival analysis and Monte Carlo simulation to forecast when a rare mini-golf performance threshold would be achieved."
layout: single
author_profile: true
toc: true
date: 2025-10-09
image:
  path: /assets/images/projects/Mini-Golf-Threshold-Forecasting-Hero.png
  thumbnail: /assets/images/projects/Mini-Golf-Threshold-Forecasting-Hero.png
  teaser: /assets/images/projects/Mini-Golf-Threshold-Forecasting-Hero.png

header:
  teaser: "/assets/images/projects/Mini-Golf-Threshold-Forecasting-Hero.png"
---

![](/assets/images/projects/Mini-Golf-Threshold-Forecasting-Hero.png)

## Overview
Applied Bayesian survival analysis and Monte Carlo simulation to forecast when a rare mini-golf performance threshold, **breaking 30 strokes over an 18-hole course**, would be achieved. Each season functions as a time-to-event process that continues until the Sanicki brothers record a 29 or lower in a two-man scramble. The project reframes that pursuit as a **rare-event forecasting problem**, using survival curves to anchor historical timing, Bayesian updating to refine probabilities as new rounds arrive, and Monte Carlo simulation to translate uncertainty into single-round decision odds.

**Repo:** [GitHub](https://github.com/Daniel-Elmore/Mini-Golf-Threshold-Forecasting)  
**Tech:** Python (pandas, lifelines, PyMC, ArviZ, matplotlib); Jupyter  
**Methods:** Kaplan–Meier survival analysis, Bayesian log-normal AFT, Monte Carlo simulation  
**Notebooks:** `00_prepare_current_season.html`, `01_historical_priors.html`, `02_live_forecast_season.html`  
**Artifacts:** [Report (PDF)](/assets/docs/Threshold Forecasting Under Rare Events - Breaking 30 Report.pdf), [Presentation Deck (PDF)](/assets/docs/Threshold Forecasting Under Rare Events - Breaking 30 Presentation Deck.pdf)

## Problem
Forecasting **when** a rare milestone will occur requires transforming uncertainty from an obstacle into structure. With only one event per season and ten observed rounds so far, the task was to infer the evolving probability surface for milestone completion rather than rely on binary outcomes.

## Approach
- **Historical priors:** Estimated completion-time distributions (Days 13–52) using Kaplan–Meier curves and a pooled Bayesian log-normal AFT model with hierarchical course effects.  
- **Posterior updating:** Conditioned priors on ten observed rounds without success to obtain a refined forecast of completion timing.  
- **Monte Carlo simulation:** Drew 10,000 posterior paths to approximate the probability of a sub-30 round under current form and visualize the likely completion window.  
- **Diagnostics:** Evaluated front- and back-nine performance to locate remaining constraints on breakthrough probability.

## Key Results (through Day 10)
- **Posterior median completion:** Approximately **Day 29**, up slightly from the historical median (about Day 26), reflecting slower early progress.  
- **Credible interval tightening:** Ninety percent of posterior mass falls between **Days 25 and 30**, reducing timing uncertainty by roughly half compared with the pooled prior.  
- **Single-round odds:** About **0.93 percent** chance of breaking 30 in any given round at Day 10, with most simulated outcomes in the **33–34 stroke** range.  
- **Performance pattern:** Front nine (about 15–16 strokes, best 14) remains efficient, while the back nine (around 17–17.5 average) continues to limit breakthrough potential.  

## Visuals
![](/assets/images/projects/priors_completion_bands_Pooled.png)  
*Figure 1. Posterior completion probability bands based on pooled historical data. The median trajectory indicates that milestone completion typically occurs near Day 27, while the shaded region reflects the 90 percent credible interval, spanning approximately Day 21 to Day 34. The gradual slope and widening uncertainty beyond Day 30 illustrate both the rarity of early success and the persistent possibility of long-tail outcomes. This distribution serves as the empirical benchmark against which live forecasts are subsequently evaluated.*

![](/assets/images/projects/prior_vs_live_overlay_bayes.png)  
*Figure 2. Bayesian forecast after ten days (red) compared with the pooled historical prior (gray). Incorporating early rounds narrows the credible interval and shifts the median completion estimate to Day 29, delineating a plausible window for a successful sub-30 round while quantifying the risk of delay.*

![](/assets/images/projects/spaghetti_rounds_10000.png)  
*Figure 3. Monte Carlo simulation of 10,000 possible rounds. Most simulated outcomes cluster around 33 to 34 strokes, with only 0.93 percent meeting the sub-30 threshold, quantifying the narrowness of the current path to success.*

## Insights & Applications
- Rare events become forecastable when uncertainty is explicitly modeled.  
- Early probability narrowing translates directly to earlier action in KPI or milestone contexts.  
- The framework generalizes to operational thresholds in business settings where leaders must decide before certainty arrives.

## Limitations
- Five historical seasons and one live sample limit statistical power.  
- The log-normal AFT assumption simplifies potential multi-modal learning paths.  
- Exogenous covariates such as course conditions, fatigue, and weather are omitted in this iteration.

## How to Reproduce
Clone:
```bash
git clone https://github.com/Daniel-Elmore/Mini-Golf-Threshold-Forecasting.git
