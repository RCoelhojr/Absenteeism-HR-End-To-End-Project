# Absenteeism-HR-End-To-End-Project


# ETL and Power BI Dashboard Creation

This repository outlines the process of extracting, transforming, and loading (ETL) data from a SQL Server database into Power BI for dashboard creation. Below, we describe the steps and key SQL queries used in the ETL process, focusing on employee absenteeism data. The final product is a dashboard that analyzes various factors influencing absenteeism and employee well-being.
#Table of Contents

###ETL Process Overview
###Key SQL Queries
###Create a Join Table
###Identify the Healthiest Employees
###Compensation for Non-Smokers
###Optimized Query for Data Transformation
###Data Loading into Power BI
###Dashboard Creation

#Extract
The data is sourced from several tables in SQL Server, including Absenteeism_at_work, compensation, and Reasons. These tables contain detailed information about employees, their absenteeism records, compensation, and the reasons for their absences.

#Transform
Data is cleaned and transformed using SQL queries to merge relevant information, categorize employees based on health and absenteeism metrics, and prepare it for loading into Power BI. The key transformations include joining tables, filtering the healthiest employees, and calculating compensation adjustments.

#Load
The transformed data is then loaded into Power BI, where we create various visualizations to analyze absenteeism patterns, health metrics, and financial impacts on the organization.

#Key SQL Queries
1. Create a Join Table
This query joins the tables Absenteeism_at_work, compensation, and Reasons to create a unified dataset with information on employees, absenteeism causes, and compensation details.


SELECT * 
FROM Absenteeism_at_work a
LEFT JOIN compensation b ON a.ID = b.ID
LEFT JOIN Reasons r ON a.Reason_for_absence = r.Number;

2. Identify the Healthiest Employees
To find the healthiest employees, we filter based on several criteria:

Non-drinkers (Social_drinker = 0)
Non-smokers (Social_smoker = 0)
BMI (Body Mass Index) below 25, indicating a healthy weight
Below-average absenteeism time

SELECT * 
FROM Absenteeism_at_work
WHERE Social_drinker = 0 
  AND Social_smoker = 0
  AND Body_mass_index < 25
  AND Absenteeism_time_in_hours < (SELECT AVG(Absenteeism_time_in_hours) FROM Absenteeism_at_work);


3. Compensation for Non-Smokers
To plan for a compensation increase for non-smokers, we calculate the number of non-smokers, which is used for financial budgeting. Based on the total budget and the planned compensation increase per hour, we calculate the yearly cost.


-- Count the number of non-smokers
SELECT COUNT(*) 
FROM Absenteeism_at_work
WHERE Social_smoker = 0;

-- The budget for non-smoker compensation is 983,221, allowing a 0.68 per hour increase, or 1,414.4 per year.

4. Optimized Query for Data Transformation
This query optimizes the dataset by categorizing the body mass index (BMI), mapping months to seasons, and classifying education levels.


SELECT
    a.ID,
    r.Reason,
    Month_of_absence,
    Body_mass_index,
    CASE 
        WHEN Body_mass_index < 18.8 THEN 'Underweight'
        WHEN Body_mass_index BETWEEN 18.8 AND 25 THEN 'Healthy'
        WHEN Body_mass_index BETWEEN 25 AND 30 THEN 'Overweight'
        WHEN Body_mass_index > 30 THEN 'Obese'
        ELSE 'Unknown'
    END AS BMI_category,
    CASE 
        WHEN Month_of_absence IN (12, 1, 2) THEN 'Winter'
        WHEN Month_of_absence IN (3, 4, 5) THEN 'Spring'
        WHEN Month_of_absence IN (6, 7, 8) THEN 'Summer'
        WHEN Month_of_absence IN (9, 10, 11) THEN 'Fall'
        ELSE 'Unknown'
    END AS Seasons_names,
    CASE 
        WHEN Education IN (1) THEN 'Bachelors'
        WHEN Education IN (2) THEN 'High School Graduate'
        WHEN Education IN (3) THEN 'Masters'
        WHEN Education IN (4) THEN 'Doctorate or above'
        ELSE 'Unknown'
    END AS Education_levels,
    Day_of_the_week,
    Transportation_expense,
    Social_drinker,
    Social_smoker,
    Pet,
    Work_load_Average_day,
    Absenteeism_time_in_hours
FROM Absenteeism_at_work a
LEFT JOIN compensation b ON a.ID = b.ID
LEFT JOIN Reasons r ON a.Reason_for_absence = r.Number;

#Data Loading into Power BI
Once the data is prepared using the SQL queries above, the next step is to load it into Power BI:
Connect Power BI to the SQL Server database.
Import the transformed data.
Perform any additional transformations or calculations required for visualization.

#Dashboard Creation
After loading the data, we can create various visuals in Power BI to help stakeholders understand absenteeism trends and employee well-being. Some suggested visuals include:

Bar charts showing absenteeism reasons by season.
Scatter plots to explore the relationship between absenteeism time and transportation expenses.
Pie charts illustrating the distribution of employees by BMI category or education level.
KPI visuals to display key metrics such as the average absenteeism time or the budget impact of compensation increases for non-smokers.
Conclusion
This project outlines the full ETL process, from data extraction and transformation in SQL Server to dashboard creation in Power BI. The data is used to analyze absenteeism, employee health, and compensation strategies, providing actionable insights for organizational decision-making.

