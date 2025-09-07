---
title: "User Behavior and Citi Bike"
excerpt: "Applied behavioral economics and data analytics to NYC’s bike share system (2016–2023), finding numerous statistically significant differences."
date: 2023-11-27
layout: single
author_profile: true
toc: true
header:
  teaser: "/assets/images/projects/city-bike-hero.png"
---

## Overview
This project analyzed Citi Bike’s New York system through the lens of behavioral economics and data analytics. I examined over **362,000 trips from May 2016–2023**, running statistical tests on distance, duration, fees, and station preferences. The goal was to understand how subscribers and casual riders differ — and what that means for pricing models, station placement, and service design.

**Repo:** [GitHub](https://github.com/Daniel-Elmore/User-Behavior-and-Citi-Bike-A-Study-of-Behavioral-Economics-and-Data-Analytics)  
**Tech:** R (tidyverse, ggplot2, inferential tests)  
**Period:** May 2016 – May 2023  
**Data:** [NYC OpenData Citi Bike System Data](https://data.cityofnewyork.us/Transportation/Citi-Bike-System-Data/vsnr-94wk)

## Problem
Bike share operators balance **accessibility vs. profitability**. Misaligned pricing or station distribution can create inefficiency. This study asked:  
- Do casual riders and subscribers show systematically different behaviors?  
- What incentives or penalties (fees, membership options) best align with actual usage?  

## Approach
- **Cleaning:** Consolidated and filtered 2016–2023 trip history data, removing underused stations.  
- **Tests:**  
  - Distances: histograms, boxplots, two-sample t-tests.  
  - Durations: t-tests, variance analysis, lateness rates.  
  - Station preferences: chi-square tests for start vs. end stations.  
- **Business case:** Evaluated pricing and infrastructure proposals based on findings.  

## Key Results
- **Distance:** Subscribers averaged ~1.08 km per ride; customers ~1.17 km (p < 0.05).  
- **Duration:** Customers ~33 min, subscribers ~11 min — despite longer official time limits for subscribers.  
- **Station use:** “Grove St PATH” (Jersey City) consistently ranked top for both groups.  
- **Outliers:** Customers ~3× as likely to start and end at the same station.  

## Business Recommendations
- Simplify pricing: merge Citi Bike and Lyft Pink memberships.  
- Raise late fees to encourage turnover.  
- Demand-based unlock pricing for casual riders.  
- Expand stations near high-footfall transit hubs.  

## Limitations
- May-only data excludes peak summer/winter holiday periods.  
- Usage patterns shaped by tourism (esp. casual customers).  
- Focused on ride history; demographic data unavailable.  

## How to Reproduce
1. Clone repo:  
   ```bash
   git clone https://github.com/Daniel-Elmore/User-Behavior-and-Citi-Bike-A-Study-of-Behavioral-Economics-and-Data-Analytics.git
