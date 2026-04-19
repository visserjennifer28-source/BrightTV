-- ==========================================================
-- Retail sales dataset Session
-- SQL Fundamentals Assessment
-- ==========================================================

--Q1: Display all columns
SELECT *
FROM `workspace`.`default`.`retail_sales_dataset` 
LIMIT 10;

--Q2: Display specific columns
SELECT `Transaction ID`,
       `Date`,
       `Customer ID`
FROM workspace.default.retail_sales_dataset;

--Q3: Display unique product values
SELECT DISTINCT `Product Category`
FROM workspace.default.retail_sales_dataset;

--Q4: Get unique gender values
SELECT DISTINCT Gender
FROM workspace.default.retail_sales_dataset;

--Q5: Filter by age(customers older than 40)
SELECT *
FROM workspace.default.retail_sales_dataset
WHERE Age > 40;

--Q6: Filter by price range(where prices are between 100 and 500)
-- Used TRY_CAST because the column may be stored as text
Select *
FROM workspace.default.retail_sales_dataset
WHERE try_cast (`Price per Unit` AS DOUBLE) BETWEEN 100 AND 500;

--Q7: Filter by product category
SELECT *
FROM workspace.default.retail_sales_dataset
WHERE `Product Category` IN ('Beauty', 'Electronics');

--Q8
SELECT *
FROM workspace.default.retail_sales_dataset
WHERE `Product Category` != 'Clothing';

--Q9: Filter by quantity >=3
SELECT *
FROM workspace.default.retail_sales_dataset
WHERE Quantity >= 3;

--Q10: Count total transactions
SELECT
    COUNT (*) AS Total_Transactions
    FROM workspace.default.retail_sales_dataset;

--Q11: Calculate average customer age 
SELECT AVG(Age) AS Average_Age
FROM workspace.default.retail_sales_dataset;

--Q12: Calculate total quantity sold
SELECT SUM(Quantity) AS Total_Quantity
FROM workspace.default.retail_sales_dataset;

--Q13: Calculate maximum total amount
SELECT MAX(`Total Amount`) AS Max_Total_Amount
FROM workspace.default.retail_sales_dataset;

--Q14: Calculate minimum price per unit
SELECT MIN(`Price per Unit`) AS Min_Price_per_Unit
FROM workspace.default.retail_sales_dataset;

--Q15: Calculate average price per unit by product category
SELECT `Product Category`,
       AVG(`Price per Unit`) AS Average_Price
FROM workspace.default.retail_sales_dataset
GROUP BY `Product Category`;

--Q16: Calculate total revenue by gender
SELECT 
      Gender,
      SUM(`Total Amount`) AS Total_Revenue
FROM workspace.default.retail_sales_dataset
GROUP BY Gender;

--Q17: Calculate average price per unit by product category
SELECT 
        `Product Category`,
       AVG(try_cast(`Price per Unit` AS DOUBLE)) AS Average_Price
FROM workspace.default.retail_sales_dataset
GROUP BY `Product Category`;

--Q18: Calculate total revenue by product category where total revenue is greater than 10000
SELECT  `Product Category`,
       SUM(try_cast(`Total Amount` AS DOUBLE)) AS Total_Revenue
FROM workspace.default.retail_sales_dataset
GROUP BY `Product Category`
HAVING SUM(`Total Amount`) > 10000;

--Q19: Calculate average quantity by product category where average quantity is greater than 2
SELECT  `Product Category`,
       AVG(Quantity) AS Average_Quantity
FROM workspace.default.retail_sales_dataset
GROUP BY `Product Category`
HAVING AVG(Quantity) > 2;

--Q20: Categorise transactions into High/Low spending levels
SELECT 
      'Transaction ID',
      'Total Amount',
      CASE
          WHEN `Total Amount` > 1000 THEN 'High'
          ELSE 'Low'
      END AS Spending_Level
FROM workspace.default.retail_sales_dataset;

--Q21: Categorise customers into age groups
SELECT
    'Customer ID',
    Age,
    CASE
        WHEN Age < 30 THEN 'Youth'
        WHEN Age BETWEEN 30 AND 59 THEN 'Adult'
        ELSE 'Senior'
    END AS Age_Group
FROM workspace.default.retail_sales_dataset;
    
-- Count total transactions and their unique customers
SELECT
    'Customer ID',
    Age,
    CASE
        WHEN Age < 30 THEN 'Youth'
        WHEN Age BETWEEN 30 AND 59 THEN 'Adult'
        ELSE 'Senior'
    END AS Age_Group
FROM workspace.default.retail_sales_dataset;

-- Top transactions in Clothing categories
SELECT  u.`Customer ID`,
        u.`Total Amount`,
        u.`Age`,
        u.`Product Category`,
        u.`Quantity`,
        u.`Gender`,
        u.`Date`,
        u.`price per unit` AS `price per unit 2`,
        v.`Total Amount` AS `Total Amount 2`,
        v.`Age` AS `Age 2`,
        v.`Product Category` AS `Product Category 2`,
        v.`Quantity` AS `Quantity 2`,
        v.`Gender` AS `Gender 2`,
        v.`Date` AS `Date 2`
FROM workspace.default.retail_sales_dataset AS v 
LEFT JOIN workspace.default.retail_sales_dataset AS u
ON v.`Customer ID` = u.`Customer ID`;

SELECT COUNT(*) AS `Total Transactions`,
       COUNT(DISTINCT `Customer ID`) AS Unique_Customers
FROM workspace.default.retail_sales_dataset;

SELECT `Customer ID`,
       `Total Amount`,
       `Age`,
       `Product Category`,
       `Quantity`,
       `Gender`,
       `Date`
FROM workspace.default.retail_sales_dataset
WHERE `Product Category` = 'Clothing'
ORDER BY `Total Amount` DESC;

SELECT *
FROM workspace.default.retail_sales_dataset
WHERE TRY_CAST(`Total Amount` AS DOUBLE) IS NOT NULL;

SELECT *
FROM workspace.default.retail_sales_dataset
WHERE `Total Amount` IS NULL
OR `Price per Unit` IS NULL;

SELECT
    Age,
    CASE
        WHEN Age <30 THEN 'Youth'
        WHEN Age BETWEEN 30 AND 59 THEN 'Adult'
        ELSE 'Senior'
    END AS Age_Group,
    TRY_CAST(`Total Amount` AS DOUBLE) AS `Total Amount`
FROM workspace.default.retail_sales_dataset;

SELECT
    `Product Category`,
    COUNT(*) AS `Total Transactions`,
    SUM(TRY_CAST(`Total Amount` AS DOUBLE)) AS `Total Revenue`
FROM workspace.default.retail_sales_dataset
GROUP BY `Product Category`
ORDER BY `Total Revenue` DESC;

SELECT DATE AS trans_date, 
        YEAR(DATE) AS trans_year_value, --Extract the year value from our datetime column 
        MONTH(DATE) AS trans_month_value, --Extract the month value from our datetime column 
        MONTHNAME(DATE) AS trans_month_name, --Extract the month name from our datetime column
        DAY(DATE) AS trans_day_value, --Extract the day value from our datetime column
        DAYNAME(DATE) AS trans_day_name, --Extract the day name from our datetime column 
        `Customer ID`,
        Gender
FROM workspace.default.retail_sales_dataset
ORDER BY DATE ASC;

SELECT DATEADD(DAY,2,'2025-06-02') AS adding_days;

SELECT DATEADD(DAY,-2,'2025-06-02') AS subtracting_days;

SELECT DATE AS trans_date,
        TO_DATE(DATEADD(DAY,2,DATE)) AS adding_days,
        TO_CHAR(DATE,'YYYYMM') AS month_id
FROM workspace.default.retail_sales_dataset
ORDER BY DATE ASC;

SELECT datediff(YEAR,DATE'2020-06-02', DATE'2021-06-02') AS diff_in_years;

SELECT DATEDIFF('2026-06-02','2025-06-02');


-CREATE OR REPLACE TABLE clean_retail_sales AS 
SELECT
    CAST(`Price per Unit` AS DOUBLE) AS `price per unit`,
    *
FROM workspace.default.retail_sales_dataset;
