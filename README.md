# 🏥 Healthcare Data Pipeline using Medallion Architecture

## 📌 Project Overview

Healthcare organizations generate large amounts of data such as patient records, admissions, billing information, and medical history. This project demonstrates how to build a scalable Healthcare Data Pipeline using Databricks, PySpark, SQL, and Delta Lake.

The pipeline follows the **Medallion Architecture (Bronze → Silver → Gold)** to transform raw healthcare data into clean, structured, and analytics-ready datasets.

---

# 🎯 Objectives

- Centralize healthcare data
- Improve data quality and consistency
- Enable structured analytics
- Build a scalable ETL pipeline
- Prepare data for dashboards and machine learning

---

# 🏗 Solution Approach

This project is implemented using the **Medallion Architecture**, which consists of three layers:

```
Raw Dataset
      │
      ▼
Bronze Layer
(Raw Data)
      │
      ▼
Silver Layer
(Cleaned & Transformed Data)
      │
      ▼
Gold Layer
(Business Analytics)
```

---

# 📂 Project Structure

```
Healthcare-Data-Pipeline/
│
├── data/
│   └── healthcare_dataset.csv
│
├── notebooks/
│   └── Healthcare_Data_Pipeline.ipynb
│
├── screenshots/
│
├── README.md
└── LICENSE
```

---

# 📊 Dataset

The dataset contains healthcare records including:

- Name
- Age
- Gender
- Blood Type
- Medical Condition
- Date of Admission
- Doctor
- Hospital
- Insurance Provider
- Billing Amount
- Room Number
- Admission Type
- Discharge Date
- Medication
- Test Results

---

# 📥 Upload Dataset into Databricks

1. Open **Databricks Workspace**.
2. Go to **Catalog** or **Data**.
3. Click **Create Table**.
4. Upload `healthcare_dataset.csv`.
5. Name the table **patients_records**.
6. Verify the data:

```sql
SELECT * FROM patients_records LIMIT 20;
```

---

# 🥉 Bronze Layer (Raw Data)

### Purpose

Store raw healthcare data without any modifications.

### Steps Performed

- Loaded dataset from Databricks table.
- Displayed sample records.
- Checked schema.
- Counted rows and columns.
- Generated descriptive statistics.

### Code Used

```python
df = spark.table("patients_records")

display(df)

df.printSchema()

display(df.describe())
```

---

# 🥈 Silver Layer (Data Cleaning & Transformation)

The Silver layer improves data quality and prepares data for analysis.

## 1. Renamed Columns

Converted column names into snake_case.

Example:

```
Billing Amount

↓

billing_amount
```

---

## 2. Data Quality Checks

Performed:

- Null value detection
- Removed null records
- Removed duplicate records

```python
dropna()

dropDuplicates()
```

---

## 3. Standardized Text

Converted:

- Patient Name
- Doctor
- Hospital

into Proper Case using:

```python
initcap()
```

---

## 4. Feature Engineering

Created new columns:

### stay_days

Calculated using:

```
Discharge Date
-
Admission Date
```

---

### billing_category

Billing Amount

```
<15000 → Low

15000-30000 → Medium

>30000 → High
```

---

### age_group

```
0-17 → Child

18-59 → Adult

60+ → Senior
```

---

## 5. Data Validation

Validated unique values for:

- Gender
- Blood Type
- Admission Type
- Test Results

---

## 6. Generate Patient ID

Generated unique patient IDs using:

```python
monotonically_increasing_id()
```

---

## 7. Save Silver Table

Stored cleaned data as a Delta table:

```
silver_patients
```

---

# 🔄 Slowly Changing Dimension (SCD Type 2)

To maintain historical records, SCD Type 2 was implemented.

## Initial Load

Created:

```
silver_patients_history
```

Added:

- start_date
- end_date
- is_current

---

## Simulated Data Change

Updated billing amount for one patient to demonstrate historical tracking.

---

## MERGE Operation

Used Delta Lake MERGE to:

- Expire old records
- Insert new records
- Preserve complete history

---

# 🥇 Gold Layer (Business Insights)

Created analytics-ready tables.

### Gold Tables

| Table | Purpose |
|--------|----------|
| gold_patient_per_hospital | Patient count by hospital |
| gold_avg_billing | Average billing amount |
| gold_insurance_analysis | Insurance provider analysis |
| gold_gender_distribution | Gender-wise patient distribution |
| gold_admission_type | Admission type distribution |
| gold_hospital_ranking | Hospital ranking by revenue |
| gold_doctor_performance | Number of patients treated by each doctor |
| gold_top_diseases | Most common medical conditions |

---

# 🛠 Technology Stack

- Python
- PySpark
- Spark SQL
- Databricks
- Delta Lake

**Conceptual Technologies**

- AWS S3 / Azure Data Lake Storage (ADLS)
- Delta Live Tables (DLT)
- IAM

---

# 📚 Data Engineering Concepts Used

- ETL Pipeline
- Medallion Architecture
- Delta Lake
- Feature Engineering
- Data Cleaning
- Data Validation
- Batch Processing
- Slowly Changing Dimension (SCD Type 2)
- MERGE Operation

---

# 🔄 ETL Workflow

### Extract

Loaded healthcare dataset from Databricks table.

### Transform

- Cleaned data
- Removed nulls
- Removed duplicates
- Standardized text
- Created new features
- Generated Patient ID

### Load

Stored transformed data into:

- Silver Layer
- Gold Layer

---

# 📈 Business Insights

The pipeline generates insights such as:

- Total patients per hospital
- Average billing amount
- Hospital revenue ranking
- Insurance provider analysis
- Gender distribution
- Admission type distribution
- Doctor performance
- Top medical conditions

---

# ⚙ Processing Approach

This project uses **Batch Processing**, where the complete dataset is processed together.

It can be extended to **Real-Time Streaming** using Apache Kafka.

---

# 🚀 Future Enhancements

### BI Dashboard

Gold layer tables can be connected to:

- Power BI
- Tableau
- Databricks SQL Dashboard

### MLOps

Possible use cases:

- Disease prediction
- Billing prediction
- Hospital workload forecasting
- Patient risk prediction

---

# ▶ How to Run the Project

1. Clone the repository.

```bash
git clone https://github.com/your-username/Healthcare-Data-Pipeline.git
```

2. Upload `healthcare_dataset.csv` into Databricks.

3. Create a table named:

```
patients_records
```

4. Open the notebook.

5. Run all cells sequentially:

- Bronze Layer
- Silver Layer
- SCD Type 2
- Gold Layer

---

# 👩‍💻 Author

**Laxmi**

Healthcare Data Pipeline using Databricks, PySpark, SQL, and Delta Lake.# Healthcare-Data-Pipeline
