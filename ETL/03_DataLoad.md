# Data Loading & Persistence

## Purpose
This document describes how the transformed dataset is persisted
into PostgreSQL for analysis, querying, and reporting.

The focus is on creating a **reliable, reproducible analytics pipeline**.

---

## Storage Layer
- Database: PostgreSQL
- Database name: `Bus_arrival`
- Schema: `public`

The database is used to store processed, analysis-ready data.

---

## Table Design

### Table: `bus_boardinggrpa` and `bus_boardinggrpb`
```sql
CREATE TABLE bus_boardinggrpa (
     ServiceNo INT,
     Operator TEXT,
     ArrivalTime TIMESTAMPTZ,
     CurrentTime TIMESTAMPTZ,
     ETA INT,
     WillBord INT
);
```
```sql
CREATE TABLE bus_boardinggrpb (
     ServiceNo INT,
     Operator TEXT,
     ArrivalTime TIMESTAMPTZ,
     CurrentTime TIMESTAMPTZ,
     ETA INT,
     C_density TEXT,
     WillBord INT
);
