# 👥 HR Attrition Analysis Dashboard

![SQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Data Analysis](https://img.shields.io/badge/Data%20Analysis-0078D4?style=for-the-badge&logo=databricks&logoColor=white)

---

## 📌 Project Overview

This project analyzes the IBM HR Analytics dataset to identify key drivers of employee attrition across departments, job roles, salary bands, age groups, and tenure. SQL was used for data querying and pattern identification, while Power BI was used to build an interactive dashboard for HR stakeholders to support data-driven retention strategies.

---

## 🎯 Objectives

- Identify **attrition patterns** across departments, job roles, and salary bands
- Analyze the impact of **job satisfaction, age, and tenure** on attrition
- Build an **interactive Power BI dashboard** for HR teams
- Deliver **actionable recommendations** to reduce employee turnover

---

## 🗂️ Dataset

| Detail | Info |
|--------|------|
| **Source** | [Kaggle — IBM HR Analytics Dataset](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) |
| **Records** | 1,470 employees |
| **Fields** | Age, Attrition, Department, JobRole, MonthlyIncome, JobSatisfaction, YearsAtCompany, Gender, and 30+ more |
| **Target Variable** | Attrition (Yes/No) |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **MySQL** | Data import, querying, attrition pattern analysis |
| **Power BI Desktop** | Dashboard development, DAX measures, KPI cards |

---

## 📊 Project Workflow

### Step 1 — Database Setup (MySQL)
```sql
CREATE DATABASE hr_analysis;
USE hr_analysis;
```
Imported CSV using MySQL Workbench Table Data Import Wizard.

### Step 2 — SQL Analysis Queries

**Overall Attrition Rate:**
```sql
SELECT 
    COUNT(*) AS Total_Employees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS Attrited,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM hr_employee_attrition;
```

**Attrition by Department:**
```sql
SELECT 
    Department,
    COUNT(*) AS Total_Employees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS Attrited,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM hr_employee_attrition
GROUP BY Department
ORDER BY Attrition_Rate DESC;
```

**Attrition by Salary Band:**
```sql
SELECT 
    CASE 
        WHEN MonthlyIncome < 3000 THEN 'Low (Below 3K)'
        WHEN MonthlyIncome BETWEEN 3000 AND 6000 THEN 'Medium (3K-6K)'
        WHEN MonthlyIncome BETWEEN 6001 AND 10000 THEN 'High (6K-10K)'
        ELSE 'Very High (Above 10K)'
    END AS Salary_Band,
    COUNT(*) AS Total,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS Attrited,
    ROUND(SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS Attrition_Rate
FROM hr_employee_attrition
GROUP BY Salary_Band
ORDER BY Attrition_Rate DESC;
```

### Step 3 — DAX Measures (Power BI)

```
Total Employees = COUNT('HR'[EmployeeNumber])

Total Attrition = CALCULATE(COUNT('HR'[Attrition]), 'HR'[Attrition] = "Yes")

Attrition Rate = 
DIVIDE(
    CALCULATE(COUNT('HR'[Attrition]), 'HR'[Attrition] = "Yes"),
    COUNT('HR'[Attrition]), 0
) * 100
```

### Step 4 — Power BI Dashboard
Built the following visuals:
- 📊 **3 KPI Cards** — Total Employees, Total Attrition, Attrition Rate %
- 📊 **Bar Chart** — Attrition by Department
- 📊 **Bar Chart** — Attrition by Job Role
- 🍩 **Donut Chart** — Attrition by Age Group
- 📈 **Line Chart** — Attrition by Years at Company
- 🔲 **Slicers** — Department, Gender, Job Satisfaction

---

## 📈 Key Findings

| Insight | Finding |
|---------|---------|
| **Total Employees** | 1,470 |
| **Total Attrition** | 237 employees |
| **Overall Attrition Rate** | 16.12% |
| **Highest Attrition Dept** | Sales — 38.8% attrition rate |
| **Highest Attrition Role** | Sales Representative — 39.8% |
| **Highest Risk Age Group** | Below 25 years |
| **Highest Risk Tenure** | 0–1 years at company |
| **Salary Impact** | Low income (Below $3K) — highest attrition |
| **Satisfaction Impact** | Low job satisfaction employees 4x more likely to leave |

---

## 💡 Business Recommendations

1. **Focus retention on Sales department** — attrition rate of 38.8% is significantly above company average of 16.12%
2. **Increase salaries for low-income employees** — below $3K salary band shows highest attrition; salary revision could reduce turnover
3. **Implement onboarding programs** — employees in their first year are most likely to leave; structured onboarding reduces early attrition
4. **Address job satisfaction** — low satisfaction employees are 4x more likely to leave; conduct quarterly satisfaction surveys
5. **Create growth paths for young employees** — employees below 25 show highest attrition; mentorship and career development programs can help retain them

---

## 🖼️ Dashboard Preview

> *(Add screenshot of your Power BI dashboard here after completion)*
>
> To add: Save dashboard as image in Power BI → `File > Export > Export to Image`

---

## 📁 Project Structure

```
HR-Attrition-Analysis/
│
├── data/
│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv    # Raw dataset
│
├── sql/
│   └── hr_attrition_queries.sql                  # All SQL queries
│
├── dashboard/
│   └── HR_Attrition_Dashboard.pbix               # Power BI file
│
├── screenshots/
│   └── dashboard_preview.png                     # Dashboard screenshot
│
└── README.md                                     # Project documentation
```

---

## 🚀 How to Run This Project

1. **Download dataset** from [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset)
2. **Import into MySQL** using Table Data Import Wizard
3. **Run SQL queries** from `hr_attrition_queries.sql`
4. **Open** `HR_Attrition_Dashboard.pbix` in Power BI Desktop
5. **Refresh data** → `Home > Refresh`
6. Use **slicers** to filter by Department, Gender, Job Satisfaction

---

## 🎓 Skills Demonstrated

- ✅ SQL Querying (JOINs, GROUP BY, CASE WHEN, Aggregations)
- ✅ Descriptive & Diagnostic Analysis
- ✅ DAX Measures in Power BI
- ✅ KPI Dashboard Development
- ✅ Data Visualization & Storytelling
- ✅ HR Domain Knowledge
- ✅ Business Insight Communication

---

## 👤 Author

**Shubham Pandey**
Certified Data Analyst | PwC × NSDC
📧 shubhp71@gmail.com
📍 Satna, Madhya Pradesh, India

---
