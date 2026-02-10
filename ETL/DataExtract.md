 Data Extract

## Overview
This document outlines the data extraction process for the A/B Analysis of Crowd-Aware Bus Boarding Decisions project.

## Data Source
- **Source:** LTA DataMall.sg (Land Transport Authority, Singapore)
- **API/Format:** RESTful API
- **Data Type:** Real-time bus operational data
- **Records:** Bus crowding, ETA, and boarding decision logs
- **Time Period:** [Insert date range]


### Bus Stop Selection Rationale
- **Bus Stop ID:** [07111]
- **Location:** [Broadway Hotel]
- **Reasoning:** Selected a high-traffic location close to [Mustafa] 
  and popular market area to capture diverse passenger behavior patterns 
  during peak and off-peak hours
- **Business Relevance:** High-footfall areas experience greater overcrowding 
  risk, making this an ideal case study for crowd-aware boarding optimization

### Data Source
- **Dataset:** DataMall.sg (Singapore's Land Transport Authority)
- **Time Period:** [Your date range]
- **Records:** [Number of records]

## Extraction Process

### 1. Data Access & Authentication
- **API Endpoint:** LTA DataMall API
- **Authentication:** API Key-based
- **Rate Limits:** [Specify requests per minute/hour]
- **Documentation:** https://datamall.lta.gov.sg/

### 2. API Endpoints Used
```
1. BusArrivals
   - /api/BusArrivals
   - Returns: Bus load, opperator, service no, arrivaltime
   
```

### 3. Data Extraction Strategy
- **Approach:** Scheduled API calls every 2 minutes
- **Time window:** Extract historical + real-time data
- **Coverage:** All bus services in Singapore scope

### 4. Extraction Tools & Technologies
- **Language:** Python 3.x
- **Libraries:**
  - `requests` — HTTP API calls
  - `pandas` — Data storage and manipulation
  - `json` — JSON response parsing
  - `time` — Scheduling and rate limiting

### 5. Code Reference
- Extraction script: `etl/scripts/extract.py`
- Handles API authentication, pagination, error handling

### 6. Raw Data Captured
- **Service no:** Bus route/service number
- **Operator:** Name of the bus operator
- **ArrivalTime:** Arrival time of next Bus
- **Load:** Current load status


### 7. Data Quality Checks (Pre-Transform)
- ✅ Validate API response status codes
- ✅ Check for null/missing values in critical fields
- ✅ Verify timestamp consistency
- ✅ Validate ETA values (positive integers)
- ✅ Identify and log API errors or timeouts

### 8. Error Handling
- Retry logic for failed API calls (3 retries with exponential backoff)
- Log failures to error file for investigation
- Continue extraction despite individual failures

### 9. Output
- Raw JSON responses stored in `data/raw/`
  

## Data Dictionary (Raw Extract)

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| service_no | Integer | Bus service/route number | 175 |
| operator | String | Name of the bus operator  | SMRT |
| ArrivalTime | Datetime | Arrival Time of nextbus | 2024-12-15 14:30:00  |
|  Load | String | Current load status | "SEA", "SDA" |



## Dependencies
- Python 3.7+
- requests library
- pandas
- API key from LTA DataMall

## Notes
- Government data is public but subject to LTA Terms of Service
- Real-time nature means data is continuously evolving
- Original raw data preserved for reproducibility
- Extraction is fully automated and scheduled
