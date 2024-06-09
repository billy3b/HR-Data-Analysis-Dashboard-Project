# HR-Data-Analysis-Dashboard-Project
This project aims to provide insights into employee absenteeism, compensation, and health metrics using SQL and Power BI. We have consolidated data from various sources and used SQL to analyze and prepare the data for visualization in Power BI. The ultimate goal is to create a dashboard that helps HR understand absenteeism patterns and reward healthy employees.

## Objectives
- Identify healthy employees with low absenteeism for a bonus program.
- Calculate wage increases for non-smokers within a specific insurance budget.
- Create an interactive dashboard in Power BI to visualize absenteeism data.

## Data Sources
The project uses three datasets:

1. Absenteeism_at_work: Contains records of absenteeism details.
2. Compensation: Provides compensation information for employees.
3. Reasons: Lists reasons for absenteeism.
These datasets were combined using SQL and imported into Power BI for further analysis and dashboard creation

## SQL Queries and Analysis
### Healthy Employees for Bonus
To identify the healthiest employees eligible for a bonus, we used the following SQL query:
```
SELECT * FROM Absenteeism_at_work 
WHERE Social_drinker = 0 
AND Social_smoker = 0 
AND Body_mass_index < 25 
AND Absenteeism_time_in_hours < (SELECT AVG(Absenteeism_time_in_hours) FROM Absenteeism_at_work);
```
### Wage Increase for Non-Smokers
To calculate the wage increase for non-smokers within the insurance budget of $983,221, we used:
```
SELECT COUNT(*) AS nonsmokers FROM Absenteeism_at_work
WHERE Social_smoker = 0;
```
- There are 686 non-smokers. If they work 8 hours a day,5 day a week, for a year. Then
5 * 8 * 52 =2080 hours a year.
- 686 employee works 2080 * 686 = 1426880 hours
- 983221/1426880 = 0.68 cents/hour increase in income.
- For 1 year increase 0.68 * 2080 = $1414.4

### SQL Query for Power BI Integration
We optimized the data query for Power BI visualization:
```
SELECT 
    a.ID,
    r.Reason,
    Body_mass_index,
    CASE 
        WHEN Body_mass_index < 18.5 THEN 'Underweight'
        WHEN Body_mass_index BETWEEN 18.5 AND 25 THEN 'Healthy Weight'
        WHEN Body_mass_index BETWEEN 25 AND 30 THEN 'Overweight'
        WHEN Body_mass_index > 30 THEN 'Obese'
        ELSE 'Unknown' 
    END AS BMI_Category,
    CASE 
        WHEN Month_of_absence IN (12, 1, 2) THEN 'Winter'
        WHEN Month_of_absence IN (3, 4, 5) THEN 'Spring'
        WHEN Month_of_absence IN (6, 7, 8) THEN 'Summer'
        WHEN Month_of_absence IN (9, 10, 11) THEN 'Fall'
        ELSE 'Unknown' 
    END AS Season_Names,
    Seasons,
    Month_of_absence,
    Day_of_the_week,
    Transportation_expense,
    Education,
    Son,
    Social_drinker,
    Social_smoker,
    Pet,
    Disciplinary_failure,
    Age,
    Work_load_Average_day,
    Absenteeism_time_in_hours
FROM Absenteeism_at_work a
LEFT JOIN compensation b ON a.ID = b.ID
LEFT JOIN Reasons r ON a.Reason_for_absence = r.Number;
```
## PowerBI Dashboard
![image](https://github.com/billy3b/HR-Data-Analysis-Dashboard-Project/assets/108816279/deaf3d48-f7a3-44b5-926c-334da729911b)

