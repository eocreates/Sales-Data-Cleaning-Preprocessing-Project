/*
===========================================================
   Sales Data Cleaning & Preprocessing Project
===========================================================

   Author: [Your Name]
   Database: [Your Database Name]
   Description:
      This SQL script cleans and preprocesses a raw sales dataset
      to make it ready for reporting and analytics.

      It includes:
      - Removing duplicates
      - Handling missing values
      - Standardizing column names
      - Extracting useful date-based columns
      - Creating reference columns
===========================================================
*/

-----------------------------------------------------------
-- 1️⃣ View the raw dataset
-----------------------------------------------------------
SELECT *
FROM raw_sales_data
LIMIT 10;

-----------------------------------------------------------
-- 2️⃣ Standardize Column Names
-----------------------------------------------------------
-- Rename columns for consistency and readability
-- (This part may vary depending on your SQL dialect)
ALTER TABLE raw_sales_data
RENAME COLUMN "Cust ID" TO Customer_ID;

ALTER TABLE raw_sales_data
RENAME COLUMN "Order ID" TO Order_ID;

ALTER TABLE raw_sales_data
RENAME COLUMN "Product name" TO Product_Name;

ALTER TABLE raw_sales_data
RENAME COLUMN "Sales Amt" TO Sales_Amount;

ALTER TABLE raw_sales_data
RENAME COLUMN "Order Dt" TO Order_Date;

ALTER TABLE raw_sales_data
RENAME COLUMN "Ship Dt" TO Ship_Date;

ALTER TABLE raw_sales_data
RENAME COLUMN "Ship Mode" TO Shipping_Mode;

ALTER TABLE raw_sales_data
RENAME COLUMN "Cust City" TO Customer_City;

ALTER TABLE raw_sales_data
RENAME COLUMN "Cust State" TO Customer_State;

ALTER TABLE raw_sales_data
RENAME COLUMN "Cust Country" TO Customer_Country;

ALTER TABLE raw_sales_data
RENAME COLUMN "Prod Category" TO Product_Category;

ALTER TABLE raw_sales_data
RENAME COLUMN "Prod Sub-Category" TO Product_Subcategory;


-----------------------------------------------------------
-- 3️⃣ Remove Duplicates
-----------------------------------------------------------
-- Create a cleaned table removing duplicates based on unique identifiers
CREATE TABLE cleaned_sales_data AS
SELECT DISTINCT *
FROM raw_sales_data;

-----------------------------------------------------------
-- 4️⃣ Handling Missing Values
-----------------------------------------------------------
-- Replace NULL numeric values with average or 0
UPDATE cleaned_sales_data
SET Sales_Amount = COALESCE(Sales_Amount, 0);

-- Replace NULL text values with 'Unknown'
UPDATE cleaned_sales_data
SET Customer_City = COALESCE(Customer_City, 'Unknown'),
    Customer_State = COALESCE(Customer_State, 'Unknown'),
    Customer_Country = COALESCE(Customer_Country, 'Unknown'),
    Product_Name = COALESCE(Product_Name, 'Unknown'),
    Product_Category = COALESCE(Product_Category, 'Unknown'),
    Product_Subcategory = COALESCE(Product_Subcategory, 'Unknown');

-----------------------------------------------------------
-- 5️⃣ Convert Data Types (if needed)
-----------------------------------------------------------
-- Example: Convert Order_Date and Ship_Date from text to date
ALTER TABLE cleaned_sales_data
ALTER COLUMN Order_Date TYPE DATE USING TO_DATE(Order_Date, 'YYYY-MM-DD');

ALTER TABLE cleaned_sales_data
ALTER COLUMN Ship_Date TYPE DATE USING TO_DATE(Ship_Date, 'YYYY-MM-DD');

-----------------------------------------------------------
-- 6️⃣ Extract Date Components
-----------------------------------------------------------
-- Create new columns for year, month, and day of week
ALTER TABLE cleaned_sales_data
ADD COLUMN Order_Year INT,
ADD COLUMN Order_Month VARCHAR(20),
ADD COLUMN Order_Weekday VARCHAR(20);

UPDATE cleaned_sales_data
SET 
    Order_Year = EXTRACT(YEAR FROM Order_Date),
    Order_Month = TO_CHAR(Order_Date, 'Month'),
    Order_Weekday = TO_CHAR(Order_Date, 'Day');

-----------------------------------------------------------
-- 7️⃣ Create Reference Columns
-----------------------------------------------------------
-- Example: Create Profit and Profit Margin columns if Cost_Amount exists
ALTER TABLE cleaned_sales_data
ADD COLUMN Profit NUMERIC,
ADD COLUMN Profit_Margin NUMERIC;

UPDATE cleaned_sales_data
SET 
    Profit = Sales_Amount - Cost_Amount,
    Profit_Margin = ROUND((Profit / Sales_Amount) * 100, 2)
WHERE Cost_Amount IS NOT NULL;

-----------------------------------------------------------
-- 8️⃣ Standardize String Values
-----------------------------------------------------------
-- Trim spaces and ensure consistent casing for key text columns
UPDATE cleaned_sales_data
SET
    Customer_City = INITCAP(TRIM(Customer_City)),
    Customer_State = INITCAP(TRIM(Customer_State)),
    Customer_Country = INITCAP(TRIM(Customer_Country)),
    Product_Name = INITCAP(TRIM(Product_Name)),
    Product_Category = INITCAP(TRIM(Product_Category)),
    Product_Subcategory = INITCAP(TRIM(Product_Subcategory));

-----------------------------------------------------------
-- 9️⃣ Verify Cleaned Data
-----------------------------------------------------------
SELECT *
FROM cleaned_sales_data
LIMIT 20;

-----------------------------------------------------------
-- 🔟 Summary Checks
-----------------------------------------------------------
-- Check nulls after cleaning
SELECT 
    SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Null_Customer_ID,
    SUM(CASE WHEN Sales_Amount IS NULL THEN 1 ELSE 0 END) AS Null_Sales_Amount,
    SUM(CASE WHEN Order_Date IS NULL THEN 1 ELSE 0 END) AS Null_Order_Date
FROM cleaned_sales_data;

-- Count unique values
SELECT 
    COUNT(DISTINCT Customer_ID) AS Unique_Customers,
    COUNT(DISTINCT Product_Name) AS Unique_Products
FROM cleaned_sales_data;

-----------------------------------------------------------
-- ✅ End of Script
-----------------------------------------------------------
