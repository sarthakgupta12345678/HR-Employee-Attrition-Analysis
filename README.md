## HR Operations / Attrition Analytics

This project analyzes employee attrition (turnover) for an HR/Operations team using:

    - **Excel** for KPIs, pivot tables, and an interactive dashboard  
    - **MySQL** for SQL-based analysis  
    - **BA-style documentation** of findings and recommendations  
The dataset is the well-known **IBM HR Analytics Employee Attrition** dataset (publicly available).

---

## 🎯 Business Question

* The company is experiencing high employee attrition and wants to understand:
    - Which departments and roles are losing people the fastest?  
    - Which age and tenure segments are at the highest risk?  
    - What actions HR can take to reduce attrition?

---

## 📂 Project Structure

```
HR Operations analytics/
├── data/
│   └── hr_attrition_raw.csv                                                       # Original HR attrition dataset
│
├── excel/
│   ├── hr_attrition.xlsx                                                          # Working file with pivots & helper columns
│   │
│   └── hr_attrition_dashboard.xlsx                                                # Final dashboard (clean presentation)
├── docs/
│   └── ba_report.md                                                               # Business Analyst style report
│
└── sql/
     └── hr_attrition_mysql.sql                                                    # MySQL queries used for analysis
```

---

## 📊 Excel Analysis & Dashboard

Steps in Excel:
* Loaded the raw CSV and saved it as "hr_attrition.xlsx".
* Created helper fields:
    - AgeBand (groups ages into <25, 25–34, 35–44, 45–54, 55+)
    - AttritionFlag (1 if Attrition = "Yes", else 0)
* Built PivotTables to analyze attrition:
    - By Department
    - By Job Role
    - By Age Band
    - By YearsAtCompany
* Built an interactive dashboard in hr_attrition_dashboard.xlsx with:
    * KPIs:
        - Total Employees
        - Attrition Count
        - Attrition Rate (%)
    * Charts:
        - Attrition by Department (bar chart)
        - Attrition by Age Band (column chart)
        - Attrition by Years at Company (line chart)
    * Slicers for:
        - Department
        - Gender
        - Marital Status
      
To view the dashboard, open "excel/hr_attrition_dashboard.xlsx" and go to the Dashboard sheet.

![HR Attrition Dashboard](hr_attrition_dashboard.png)

---

## 🧮 SQL / MySQL Analysis

The file "sql/hr_attrition_mysql.sql" contains the main queries used to reproduce the Excel pivots in MySQL.
Key queries include:

* Attrition rate by department
  
        SELECT 
            Department,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS attrition_count,
            COUNT(*) AS total_employees,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*) AS attrition_rate
        FROM employees
        GROUP BY Department
        ORDER BY attrition_rate DESC;

* Attrition rate by job role
  
        SELECT 
            JobRole,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS attrition_count,
            COUNT(*) AS total_employees,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*) AS attrition_rate
        FROM employees
        GROUP BY JobRole
        ORDER BY attrition_rate DESC;

* Attrition rate by age band (same logic as Excel AgeBand field)

        SELECT
            CASE 
                WHEN Age < 25 THEN '<25'
                WHEN Age BETWEEN 25 AND 34 THEN '25-34'
                WHEN Age BETWEEN 35 AND 44 THEN '35-44'
                WHEN Age BETWEEN 45 AND 54 THEN '45-54'
                ELSE '55+'
            END AS age_band,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS attrition_count,
            COUNT(*) AS total_employees,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*) AS attrition_rate
        FROM employees
        GROUP BY age_band
        ORDER BY attrition_rate DESC;

* Attrition rate by years at the company
  
        SELECT
            YearsAtCompany,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS attrition_count,
            COUNT(*) AS total_employees,
            SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*) AS attrition_rate
        FROM employees
        GROUP BY YearsAtCompany
        ORDER BY YearsAtCompany ASC;

---

## 🔍 Key Insights (from dashboard & SQL)

Some of the main findings (using this dataset):
* Overall attrition is around 16
* Sales and HR departments have higher attrition rates than R&D.
* Sales Representatives and Laboratory Technicians are among the highest-risk roles.
* Attrition is highest for employees under 25 and within their first 0–3 years at the company.
* Senior roles (Directors, Managers) have relatively low attrition.

These insights feed into the recommendations documented in docs/ba_report.md.

---

## 🧾 BA-Style Report
The file "docs/ba_report.md" summarizes the project like a Business Analyst would:
* Problem statement
* Methods (how attrition rate was calculated, what segments were analyzed)
* Key findings
* Actionable recommendations, such as:
    - Improving onboarding and early-tenure experience (0–3 years)
    - Targeted retention for Sales & specific high-risk roles
    - Monitoring demographic patterns (Gender, Marital Status) using slicers

---

## 🛠️ Tools & Skills Used
* Excel
    - PivotTables
    - PivotCharts
    - Slicers & KPIs
    - Conditional formatting
* SQL / MySQL
    - GROUP BY, CASE WHEN, aggregate functions
    - Calculated attrition metrics by segment
* Git & GitHub
    - Version control for analysis assets
* Business Analysis
    - Framing a problem statement  
    - Translating data insights into recommendations

---

## 🚀 How to Reproduce
* Clone the repo:
        https://github.com/sarthakgupta12345678/HR-Employee-Attrition-Analysis
* Open "excel/hr_attrition_dashboard.xlsx" in Excel to explore the dashboard.
* To run the SQL:
    - Import "data/hr_attrition_raw.csv" into a MySQL table called "employees" in a database called "hr_operations".
    - Run "sql/hr_attrition_mysql.sql" in MySQL Workbench.

---

## ✍️ Author

Sarthak Gupta.

Business / Data Analyst – Python, SQL, powerBI, Excel

