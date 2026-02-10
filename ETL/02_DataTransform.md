# Data Transformation Logic

## Purpose
This document describes how raw bus arrival data is transformed into
decision-ready features for evaluating commuter boarding behavior.

The transformation focuses on **clarity, interpretability, and business relevance**
rather than complex modeling.

---

## Input Data
Source: Public LTA DataMall – Bus Arrival API

Key fields used:
- Estimated Time of Arrival (ETA, minutes)
- Passenger crowd density (Low / Medium / High)
- Bus service identifier
- Event timestamp

No personal or private data is used.

---

## Feature Engineering

### 1. ETA Normalization
ETA values are converted to integer minutes to ensure consistency
across all decision rules.

---

### 2. Crowd Density Categorization
Crowd density is mapped into three interpretable levels:
- Low
- Medium
- High

This aligns with how commuters intuitively perceive crowding.

---

## Decision Rule Construction

The goal is to simulate commuter boarding decisions under
different information conditions.

---

### Group A — Baseline (ETA Only)

**Assumption**  
The commuter only sees ETA.

**Decision Rule**
```
  WillBoard = 1 if ETA ≤ 10
  WillBoard = 0 otherwise
  This represents time-based decision-making without crowd awareness.
```
---

### Group B — Enhanced Information (ETA + Crowd Density)

**Assumption**  
The commuter sees ETA and passenger crowd level.

**Decision Rule**
```
  WillBoard_B = 1 if (ETA ≤ 10) AND (Crowd Density ≠ High)
  WillBoard_B = 0 otherwise
```
This rule reflects a trade-off between waiting time and comfort.

---

## Output Columns

The transformed dataset contains:
- bus_no
- eta_minutes
- crowd_density
- Willboard (binary boarding indicator)
- CurrentTime(real-timestamp with timezone)
- ArrivalTime(timestamp with timezone)

The `WillBoard` column is encoded as:
- `1` → Boarding decision
- `0` → No boarding decision

---

## Sensitivity Checks

To ensure robustness, the ETA threshold was varied:
- 7 minutes
- 10 minutes
- 12 minutes

Key observation:
- Boarding trends remain consistent
- Poor boarding decisions are eliminated in Group B

---

## Design Rationale

- Rule-based logic ensures transparency
- Decisions are explainable and auditable
- Approach mirrors real-world product decision constraints

---

## Outcome
The transformed dataset enables direct comparison of
boarding behavior under different information scenarios,
supporting downstream analysis and business insights.
