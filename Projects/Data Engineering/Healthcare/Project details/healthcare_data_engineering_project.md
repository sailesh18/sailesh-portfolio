
# 🏥 Healthcare Data Engineering Project — Medallion Architecture

## 🚀 Project Overview

This project is a complete data engineering workflow designed using Databricks Medallion Architecture. It models patient visits, doctor information, and treatment costs across multiple hospital departments, transforming raw data into business-ready insights.

---

## 🧱 Medallion Architecture Layers

- ✅ Bronze Layer: Ingested CSV files (patients, doctors, visits, treatments)
- ✅ Silver Layer: Cleaned, joined, and typed dimensional tables
- ✅ ✅ Gold Layer: Aggregated fact/dim model for reporting

---

## 📦 Source Datasets (Bronze Layer)

1. `patients.csv` (600 rows)
2. `doctors.csv` (60 rows)
3. `visits.csv` (1,500 rows)
4. `treatments.csv` (2,000 rows)

---

## 📐 Data Modeling

### 📌 Conceptual Model
- Entities: Patient, Doctor, Visit, Treatment
- Relationships: 
  - A Patient can have many Visits.
  - A Doctor can handle many Visits.
  - A Visit can contain many Treatments.

---

### 🧩 Logical Model (Star Schema)

#### Dimension Tables

- `dim_patients(patient_id PK, patient_name, age, gender, country)`
- `dim_doctors(doctor_id PK, doctor_name, department, years_experience)`
- `dim_visits(visit_id PK, patient_id FK, doctor_id FK, department, visit_date, follow_up_required)`

#### Fact Table

- `fact_treatments(treatment_id PK, visit_id FK, treatment_type, treatment_cost, insurance_provider)`

---

### 💽 Physical Model (Delta Lake)

| Table              | Format | Partitioned By     | Z-Ordering       |
|-------------------|--------|--------------------|------------------|
| bronze.patients   | CSV    | -                  | -                |
| silver.patients   | Delta  | country            | -                |
| silver.visits     | Delta  | department         | -                |
| gold.fact_treatments | Delta | -                  | treatment_type   |

---

## 📊 Aggregated Views (Gold Layer)

| View Name                      | Description                                                            | Key Dimensions                 | Key Metrics                                                             |
|-------------------------------|------------------------------------------------------------------------|-------------------------------|-------------------------------------------------------------------------|
| gold.department_treatment_summary | Treatments per department                                             | department                    | total_treatments, total_cost, avg_cost, patient count, doctor count    |
| gold.doctor_performance       | Revenue and activity per doctor                                       | doctor_id                     | total_cost, num_treatments, avg_cost, num_visits                       |
| gold.patient_summary          | Patient-wise activity and cost                                        | patient_id                    | total_visits, total_cost, num_treatments, follow_ups                   |
| gold.treatment_type_summary   | Cost per treatment type                                               | treatment_type                | total_cost, avg_cost, cost %                                           |
| gold.weekly_visit_trends      | Visit and treatment trends over time                                 | week                          | num_visits, patients, treatments, total_cost                           |
| gold.insurance_summary        | Insurance provider coverage                                           | insurance_provider            | total_cost, coverage_rate, avg_cost                                   |
| gold.follow_up_summary        | Department-level follow-up analysis                                   | department                    | follow_up_rate, count                                                  |
| gold.countrywise_cost_summary | Total cost and visits grouped by country                             | country                       | total_cost, avg_cost, visit count                                     |
| gold.revenue_by_age_group     | Treatment revenue by age brackets                                    | age group                     | total_cost, avg_cost                                                  |
| gold.top_10_expensive_patients| Top 10 patients with highest bills                                   | patient_id                    | total_cost, treatment_count                                           |
| gold.department_weekly_kpis   | Weekly KPIs per department                                            | department, week              | total_cost, avg_cost, treatment count                                 |
| gold.doctor_follow_up_ratio   | Doctor-wise follow-up recommendation rates                           | doctor_id                     | follow_up_rate, count                                                 |
| gold.treatment_distribution   | Distribution and cost range of treatments                            | treatment_type                | total, min, max, avg costs                                            |

---

## 🧠 Use Cases

- Monitor departmental treatment cost and performance
- Identify doctors with high or low patient load or follow-up rates
- Track insurance provider impact on coverage
- Time-based treatment trends for operational planning
- Patient cost analysis and segmentation by country or age group

---

## 🛠 Tools & Technologies

- Databricks (Delta Lake, Spark SQL)
- Python (for data generation)
- Markdown (for documentation)
- Jupyter/VSCode (development environment)

---

