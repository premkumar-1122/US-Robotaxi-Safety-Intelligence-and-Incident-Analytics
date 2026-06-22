# Data Quality & Engineering Log
## US Robotaxi Safety Intelligence & Incident Analytics

### 1. Dataset Shape & Transformation
* **Original Dataset:** 895 rows, 116 columns.
* **Final Cleaned Dataset:** 861 rows, 23 engineered columns.
* **Rows Removed:** 34 rows removed via structured deduplication (representing updated/amended regulatory filings rather than exact duplicates).
* **Exact Row Duplicates:** Only 1 exact duplicate was found and purged.

### 2. Temporal Completeness & Artifacts
* **2025 Data:** Represents a complete operational year.
* **2026 Data:** Represents partial-year data (January through April).
* **Identified Anomalies:** * A sudden drop in incidents in April 2026 (5 incidents total) is an artifact of the dataset cutoff, not an operational reality.
    * A single isolated incident in April 2025 was flagged as suspicious (potential historical correction or delayed backfill) but retained for completeness.
* **Note:** Incident Dates contain only Month/Year combinations; day-level analysis was correctly deemed invalid.

### 3. Feature Engineering & Normalization
* **Temporal Features Created:** `Incident Year`, `Incident Month`, `Incident Month Name`, `Incident Quarter`, `Reporting Delay Months`.
* **Weather Column Consolidation:** * *Original State:* Sparse binary indicators (`Weather - Clear`, `Weather - Rain`, `Weather - Fog/Smoke/Haze`).
    * *Transformation:* Melted into a single categorical column `Weather Condition`.
    * *Finding:* `Fog/Smoke/Haze` contained 0 instances and was dropped.
    * *Final Distribution:* Clear (680), Unknown (177), Rain (38).

### 4. Reporting Behavior & Compliance Anomalies
An analysis of `Reporting Delay Months` revealed operationally structured reporting pipelines:
* **0-1 Month Delays:** ~97% of all incidents fall here, signaling highly standardized compliance workflows across the major operators.
* **Entity-Specific Anomalies:** * **Zoox & Waymo:** Exhibit highly stable, predictable reporting cadences (roughly 50/50 split between 0 and 1 month).
    * **Tesla:** Exhibits a distinct long-tail reporting delay (e.g., 6 to 8 months). This anomaly was investigated and linked to staged regulatory amendment workflows and de-redaction processes, not missing data.
* **Missing Values / Unknowns:** Zoox displayed an unusually high proportion (~12%) of "Unknown" severity classifications, flagging a potential difference in internal investigation maturity or reporting thresholds compared to peers.

### 5. Exposure Data Limitations
* **Critical Missing Elements:** The dataset lacks mileage, fleet size, active operational hours, and trip counts.
* **Impact:** True statistical rate calculations (e.g., accidents per million miles) are impossible. All metrics are volume and proportion-based.
