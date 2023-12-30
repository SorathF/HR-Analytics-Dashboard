# HR-Analytics-Dashboard
## Introduction
This project is a part of the “Data Storytelling with Power BI” course. 
## Tool
The tool used for analysis and visualizing was Power BI desktop. 

## Prepare
The instructor provided the dataset in an Excel file. The workbook contains three worksheets: AgeGroup, BU, Date, Employee, FP, Gender and Paytype. Details of the dataset are as follows:

- AgeGroup- AgeGroupID, AgeGroup

-	BU- BU, RegionSeq

-	Date- Date, Month, MonthNumber, Period, PeriodNumber, Qtr, QtrNumber, Year, Day, MonthStartDate, MonthEndDate

- Employee- Date, EmplID, Gender, Age, FP, TermDate, BU, HireDate, PayTypeID

- FP- FP, Job type

- Gender- ID, Gender

- Paytype- PayTypeID, PayType

After downloading the dataset, I imported it to Power BI. 
 
## Process
The dataset was loaded in the Power BI Query editor for data transformation. The process phase includes the following steps:
-	Set first rows as header (wherever applicable)

-	Checked for duplicate values in EmplID- none found.

-	Made the following replacements Gender column of employee:

    1. Replaced C with Female

    2. Replaced D with Male

Once all required cleaning tasks were performed, the dataset was loaded into the Power BI desktop. The following additions were made using DAX. 
- Addition of Region column in the BU table

       Region =mid([RegionSeq], 3,10)

- Addition of Age Group ID, New Hire, and Tenure Days columns in the Employee table

        Age Group ID = IF(Employee[Age] < 30, 1, IF(Employee[Age] < 50, 2, 3))

       New Hire = IF(YEAR(Employee[date]) = YEAR(Employee[HireDate]) && MONTH(Employee[date]) = MONTH(Employee[HireDate]), 1)

       Tenure Days = IF(Employee[date] - Employee[HireDate] < 0, Employee[HireDate] - Employee[date], Employee[date] - Employee[HireDate])
## Analysis
Now the data was cleaned and transformed, it was time for analysis. Before that, necessary relationships were established. 

![Screenshot (348)](https://github.com/SorathF/HR-Analytics-Dashboard/assets/154694595/0caff709-550a-45d2-9127-10d9713416b7)


A new measure table was created for the following measures:

    Employee Count = CALCULATE(COUNT(Employee[EmplID]), FILTER(ALL('Date'[PeriodNumber]), 'Date'[PeriodNumber] = MAX('Date'[PeriodNumber]))

     Active Employees = CALCULATE([Employee Count], FILTER(Employee, ISBLANK(Employee[TermDate])))


    New Hires = SUM(Employee[New Hire])

    Separations = CALCULATE(COUNT(Employee[EmplID]), FILTER(Employee, NOT(ISBLANK(Employee[TermDate])))) 

    Average Tenure Days = AVERAGE(Employee[Tenure])

    Average Tenure Months = ROUND(AVERAGE(Employee[Tenure])/30, 1) - 1

## Visualization
Finally, I created the HR analytics dashboard using Power BI. 

![Dashboard](https://github.com/SorathF/HR-Analytics-Dashboard/assets/154694595/17f44842-54e9-4dcc-a627-a03dbb964212)
