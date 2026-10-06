# Hospital Analytics Dashboard

**Portfolio Project | Python (Pandas) | Matplotlib | NumPy | + Power BI**

## Project Overview
This project analyzes hospital admission data to identify patterns in **patient volume, department performance, waiting times, length of stay, billing, revenue, admission type, primary condition, and insurance mix**.

The project emphasizes realistic data cleaning, validation, business-rule checks, analytical thinking, and dashboard storytelling.

## Dataset
- **15,000 hospital admission records**
- **20 core fields**
- Admission, patient, operational, financial, and insurance information
- Controlled realistic data-quality issues for cleaning practice

Key fields: `Admission_ID`, `Patient_ID`, `Admission_Date`, `Discharge_Date`, `Department`, `Primary_Condition`, `Admission_Type`, `Waiting_Time_Min`, `Length_of_Stay_Days`, `Room_Charges`, `Treatment_Charges`, `Medication_Charges`, `Total_Bill`

## Data Cleaning — Python/Pandas
  Performed in a Jupyter Notebook using VS Code.

- Profiled shape, columns, data types, nulls, duplicates, and descriptive statistics.
- Investigated duplicate records and duplicate identifiers.
- Handled missing values using median, mode, and `Unknown` where appropriate.
- Standardized text and categorical values.
- Converted admission/discharge dates to datetime.
- Investigated suspicious numeric values and outliers.
- Checked admission/discharge date consistency using business rules.
- Validated length of stay and performed final post-cleaning validation.

### 2.Exploratory Data Analysis — Python/Pandas & Matplotlib
Performed in the same Jupyter Notebook using VS Code.

Analyzed hospital operations and financial patterns, including:

- Admission volume by department and admission type
- Repeat vs single-admission patients
- Waiting time across departments and admission types
- Revenue and average billing by department
- Revenue and Length of Stay by primary condition
- Insurance-wise admissions and revenue
- Monthly admission and revenue trends
- Doctor-level billing and admission patterns

Created four focused Matplotlib visualizations to support
interpretation of key operational and financial patterns.

The EDA was used to identify operational bottlenecks,
revenue concentration, and areas requiring further investigation.

The analysis was used to identify operational bottlenecks,
revenue concentration, and areas requiring further investigation.

## Power BI Dashboard

### Page 1 — Executive Overview
KPI cards plus overall admissions, revenue, department, time, and admission-type views.

### Page 2 — Operational Analysis
Department admissions, waiting time, average bill, revenue contribution, and admission-type analysis.

### Page 3 — Revenue & Patient Analysis
Revenue by primary condition, average bill/LOS, insurance revenue, and admission mix.

### Page 4 — Hospital Operational & Patient Insights
Department comparisons, admissions vs revenue, LOS vs bill, and admission-type waiting-time patterns.

## Business Questions
- Which departments have the highest admission volume?
- Which departments have higher waiting times?
- Which departments contribute the most revenue?
- Which primary conditions generate the highest revenue?
- How do average LOS and average bill vary by department?
- How do admission types differ in volume and waiting time?
- What is the insurance mix and revenue contribution?
- Are high-volume departments necessarily the highest-revenue departments?

#### Tools & Technologies

- Python
- Pandas
- Matplotlib
- Jupyter Notebook
- VS Code
- Power BI
- DAX
  
**Python/Pandas/Matplotlib:** data cleaning, missing values, duplicates, text standardization, datetime analysis, outlier investigation, business-rule validation.

**Matplotlib:** Created four focused Matplotlib visualizations to support interpretation of key operational and financial patterns

**Power BI/DAX:** KPI measures, DateTable/time analysis, interactive filtering, department analysis, dashboard design, business storytelling.

## Repository Structure
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

