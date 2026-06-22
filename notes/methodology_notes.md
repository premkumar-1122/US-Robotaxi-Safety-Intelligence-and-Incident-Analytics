# Methodology Notes
## US Robotaxi Safety Intelligence & Incident Analytics

### 1. Project Overview & Positioning
This project is designed as a professional-grade **Operational Intelligence Analytics** portfolio piece, distinctly avoiding the tropes of generic data science projects (e.g., standard Kaggle datasets or basic sentiment analysis). The focus is strictly on operational risk patterns, deployment trends, reporting behavior, and interaction complexity across US autonomous driving systems using authentic regulatory data.

### 2. Data Source & Scope
* **Dataset:** Official NHTSA ADS (Autonomous Driving Systems) Crash Dataset.
* **Temporal Scope:** January 2025 through April 2026.
* **Scale:** 895 raw incident reports involving major operators like Waymo, Zoox, Avride, and Tesla.
* **Dimensions:** Reduced from 116 original columns to 18 highly strategic analytical columns to balance comprehensiveness with analytical tractability.

### 3. Core Column Selection Strategy
The following columns were isolated for targeted analysis:
* **Report ID / Reporting Entity:** To track compliance and compare operator footprints.
* **Dates (Incident Date, Submission Date):** To analyze temporal scaling and reporting lag.
* **Location Data (City, State):** For geographic concentration intelligence.
* **Roadway Type / Weather:** To assess environmental operational design domains.
* **Crash Counterpart:** To evaluate mixed-traffic interaction risks.
* **Injury Severity:** As the primary safety outcome KPI.
* **Vehicle Speed / Engagement Status:** To determine operational state during the incident.
* **Incident Narratives:** Preserved for future NLP/text analytics pipelines.

### 4. Deduplication & Canonical Data Policy
The dataset acts as a living regulatory reporting system, not a static log. Repeated `Report IDs` represent evolving investigations, not necessarily bad data. A formal business-logic deduplication policy was applied:
* **Rule 1 - Default:** Keep the row with the latest *Report Submission Date* (captures de-redactions and updated classifications).
* **Rule 2 - Severity Escalations:** If severity changes, keep the latest severity version (injury confirmation often occurs later).
* **Rule 3 - System Reclassification:** Prioritize later filings that explicitly correct system type (e.g., Waymo ADS vs. ADAS correction).
* **Rule 4 - Manual Exceptions:** Unresolved severity conflicts (e.g., specific Tesla and Avride records) are documented and kept for ambiguity context rather than deleted.

### 5. Interpretative Analytical Framework
To maintain a senior-level analytical posture, the following interpretive rules were strictly adhered to:
* **Volume ≠ Risk:** Increases in incident volume are interpreted as *deployment scaling and operational expansion*, not deteriorating safety, unless exposure data (miles driven) proves otherwise.
* **Dominance ≠ Inferiority:** An operator's high incident count (e.g., Waymo) reflects a larger operational footprint and mature compliance workflows.
* **Temporal Coverage Awareness:** The dataset cuts off in April 2026. The Q2 2026 drop-off is recognized as a data ingestion artifact, not a sudden safety improvement.
