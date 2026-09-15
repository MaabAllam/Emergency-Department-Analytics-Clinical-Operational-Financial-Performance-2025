# Emergency-Department-Analytics-Clinical-Operational-Financial-Performance-2025
End-to-end Emergency Department analytics project using SQL, Python and Power BI to evaluate clinical, operational, financial and resource performance.
# Emergency Department Analytics

## Clinical, Operational & Financial Performance — 2025

---

## 1. Project Overview

This project analyzes **10,000 synthetic Emergency Department encounters from 2025** to evaluate clinical acuity, patient demand, operational performance, resource utilization, and estimated financial activity.

The project follows an end-to-end healthcare analytics workflow:

**Raw Data → Data Validation → SQL Analysis → Python EDA → Power BI Dashboard → Business Insights**

The objective was to transform healthcare encounter data into meaningful insights that could support clinical, operational, and financial decision-making.

---

## 2. Business Objective

The analysis evaluates Emergency Department performance across four major areas:

* **Clinical performance**
* **Operational performance**
* **Financial performance**
* **Resource utilization**

The project also evaluates data quality and identifies fields requiring further validation before being used for production-level reporting.

---

## 3. Key Business Questions

### Clinical Performance

* How does patient acuity vary across Emergency Department encounters?
* How does triage acuity relate to admission and ICU utilization?
* Which chief complaints are associated with higher admission rates?
* Which chief complaints are associated with longer lengths of stay?

### Operational Performance

* What are the overall Emergency Department waiting time and length of stay?
* How does waiting time vary by clinical acuity?
* How does patient demand vary by shift and month?
* Which shifts contribute most to Emergency Department volume?
* How does operational performance vary across hospitals?

### Financial & Resource Performance

* What is the total and average estimated financial activity?
* How do estimated charges vary by hospital, payer, acuity, admission status, and chief complaint?
* How does clinical acuity relate to laboratory and medication utilization?
* Which patient groups generate the highest estimated charges?

### Data Quality

* Are encounter identifiers unique?
* Are key fields complete?
* Are admission and disposition fields consistently defined?
* Does the LOS field reconcile with other operational time components?
* Are LWBS-related fields internally consistent?

---

## 4. Dataset

The dataset contains:

* **10,000 Emergency Department encounters**
* **31 variables**
* **2025 encounter data**
* Clinical, operational, financial, demographic, payer, and resource-utilization fields

### Main Data Categories

**Patient & Encounter**

* Patient ID
* Encounter ID
* Arrival datetime
* Hospital
* Region
* Age
* Age group
* Gender

**Clinical**

* Chief complaint
* Triage acuity
* Arrival mode
* Disposition
* Admission status
* ICU flag
* 72-hour readmission flag

**Operational**

* Wait time
* Service time
* Boarding time
* Length of stay
* Shift
* Hour of arrival
* Day of week
* Month
* Weekend indicator

**Financial & Resources**

* Payer type
* Estimated charge
* Laboratory orders
* Medications administered

---

## 5. Tools & Technologies

### SQL

Used for:

* Data aggregation
* KPI calculation
* Grouping and filtering
* Clinical and operational comparisons
* Financial analysis
* Business-question analysis

### Python

Used for:

* Data validation
* Exploratory data analysis
* Descriptive statistics
* Distribution and skewness analysis
* Correlation analysis
* Outlier detection
* Clinical and financial pattern analysis

### Power BI

Used for:

* Interactive dashboard development
* KPI visualization
* Clinical and operational reporting
* Financial analysis
* Executive insights and recommendations

---

# 6. Data Validation

Before performing the analysis, the dataset was checked for structural and data-quality issues.

### Validation Results

* Dataset size: **10,000 rows × 31 columns**
* Missing values: **No missing values identified**
* Encounter ID: **10,000 unique values**
* Duplicate encounter IDs: **0**

Datetime fields were converted and validated before analysis.

### Important Data Quality Findings

#### Admission vs. Disposition

The admission flag and disposition field do not produce identical admission counts.

* `admitted = Yes`: **1,513 encounters**
* `disposition = Admitted`: **1,665 encounters**

This indicates that the definitions or business rules behind these fields should be validated against an organization's official data dictionary before production reporting.

#### LWBS Definition

The dedicated LWBS flag and disposition field also produce different counts.

* Dedicated LWBS flag: **11 encounters**
* Disposition = Left Without Being Seen: **565 encounters**

For the dashboard KPI, the dedicated `left_without_being_seen` flag was used rather than assuming that the disposition value represented the same business definition.

#### Length of Stay

Length of stay was compared with:

**Wait Time + Service Time + Boarding Time**

The values did not consistently reconcile, indicating that the LOS field likely follows a different definition or calculation method.

This field should therefore be validated before being used as a production operational KPI.

---

# 7. SQL Analysis

SQL was used to answer the main healthcare business questions and create aggregated analytical views.

Key analyses included:

* Total ED encounters
* Patient volume by hospital
* Patient volume by shift
* Patient volume by day and month
* Waiting time by acuity
* Admission rate by acuity
* ICU rate by acuity
* Readmission rate
* Estimated charges by payer
* Estimated charges by hospital
* Estimated charges by chief complaint
* Estimated charges by triage acuity
* Resource utilization by acuity

The SQL analysis provided the foundation for the Python analysis and Power BI dashboard.

---

# 8. Python Exploratory Data Analysis

Python was used to investigate distributions, relationships, outliers, and patterns that were not limited to simple aggregation.

### Distribution Analysis

Observed skewness:

* Waiting time: **0.24**
* Length of stay: **0.25**
* Estimated charges: **0.99**

Estimated charges showed the strongest positive skew.

### Estimated Charge Outliers

Using the IQR method:

* Upper outlier threshold: approximately **$17,453**
* Identified high-charge encounters: **436**
* Percentage of encounters: **4.36%**
* Average charge among high-charge encounters: approximately **$20.6K**
* Overall average charge: approximately **$7.87K**

High-charge encounters were strongly concentrated among high-acuity patients.

### Correlation Analysis

The strongest observed relationships included:

* Service time vs. LOS: **0.92**
* Waiting time vs. estimated charges: **0.79**
* Laboratory orders vs. estimated charges: **0.78**
* Waiting time vs. laboratory orders: **0.65**
* Medications vs. estimated charges: **0.60**

These results indicate statistical associations within the dataset and should not be interpreted as evidence of causation.

---

# 9. Key Findings

## Clinical Findings

Clinical acuity showed a clear relationship with admission rates.

Admission rate increased from:

* **3.66%** for Non-Urgent encounters
* **8.44%** for Less Urgent
* **18.58%** for Urgent
* **27.08%** for Emergent
* **37.11%** for Resuscitation

This demonstrates a strong acuity-to-admission gradient within the dataset.

---

## Operational Findings

Waiting time increased progressively with clinical acuity.

Average waiting time:

* Non-Urgent: **20.47 minutes**
* Less Urgent: **35.13 minutes**
* Urgent: **54.93 minutes**
* Emergent: **75.20 minutes**
* Resuscitation: **89.91 minutes**

This suggests that waiting-time analysis should be interpreted together with clinical acuity and workload rather than as an isolated operational measure.

---

## Emergency Department Volume

Total encounters:

**10,000**

Shift volume:

* Day: **4,654**
* Evening: **3,169**
* Night: **2,177**

The Day shift represented the highest encounter volume.

---

## Financial Findings

Total estimated charges:

**$78.68 million**

Average estimated charge:

**$7,867.71**

Average estimated charges increased substantially with acuity:

* Non-Urgent: **$2,738.62**
* Less Urgent: **$4,694.93**
* Urgent: **$8,924.66**
* Emergent: **$14,250.30**
* Resuscitation: **$20,706.89**

Resuscitation encounters had the highest average estimated charge.

However, total financial activity was strongly influenced by encounter volume. Urgent encounters generated the highest total estimated charges because of their substantially larger patient volume.

---

## Resource Utilization

Resource utilization also increased with clinical acuity.

Average laboratory orders:

* Non-Urgent: **2.01**
* Resuscitation: **14.13**

Average medications administered:

* Non-Urgent: **1.00**
* Resuscitation: **5.89**

This indicates an association between higher clinical acuity and greater resource utilization within the dataset.

---

# 10. Power BI Dashboard

The Power BI dashboard consists of four pages.

### Page 1 — Executive Overview

Provides a high-level summary of:

* Total encounters
* Total estimated charges
* Admission rate
* Average estimated charge
* Average waiting time
* Average LOS
* ICU rate
* LWBS rate
* Hospital volume
* Monthly encounter volume
* Triage acuity distribution

### Page 2 — Clinical & Operational Performance

Focuses on:

* Triage acuity
* Chief complaints
* Admission rates
* Waiting time
* Length of stay
* Shift performance
* Patient demand

### Page 3 — Financial & Resource Analysis

Focuses on:

* Estimated charges
* Hospital financial activity
* Payer performance
* Admission status
* Chief complaints
* Triage acuity
* Laboratory utilization
* Medication utilization
* Shift-level financial activity

### Page 4 — Key Insights & Recommendations

Summarizes:

* Clinical findings
* Operational findings
* Financial findings
* Resource utilization
* Data quality limitations
* Recommended areas for further analysis

---

# 11. Business Recommendations

### 1. Staffing & Capacity Planning

Use encounter volume and acuity patterns to support evaluation of staffing and capacity requirements across shifts.

### 2. Waiting-Time Management

Evaluate waiting time alongside triage workload, provider availability, staffing levels, and patient volume.

### 3. High-Acuity Resource Planning

Monitor laboratory, medication, admission, and LOS patterns among high-acuity encounters when planning clinical resources.

### 4. Financial Performance Monitoring

Combine estimated charge analysis with payer, claims, reimbursement, and denial information for a more complete financial view.

### 5. Data Governance

Establish clear business definitions for admission, disposition, LWBS, LOS, and financial measures before implementing similar KPIs in production reporting.

---

# 12. Data Quality & Limitations

* The dataset is **synthetic** and does not represent real hospital patients or actual hospital financial performance.
* Estimated charges are used as a financial proxy and should not be interpreted as actual collected revenue or reimbursement.
* Admission and disposition fields show definition differences that require validation.
* LWBS indicators require validation because the dedicated LWBS flag and disposition field produce different counts.
* LOS does not consistently reconcile with wait time, service time, and boarding time.
* Correlation results indicate association, not causation.
* Small counts in some clinical categories, particularly ICU-related analysis, should be interpreted cautiously.

---

# 13. Future Analysis

A future phase could integrate additional healthcare data sources to evaluate:

* Revenue Cycle Management
* Claims processing
* Denial rates
* Payer reimbursement
* Authorization performance
* Accounts receivable
* Claims aging
* Clean claim rate
* Coding accuracy
* CPT/HCPCS utilization
* ICD-10-CM diagnosis patterns
* DRG analysis

This would extend the project from Emergency Department performance analytics into broader healthcare financial and Revenue Cycle Management analytics.

---

# 14. Conclusion

This project demonstrates an end-to-end healthcare analytics workflow, combining SQL, Python, and Power BI to analyze clinical, operational, financial, and resource-utilization data.

The analysis demonstrates how healthcare encounter data can be transformed into business-oriented insights while maintaining attention to data quality, metric definitions, and analytical limitations.

The project also provides a foundation for future healthcare analytics work involving **Revenue Cycle Management, claims, payer performance, and reimbursement analysis**.
