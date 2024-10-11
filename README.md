# Absenteeism-HR-End-To-End-Project

## Table of Contents
- [ETL Process Overview](#etl-process-overview)
- [Key SQL Queries](#key-sql-queries)
  - [Create a Join Table](#create-a-join-table)
  - [Identify the Healthiest Employees](#identify-the-healthiest-employees)
  - [Compensation for Non-Smokers](#compensation-for-non-smokers)
  - [Optimized Query for Data Transformation](#optimized-query-for-data-transformation)
- [Data Loading into Power BI](#data-loading-into-power-bi)
- [Dashboard Creation](#dashboard-creation)

---

## ETL Process Overview

### Extract
The data is sourced from several tables in SQL Server, including `Absenteeism_at_work`, `compensation`, and `Reasons`. These tables contain detailed information about employees, their absenteeism records, compensation, and the reasons for their absences.

### Transform
Data is cleaned and transformed using SQL queries to merge relevant information, categorize employees based on health and absenteeism metrics, and prepare it for loading into Power BI. The key transformations include joining tables, filtering the healthiest employees, and calculating compensation adjustments.

### Load
The transformed data is then loaded into Power BI, where we create various visualizations to analyze absenteeism patterns, health metrics, and financial impacts on the organization.

---

## Key SQL Queries

### 1. Create a Join Table
This query joins the tables `Absenteeism_at_work`, `compensation`, and `Reasons` to create a unified dataset with information on employees, absenteeism causes, and compensation details.

```sql
SELECT * 
FROM Absenteeism_at_work a
LEFT JOIN compensation b ON a.ID = b.ID
LEFT JOIN Reasons r ON a.Reason_for_absence = r.Number;
