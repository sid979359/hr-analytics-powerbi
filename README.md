# hr-analytics-powerbi
Power BI HR analytics project using DAX to analyze employee tenure, department ranking, and performance comparison.

# HR Analytics using Power BI (DAX Measures)

## 📊 Project Overview
This project focuses on **Human Resources Analytics** using **Power BI and DAX**.  
The objective is to analyze employee tenure, rank departments based on tenure, and compare department performance against the overall company average.

This project demonstrates **clear DAX thinking**, proper use of **row vs filter context**, and business-oriented analytics.

---

## 📂 Dataset
The project uses a single CSV file:

- `EmployeeData.csv`

### Key Columns:
- EmployeeID  
- Department  
- StartDate  
- EndDate  
- PerformanceRating  

---

## 🎯 Business Objectives
The report answers the following HR questions:

1. What is the **average tenure of employees in each department**?
2. How do departments **rank based on average tenure**?
3. How does each department’s **average performance compare to the overall company average**?

---

## 🛠 Tools & Technologies
- Power BI Desktop  
- Power Query  
- DAX (Data Analysis Expressions)  

---

## 🧠 Problem-Solving Approach
Before writing DAX, the following steps were considered:

- Identify the **level of calculation** (employee vs department)
- Decide between **calculated column vs measure**
- Handle **current employees** using EndDate logic
- Control **filter context** using `CALCULATE`, `ALL`, and `ALLEXCEPT`
- Use **iterators** (`AVERAGEX`, `RANKX`) where row-level calculation was required

---

## 📐 DAX Measures Created

### 1️⃣ Average Tenure per Department
- Calculates tenure for each employee using `DATEDIFF`
- Averages tenure at the department level
- Includes only current employees


Average Tenure per Department =
CALCULATE(
    AVERAGEX(
        EmployeeData,
        DATEDIFF(
            EmployeeData[StartDate],
            TODAY(),
            YEAR
        )
    ),
    ALLEXCEPT(EmployeeData, EmployeeData[Department]),
    ISBLANK(EmployeeData[EndDate])
)

##2️⃣ Department Tenure Rank

Ranks all departments based on average tenure

Higher tenure receives a higher rank

Dense ranking is used
Department Tenure Rank =
RANKX(
    ALL(EmployeeData[Department]),
    [Average Tenure per Department],
    ,
    DESC,
    Dense
)

3️⃣ Department vs Company Performance

Calculates department average performance

Compares it against the overall company average

Positive value indicates above-average performance
Dept vs Company Performance =
CALCULATE(
    AVERAGE(EmployeeData[PerformanceRating]),
    ALLEXCEPT(EmployeeData, EmployeeData[Department])
)
-
[Overall Company performance Avg]

