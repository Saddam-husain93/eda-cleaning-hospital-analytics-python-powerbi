# Hospital Analytics Dashboard

**Portfolio Project | June 2026 – August 2026**  
**Tools:** Python, Pandas, Power BI, DAX  
**Dataset:** 15,000 hospital admission records  
**Dashboard:** 4-page Power BI report

---

## 1. Project Overview

This project analyzes hospital admission data to understand patient volume, department performance, waiting times, length of stay, billing, revenue, admission type, primary condition, and insurance mix.

The project follows an end-to-end Data Analyst workflow with emphasis on data profiling, cleaning, validation, focused exploratory analysis, KPI development, dashboard design, and business storytelling.

> **Scope note:** This is an operational and financial analytics project. It is not a clinical decision-support system.

## 2. Business Problem

Hospital admission data can contain patient, operational, and billing information, but raw records do not directly answer management questions. The project therefore focuses on identifying department demand, waiting-time patterns, revenue concentration, billing and length-of-stay differences, admission-type behavior, and insurance contribution.

## 3. Business Objectives

1. Understand overall hospital admission volume.
2. Measure unique patients.
3. Compare admission volume across departments.
4. Identify departments with higher waiting times.
5. Analyze department-level revenue contribution.
6. Compare average billing and length of stay.
7. Understand admission-type patterns.
8. Analyze revenue by primary condition.
9. Understand insurance mix and revenue contribution.
10. Compare volume and financial performance to identify areas for further investigation.

## 4. Dataset

- **Rows:** 15,000
- **Core fields:** 20
- **Time period:** 2024–2025
- **Unit of analysis:** Hospital admission

A patient can have multiple admissions, so `Patient_ID` is not expected to be unique at the admission level.

### Main Fields

| Field | Description |
|---|---|
| `Admission_ID` | Admission identifier |
| `Patient_ID` | Patient identifier |
| `Patient_Name` | Patient name |
| `Admission_Date` | Admission date |
| `Discharge_Date` | Discharge date |
| `Department` | Hospital department |
| `Primary_Condition` | Primary condition |
| `Admission_Type` | Emergency, Scheduled, Referral, or Walk-in |
| `Patient_Age` | Patient age |
| `Gender` | Patient gender |
| `City` | Patient city |
| `Doctor` | Assigned doctor |
| `Insurance_Type` | Insurance/coverage category |
| `Payment_Method` | Payment method |
| `Waiting_Time_Min` | Waiting time in minutes |
| `Length_of_Stay_Days` | Recorded length of stay |
| `Room_Charges` | Room charges |
| `Treatment_Charges` | Treatment charges |
| `Medication_Charges` | Medication charges |
| `Total_Bill` | Total admission bill |

## 5. Data Quality Issues


- Missing values
- Duplicate rows and duplicate identifiers
- Inconsistent categorical values
- Text-format inconsistencies
- Date parsing issues
- Missing/invalid dates
- Impossible admission/discharge sequences
- Invalid age values
- Suspicious waiting-time values
- Length-of-stay inconsistencies
- Missing financial values

Each issue was investigated using business context.

## 6. Data Cleaning & Validation — Python/Pandas

### 6.1 Initial profiling

The raw dataset was profiled for shape, columns, data types, null values, duplicate rows, descriptive statistics, categorical values, identifiers, and suspicious numeric values.

### 6.2 Missing values

Missing values were handled according to field meaning. Numeric fields used context-appropriate strategies where justified; categorical values were assigned `Unknown` where the original category could not be reliably inferred; dates were parsed and validated rather than blindly filled.

### 6.3 Duplicate validation

Duplicate rows were investigated separately from duplicate identifiers. A repeated `Patient_ID` is legitimate because a patient can have multiple admissions, whereas a repeated `Admission_ID` requires investigation. After cleaning, the final dataset contained **0 complete duplicate rows**.

### 6.4 Admission ID investigation

Duplicate admission identifiers were investigated because admission-level records should be uniquely identifiable. The key principle was that a duplicate key does not automatically mean a duplicate row; the business meaning of the identifier must be checked before deciding what to do.

## 7. Date Cleaning and Validation

Admission and discharge dates were converted to datetime and checked for missing values, invalid dates, chronological consistency, and agreement with length of stay.

The business rule used was:

> A discharge date should not occur before the corresponding admission date.

The validation identified **20 records with invalid date chronology**. These records were flagged and investigated rather than silently ignored.

## 8. Length of Stay Validation

A calculated LOS was derived from admission and discharge dates and compared with `Length_of_Stay_Days`.

The validation identified **26 rows where recorded LOS differed from calculated LOS**. The mismatches included impossible negative durations and implausible recorded values, so they were reviewed as business-rule issues rather than treated as ordinary missing values.

## 9. Numeric Validation

### Patient Age

Five clearly invalid age values were identified, including `-3`, `-1`, `125`, `130`, and `150`. These were treated as invalid rather than used in age analysis.

### Waiting Time

`Waiting_Time_Min` was investigated for suspicious values. The raw data included negative values and a value of `999`; the value `999` occurred **5 times** and was investigated rather than assumed to be a normal waiting time.

## 10. Categorical Standardization

Categorical fields were standardized so equivalent values were represented consistently.

Final department categories included:

- Cardiology
- Dermatology
- ENT
- General Medicine
- Gynecology
- Neurology
- Orthopedics
- Pediatrics

Admission types were standardized to:

- Emergency
- Scheduled
- Referral
- Walk-in

Gender was standardized to Female/Male, and city, insurance, and payment categories were also normalized.

## 11. Final Data Validation

Final validation covered missing values, duplicate rows, data types, date consistency, numeric validity, categorical consistency, and LOS logic.

**Final dataset: 15,000 rows × 20 core fields.**

## 12. Exploratory Data Analysis

EDA was intentionally focused on business questions. The analysis covered patient volume, department performance, waiting time, revenue, billing, LOS, admission type, primary condition, insurance mix, and repeat admissions.

## 13. Overall Hospital Metrics

| Metric | Result |
|---|---:|
| Total Admissions | 15,000 |
| Unique Patients | 4,751 |
| Average Admissions per Patient | 3.16 |
| Median Admissions per Patient | 3 |
| Average Waiting Time | 56.35 min |
| Average Bill | 49,427.51 |
| Average LOS | 5.38 days |
| Total Revenue | 741.41M |

## 14. Patient Repeat-Admission Analysis

- **4,751 unique patients**
- **3,995 patients had multiple admissions**
- **756 patients had exactly one admission**
- Maximum admissions for one patient: **10**
- Average admissions per patient: **3.16**
- Median admissions per patient: **3**

Repeat admissions are common in the dataset. They should not, by themselves, be interpreted as evidence of disease severity because clinical severity variables are not available.

## 15. Department Analysis

| Department | Admissions | Avg Waiting (min) | Avg Bill | Revenue % |
|---|---:|---:|---:|---:|
| General Medicine | 3,074 | 57.27 | 40,525.12 | 17% |
| Cardiology | 2,249 | 69.44 | 80,386.25 | 24% |
| ENT | 2,134 | 44.55 | 27,175.80 | 8% |
| Orthopedics | 2,048 | 65.70 | 67,940.61 | 19% |
| Gynecology | 1,533 | 52.59 | 38,179.47 | 8% |
| Neurology | 1,475 | 62.50 | 75,298.61 | 15% |
| Pediatrics | 1,400 | 50.62 | 32,487.84 | 6% |
| Dermatology | 1,087 | 36.59 | 21,928.93 | 3% |

**Observation:** Cardiology had the highest average waiting time and the highest revenue share. This makes it a strong candidate for operational investigation, but the analysis does not establish that reducing waiting time would cause higher revenue.

## 16. Department Revenue

| Department | Revenue |
|---|---:|
| Cardiology | 180.79M |
| Orthopedics | 139.14M |
| General Medicine | 124.57M |
| Neurology | 111.07M |
| Gynecology | 58.53M |
| ENT | 57.99M |
| Pediatrics | 45.48M |
| Dermatology | 23.84M |

Cardiology generated the highest revenue and also had the highest average bill.

## 17. Admission Type Analysis

| Admission Type | Admissions | Avg Waiting (min) | Avg Bill | Revenue |
|---|---:|---:|---:|---:|
| Emergency | 4,477 | 76.13 | 55,772.33 | 249.69M |
| Scheduled | 4,472 | 38.95 | 46,222.53 | 206.71M |
| Walk-in | 3,333 | 53.50 | 45,633.01 | 152.09M |
| Referral | 2,718 | 55.92 | 48,902.89 | 132.92M |

Emergency admissions had the highest average waiting time and highest average bill among admission types.

## 18. Primary Condition Analysis

| Primary Condition | Admissions | Avg Bill | Avg LOS |
|---|---:|---:|---:|
| General | 4,438 | 45,325.91 | 4.79 |
| Respiratory | 2,366 | 50,398.32 | 5.71 |
| Cardiac | 1,257 | 87,136.55 | 7.86 |
| Neurological | 1,185 | 74,833.46 | 7.17 |
| Infection | 2,456 | 35,046.25 | 4.77 |
| Fracture/Injury | 701 | 72,765.80 | 7.12 |
| Gastrointestinal | 1,169 | 37,870.40 | 4.94 |
| Maternity | 498 | 39,544.16 | 4.44 |
| ENT | 520 | 25,504.17 | 3.33 |
| Skin | 410 | 20,725.71 | 2.71 |

Cardiac admissions had the highest average bill and highest average LOS among the listed conditions.

## 19. Revenue by Primary Condition

| Primary Condition | Revenue |
|---|---:|
| General | 201.16M |
| Respiratory | 119.24M |
| Cardiac | 109.53M |
| Neurological | 88.68M |
| Infection | 86.07M |
| Fracture/Injury | 51.01M |
| Gastrointestinal | 44.27M |
| Maternity | 19.69M |
| ENT | 13.26M |
| Skin | 8.50M |


General conditions generated the highest total revenue because of high admission volume, while Cardiac admissions had much higher value per admission.

## 20. Insurance Analysis

| Insurance Type | Admissions | Revenue |
|---|---:|---:|
| Private | 6,287 | 311.40M |
| Government | 3,815 | 186.17M |
| Corporate | 2,637 | 131.57M |
| Self-Pay | 2,196 | 109.23M |
| Unknown | 65 | 3.04M |

Private insurance represented the largest admission group and revenue contribution.

## 21. Doctor-Level Analysis

Doctor-level analysis showed differences in admission volume and average bill. For example, Dr. Mehta had 1,152 admissions with an average bill of 80,380.38, while Dr. Patil had 1,101 admissions with an average bill of 27,031.44.

These differences should not be interpreted as individual doctor performance because the dataset does not contain sufficient case-mix or clinical complexity information.

## 22. Monthly Analysis

The dataset covers 2024 and 2025.

- Average monthly admissions: approximately **625**
- Monthly admissions range: approximately **556–664**
- Average monthly revenue: approximately **30.89M**
- Average bill by month ranged approximately **47.1K–51.8K**

The dashboard therefore focused more strongly on department and operational differences than on large time-series shifts.

# 23. Business Questions and Answers

### Which departments have the highest admission volume?

General Medicine had the highest volume with **3,074 admissions**.

### Which departments have higher waiting times?

Cardiology had the highest department average at approximately **69.44 minutes**, followed by Orthopedics and Neurology.

### Which departments contribute the most revenue?

Cardiology generated approximately **180.79M**, followed by Orthopedics and General Medicine.

### Which primary conditions generate the highest revenue?

General conditions generated the highest total revenue because of volume, while Cardiac admissions had much higher average billing.

### How do LOS and average bill vary?

Cardiac, Neurological, and Fracture/Injury admissions had relatively high average LOS and high average bills.

### How do admission types differ?

Emergency admissions had the highest average waiting time at approximately **76.13 minutes**.

### What is the insurance mix?

Private insurance represented the largest admission volume and revenue contribution.

### Are high-volume departments necessarily the highest-revenue departments?

No. General Medicine had the highest admission volume, while Cardiology generated the highest revenue.

# 24. Power BI Dashboard

A DateTable was used for time-based analysis. The dashboard contains four pages.

## Page 1 — Executive Overview

Provides management-level KPIs for total revenue, admissions, unique patients, average bill, average waiting time, and average LOS, plus department, monthly, and admission-type views.

## Page 2 — Operational Analysis

Focuses on department admissions, waiting time, average bill, revenue contribution, admission-type waiting time, and a department summary table.

## Page 3 — Revenue & Patient Analysis

Focuses on revenue by primary condition, average bill and LOS, insurance revenue, and insurance admission mix.

## Page 4 — Hospital Operational & Patient Insights

Combines department comparisons, admissions versus revenue, LOS versus bill, and admission-type waiting-time patterns.

# 25. DAX Measures

### Total Admissions

```DAX
Total Admissions =
COUNTROWS(hospital_data)
```
Counts admission records.

### Unique Patients

```DAX
Unique Patients =
DISTINCTCOUNT(hospital_data[Patient_ID])
```
Counts distinct patients.

### Total Revenue

```DAX
Total Revenue =
SUM(hospital_data[Total_Bill])
```
Calculates total revenue represented by admission billing.

### Average Bill

```DAX
Average Bill =
AVERAGE(hospital_data[Total_Bill])
```
Calculates average bill per admission.

### Average Waiting Time

```DAX
Average Waiting Time =
AVERAGE(hospital_data[Waiting_Time_Min])
```
Calculates average waiting time in minutes.

### Average LOS

```DAX
Average LOS =
AVERAGE(hospital_data[Length_of_Stay_Days])
```
Calculates average length of stay in days.

### Revenue %

```DAX
Revenue % =
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL(hospital_data[Department])
    )
)
```
Calculates department contribution to overall revenue.


# 26. Dashboard Design Decisions

- KPI cards were used for important hospital-level metrics.
- Bar/column charts were used for categorical comparisons.
- A line chart was used for monthly admissions.
- Scatter charts were used for relationships such as LOS versus bill and admissions versus waiting time.
- Tables were used where exact department-level values were useful.
- Slicers were used selectively rather than adding filters to every page.

The goal was to make each visual answer a business question rather than add visual quantity.

# 27. Key Insights

1. General Medicine had the highest admission volume at **3,074**.
2. Cardiology generated the highest department revenue at approximately **180.79M**.
3. Cardiology also had the highest department waiting time at approximately **69.44 minutes**.
4. Emergency admissions had the highest average waiting time at approximately **76.13 minutes**.
5. High admission volume did not necessarily mean the highest revenue.
6. Cardiac admissions had a high average bill of approximately **87.14K** and average LOS of approximately **7.86 days**.
7. Private insurance represented the largest insurance segment.
8. Repeat admissions were common, with **3,995 patients** having multiple admissions.

# 28. Business Value

The dashboard provides a consolidated view of patient demand, department workload, waiting-time patterns, revenue concentration, billing, LOS, admission type, and insurance mix.

Potential areas for further operational investigation include high-volume departments with elevated waiting times, emergency patient flow, departments with high revenue and high workload, and conditions associated with higher LOS and billing.

These are areas for investigation rather than measured business improvements.

# 29. Important Analytical Limitation

The project identifies relationships and patterns in historical data; it does not establish causation.

For example, a department having high waiting time and high revenue does not prove that reducing waiting time will increase revenue.

A real intervention analysis would require additional information such as staffing, capacity, patient-flow timestamps, operational costs, case mix, and before/after intervention data.

# 30. Project Outcome

The project transformed a 15,000-record hospital dataset containing realistic data-quality issues into a validated analytical dataset and four-page Power BI dashboard to Compare volume and financial performance to identify areas for investigation

The workflow was:

**Raw Data → Profiling → Cleaning → Validation → EDA → Business Analysis → DAX → Power BI Dashboard → Insights**

# 31. Skills Demonstrated

### Python / Pandas

- Data loading
- Data profiling
- Missing-value analysis
- Duplicate detection
- Data type conversion
- Datetime handling
- String standardization
- Categorical normalization
- Numeric validation
- Outlier investigation
- Business-rule validation
- Grouped analysis
- Aggregation

### Power BI

- Data modeling
- DateTable
- KPI cards
- Bar charts
- Line charts
- Tables
- Scatter plots
- Slicers
- Dashboard layout
- Business storytelling

### DAX

- `COUNTROWS`
- `DISTINCTCOUNT`
- `SUM`
- `AVERAGE`
- `DIVIDE`
- `CALCULATE`
- `ALL`

# 32.GitHub Structure

```text
eda-cleaning-hospital-analytics-python-powerbi/
├── data/
│   ├── raw/
│   └── clean/
├── documentation/
│   └──PROJECT_DOCUMENTATION.md
├── notebook/
│   └── hospital_data_cleaning.ipynb Hospital_Dashboard.pbix
├── powerbi/
|    |__ DAX_MEASURES.md
|    |__ Hospital_Dashboard.pbix
|    |__ Hospital_Dashboard.pdf
|
├── screenshots/
│   
└── README.md
```

# 33. GitHub Screenshot Presentation

Use one full-width screenshot per dashboard page with a one-line explanation.

### Executive Overview

> Management-level summary of hospital volume, revenue, waiting time, billing, and length of stay.

### Operational Analysis

> Department workload, waiting time, billing, and revenue contribution for operational review.

### Revenue & Patient Analysis

> Revenue concentration by condition and insurance mix, with billing and LOS comparisons.

### Hospital Operational & Patient Insights

> Combined volume, waiting-time, revenue, billing, and LOS relationships across departments and admission types.

# 34. Future Improvements

For a real hospital analytics engagement, the next stage could combine this dataset with:

- Bed occupancy
- Staffing levels
- Appointment schedules
- Emergency arrival timestamps
- Registration and treatment timestamps
- Discharge processing time
- Department capacity
- Cost data
- Patient satisfaction
- Readmission definitions
- Clinical case-mix information

These additions would allow deeper analysis of capacity utilization, causes of waiting time, cost efficiency, and patient experience.

# 35. Final Summary

This project demonstrates the core Data Analyst workflow:

**Understand the business problem → validate the data → clean the data → analyze the right metrics → communicate findings → avoid unsupported conclusions.**
