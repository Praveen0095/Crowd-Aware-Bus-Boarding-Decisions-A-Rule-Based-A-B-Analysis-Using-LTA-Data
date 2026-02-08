# Crowd Aware Bus Boarding Decisions: A Rule Based AB-Analysis Using LTA Data

## Overview

This project analyzes how additional real-time information influences commuter boarding decisions in a public transport context. Using Singapore's LTA DataMall Bus Arrival API, the analysis evaluates whether displaying passenger crowd density alongside arrival time can reduce poor boarding decisions without reducing overall boarding likelihood. The focus is on decision quality, not prediction accuracy.

## Problem Statement

Commuters typically decide whether to board a bus based on estimated arrival time (ETA) alone. However, boarding a highly crowded bus can negatively impact:

1. Travel comfort
2. Boarding time
3. Overall user experience

This project evaluates whether adding **crowd density** information leads to better boarding decisions.

## Experiment Design (Conceptual A/B Framework)

Since direct user interaction data is unavailable, a rule-based counterfactual analysis is used.

### Group A — Baseline (ETA Only)
```
  Information shown: Bus number + ETA
```
**Decision rule:** Board if ETA ≤ 10 minutes

### Group B — Enhanced Information
```
  Information shown: Bus number + ETA + Crowd Density
```
**Decision rule:** Board if ETA ≤ 10 minutes AND crowd density is not High

*This design isolates the impact of additional information, not user behavior randomness.*

## Data Source

**Public API:** Singapore LTA DataMall – Bus Arrival API

### Data Used

- Estimated Time of Arrival (minutes)
- Passenger crowd density (Low / Medium / High)

*No scraping, private, or personal data is used.*

## Key Metrics

1. **Boarding Rate** — Percentage of cases where the commuter decides to board
2. **Bad Boarding Rate** — Percentage of boarding decisions where the bus is highly crowded

*This metric directly measures decision quality.*

## Decision Logic

### Boolean Representation

**Group A:**
```python
    Board_A = 1 if ETA ≤ 10
```

**Group B:**
```python
    Board_B = 1 if (ETA ≤ 10) AND (Crowd Density ≠ High)
```

## Results Summary

### 1. Boarding Rate

- **Group A:** Stable
- **Group B:** Comparable to Group A

### 2. Bad Boarding Rate

- **Group A:** Present (boarding crowded buses)
- **Group B:** Reduced by 100%

*This shows that adding crowd density information eliminates poor boarding decisions without reducing overall boarding likelihood.*

### 3. Sensitivity Analysis

To ensure robustness, the ETA threshold was varied:

- 7 minutes
- 10 minutes
- 12 minutes

### Observation

- Boarding trends remain consistent
- Bad boarding remains near zero in Group B

*The findings are not overly sensitive to the ETA threshold.*

## Business Impact

1. Improves commuter comfort and trust
2. Reduces overcrowding-related dissatisfaction
3. Supports better boarding distribution without operational changes
4. Demonstrates how information design can improve decision outcomes

## Why This Matters

This project demonstrates how simple, interpretable rules can:

- Improve user decision-making
- Deliver measurable impact
- Avoid unnecessary model complexity

*It reflects a product analytics mindset, focusing on clarity, trade-offs, and actionable insights.*

## Tools & Technologies

- Python (Pandas)
- Rule-based logic
- Public transport open data
- Analytical decision modeling
- PostgreSQL (DataBase)

## Limitations & Future Work

Real user interaction data would enable true randomized A/B testing.

### Future work could incorporate:

- User segmentation (rush vs non-rush)
- Probabilistic boarding models
- Live feedback loops

## Conclusion

Providing crowd density information alongside ETA significantly improves commuter boarding decisions. This project highlights how small UI changes, backed by data, can deliver meaningful real-world impact.
