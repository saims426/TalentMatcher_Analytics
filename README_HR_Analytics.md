
# 📊 HR Analytics Dashboard Project

## 🔍 Overview

This project delivers a comprehensive HR analytics dashboard using **Power BI**, aimed at uncovering insights on employee attrition, compensation trends, career growth, and role recommendations. The workflow includes data cleaning with Power Query (M Language), modeling using DAX, and presenting findings through interactive dashboards.

---

## 📁 Project Files

| File | Description |
|------|-------------|
| [HR_Analytics.csv](https://github.com/saims426/TalentMatcher_Analytics/blob/fb9f259737fccdd494d87fcc08d0ceb734c1edc4/HR_Analytics.csv) | Final cleaned dataset |
| [teja_Dashboard.pbix](https://github.com/saims426/TalentMatcher_Analytics/blob/fb9f259737fccdd494d87fcc08d0ceb734c1edc4/teja_Dashboard.pbix) | Power BI dashboard file |

---

## ⚙️ Tools & Technologies

- Power BI Desktop
- Power Query (M Language)
- DAX (Data Analysis Expressions)
- Excel (used for preliminary data checks)

---

## 🧪 Data Cleaning & Transformation

### 🔹 Step 1: Column Type Conversion

```m
Table.TransformColumnTypes(#"Promoted Headers", {
    {"EmpID", type text}, {"Age", Int64.Type}, {"AgeGroup", type text}, {"Attrition", type text},
    {"BusinessTravel", type text}, {"DailyRate", Int64.Type}, {"Department", type text},
    {"DistanceFromHome", Int64.Type}, {"Education", Int64.Type}, {"EducationField", type text},
    {"EmployeeCount", Int64.Type}, {"EmployeeNumber", Int64.Type}, {"EnvironmentSatisfaction", Int64.Type},
    {"Gender", type text}, {"HourlyRate", Int64.Type}, {"JobInvolvement", Int64.Type},
    {"JobLevel", Int64.Type}, {"JobRole", type text}, {"JobSatisfaction", Int64.Type},
    {"MaritalStatus", type text}, {"MonthlyIncome", Int64.Type}, {"SalarySlab", type text},
    {"MonthlyRate", Int64.Type}, {"NumCompaniesWorked", Int64.Type}, {"Over18", type text},
    {"OverTime", type text}, {"PercentSalaryHike", Int64.Type}, {"PerformanceRating", Int64.Type},
    {"RelationshipSatisfaction", Int64.Type}, {"StandardHours", Int64.Type}, {"StockOptionLevel", Int64.Type},
    {"TotalWorkingYears", Int64.Type}, {"TrainingTimesLastYear", Int64.Type}, {"WorkLifeBalance", Int64.Type},
    {"YearsAtCompany", Int64.Type}, {"YearsInCurrentRole", Int64.Type}, {"YearsSinceLastPromotion", Int64.Type},
    {"YearsWithCurrManager", Int64.Type}
})
```

---

### 🔹 Step 2: Categorical Cleanup

```m
Table.ReplaceValue(#"Changed Type", "TravelRarely", "Travel_Rarely", Replacer.ReplaceText, {"BusinessTravel"})
```

---

### 🔹 Step 3: Null Value Handling

```m
Table.ReplaceValue(#"Replaced Value", null, 4, Replacer.ReplaceValue, {"YearsWithCurrManager"})
```

---

## 📐 DAX Measures & Calculated Columns

### 🔸 1. Attrition Rate

```DAX
Attrition Rate = 
DIVIDE(
    CALCULATE(COUNT('EmployeeData'[EmployeeNumber]), EmployeeData[Attrition] = "Yes"),
    [Total Employees]
)
```

### 🔸 2. Average Monthly Income by Role

```DAX
AvgMonthlyIncome = 
CALCULATE(
    AVERAGE(EmployeeData[MonthlyIncome]),
    ALLEXCEPT('EmployeeData', 'EmployeeData'[JobRole])
)
```

### 🔸 3. Estimated Monthly Revenue

```DAX
EstimatedMonthlyRev = 
DIVIDE([AvgMonthlyIncome] * COUNTROWS(EmployeeData) * 0.2, 1)
```

### 🔸 4. Hiring Rate

```DAX
Hiring Rate = 
DIVIDE(
    CALCULATE(COUNT(EmployeeData[EmployeeNumber]), EmployeeData[YearsAtCompany] <= 1),
    [Total Employees]
)
```

### 🔸 5. Yearly Salary

```DAX
Yearly Salary = 
EmployeeData[MonthlyIncome] * 12
```

### 🔸 6. Job Level Category

```DAX
Job Level Category = 
SWITCH(
    TRUE(),
    EmployeeData[JobLevel] = 1, "Entry Level",
    EmployeeData[JobLevel] = 2, "Junior Associate",
    EmployeeData[JobLevel] = 3, "Senior Associate",
    EmployeeData[JobLevel] = 4, "Manager",
    EmployeeData[JobLevel] = 5, "Director",
    "Other"
)
```

### 🔸 7. Skill Category

```DAX
Skill Category = 
SWITCH(
    TRUE(),
    EmployeeData[JobRole] = "Sales Executive", "Sales Strategy",
    EmployeeData[JobRole]= "Research Scientist", "Data Analysis",
    EmployeeData[JobRole] = "Laboratory Technician", "Laboratory Testing",
    EmployeeData[JobRole] = "Manufacturing Director", "Operations Management",
    EmployeeData[JobRole] = "Healthcare Representative", "Customer Relationship Management",
    EmployeeData[JobRole] = "Manager", "Leadership",
    EmployeeData[JobRole] = "Sales Representative", "Negotiation",
    EmployeeData[JobRole] = "Research Director", "Strategic Planning",
    EmployeeData[JobRole] = "Human Resources", "Talent Acquisition",
    "Other"
)
```

---

## 🧭 Dashboard Pages

### 1. Homepage
- KPIs: Attrition Rate, Hiring Rate, Avg Monthly Income
- Department & Gender distribution
- Slicers: Age Group, Department, Job Level

### 2. Workforce Trends
- Attrition analysis by Department, Gender, Age
- Hiring rate tracking (≤1 year at company)

### 3. Salary Benchmarks
- Income comparisons by Role, Department
- Salary slab distribution and yearly salary view

### 4. Career Progression
- Promotion trends and job level analysis
- Tenure and role stability metrics

### 5. Career Recommendations
- In-demand roles by skill
- Job Role to Skill Category mapping

---

## 📈 Key Insights

- Entry-level attrition is highest in Sales & R&D roles
- Salary imbalances visible across same job levels
- Promotions typically happen after 3+ years in-role
- Skill-based recommendations can improve hiring outcomes

---

## 📬 Contact

For queries or feedback, connect on [LinkedIn](https://www.linkedin.com/in/aishwarya-baluri/) or open an issue on this repository.

---
