<img width="1890" height="742" alt="image" src="https://github.com/user-attachments/assets/d996e4f9-3092-42e8-af95-9265b62929b6" />


# HR_Analytics
## Creation of data base
create database hranalytics;
## Entering into database
use hranalytics;
## Retriving all the data 
select * from hrdataan;
## Counting of all rows in the table
select count(*) from hrdataan;

## 1 Find the department wise average attrition rate

select Department, round(avg(numofyes) * 100, 2) as Average_attrition_percentage
from hrdataan
group by department;

## 2 Find the average hourly rate of male research scientist

select JobRole, Gender , avg(hourlyrate)  as average_hourly_rate from hrdataan
where JobRole = "Research Scientist" and Gender = "Male"
group by JobRole;

## 3 Find the monthly income wise attrition rate 

select monthlyincomerange, round(avg(numofyes) * 100, 2) AS average_attrition_percentage 
from hrdataan
group by monthlyincomerange
order by average_attrition_percentage desc;

#4 Find the average working years for each department

select Department, avg(TotalWorkingYears) as averageWorkingYears
from hrdataan
group by Department
order by averageWorkingYears asc; 

## 5 Find the average attrition rate for year since last promotion
select RANGEOFYEARS, ROUND(AVG(numofyes) * 100, 2) AS average_attrition_percentage 
from hrdataan
group by RANGEOFYEARS
order by average_attrition_percentage asc;

## 6 Find the jobrole vs worklifebalance

select JobRole, sum(WorkLifeBalance) AS WorkLifeBalance
from hrdataan
group by JobRole
order by  WorkLifeBalance asc;

## 7 Find the count of employee based on educational fields

select EducationField, sum(EmployeeCount) AS Totalemployee
from hrdataan
group by EducationField
order by Totalemployee asc;

## 8 Average age by department
SELECT Department, ROUND(AVG(Age), 1) AS AverageAge
FROM hrdataan
GROUP BY Department;

## 9 Employee Count by Job Role and Gender
SELECT JobRole, Gender, COUNT(*) AS EmployeeCount
FROM hrdataan
GROUP BY JobRole, Gender
ORDER BY JobRole, Gender;


## 10 Top 5 Highest Paid Job Roles (Based on Monthly Income)
SELECT JobRole, ROUND(AVG(MonthlyIncome), 2) AS AverageMonthlyIncome
FROM hrdataan
GROUP BY JobRole
ORDER BY AverageMonthlyIncome DESC
LIMIT 5;


## 11 Attrition Rate by Education Field
SELECT EducationField,
       CONCAT(ROUND(AVG(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100, 2), '%') AS AttritionRate
FROM hrdataan
GROUP BY EducationField;

## 12 Percentage of Employees with Overtime
SELECT CONCAT(ROUND(SUM(CASE WHEN OverTime = 'Yes' THEN 1 ELSE 0 END)*100.0 / COUNT(*), 2), '%') AS OvertimePercentage
FROM hrdataan;

## 13 Employees with Above-Average Monthly Income
SELECT EmployeeNumber,MonthlyIncome
FROM hrdataan
WHERE MonthlyIncome > (SELECT AVG(MonthlyIncome)
    FROM hrdataan);
    
## 14 Rank Employees by Income Within Each Department  
SELECT EmployeeNumber, Department, MonthlyIncome,
       RANK() OVER (PARTITION BY Department ORDER BY MonthlyIncome DESC) AS IncomeRank
FROM hrdataan;

## 15 Employees with Above-Average Monthly Income (Monthly Income)
  SELECT EmployeeNumber, MonthlyIncome
FROM hrdataan
WHERE MonthlyIncome > (
    SELECT AVG(MonthlyIncome)
    FROM hrdataan);

## 16 Average Monthly Income by Job Role and Gender
SELECT JobRole, Gender, ROUND(AVG(MonthlyIncome), 2) AS AvgIncome
FROM hrdataan
GROUP BY JobRole, Gender;

## 17 Percentage of Attrition by Job Role
SELECT JobRole,
       CONCAT(ROUND(AVG(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) * 100, 2), '%') AS AttritionRate
FROM hrdataan
GROUP BY JobRole;

## 18 Departments with Above-Average Attrition Rate (Subquery)
SELECT Department
FROM (
    SELECT Department,
           AVG(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS AttrRate
    FROM hrdataan
    GROUP BY Department
) AS DeptAttrition
WHERE AttrRate > (
    SELECT AVG(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END)
    FROM hrdataan);


## 19 Top 3 Highest-Paid Employees per Department (CTE)
WITH RankedIncomes AS (
    SELECT EmployeeNumber, Department, MonthlyIncome,
           DENSE_RANK() OVER (PARTITION BY Department ORDER BY MonthlyIncome DESC) AS rnk
    FROM hrdataan
)
SELECT EmployeeNumber, Department, MonthlyIncome
FROM RankedIncomes
WHERE rnk <= 3;

## 20 Employees With Income Below Department Average
SELECT a.EmployeeNumber, a.Department, a.MonthlyIncome
FROM hrdataan a
JOIN (
    SELECT Department, AVG(MonthlyIncome) AS dept_avg
    FROM hrdataan
    GROUP BY Department
) b ON a.Department = b.Department
WHERE a.MonthlyIncome < b.dept_avg;

## 21 DEPARTMENT WISE NO. OF EMPLOYEES
SELECT DEPARTMENT, SUM(EMPLOYEECOUNT) AS EMP_COUNT
FROM hrdataan
GROUP BY DEPARTMENT;


## 22 GENDER BASED PERCENTAGE OF EMPLOYEE
select gender,format((count(gender) * 100.0 / (select count(*) from hr_analytics)),2)
as 'employee %'
from hrdataan
group by gender;

## 23 DEPARTMENT and JOBROLE WISE JOB SATISFACTION
SELECT JOBROLE,AVG(JOBSATISFACTION) AS AVERAGEJOBSATISFACTION
FROM hrdataan
GROUP BY JOBROLE;

## 24 ATTRITION BY MARITAL STATUS
SELECT MARITALSTATUS,GENDER,
CONCAT(FORMAT((COUNT(CASE WHEN ATTRITION='YES' THEN 1 END)*100.0 / COUNT(*)),2),'%')
AS ATTRITIONRATE
FROM hrdataan
GROUP BY MaritalStatus, GENDER;



   

