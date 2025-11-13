----
Sales Data Cleaning & Preprocessing Project
-----
Table of Contents
-----
- **Project Overview**
  - Key Objectives:
 - **Dataset**
 - **Data Cleaning Process**
    - Load Data
    - Handling NULL and Missing Values
    - Standardizing Numeric Columns
    - Standardizing Dates
    - Standardizing Categorical Columns
    - Standardizing State & City
    - Creating Fullname Columns
    - Duplicate Detection
 - **Key Outcomes**
 - **Skills Demonstrated**
 - **Conclusion**

 -----
Project Overview
----
This project demonstrates a comprehensive data cleaning and preprocessing workflow using a sample sales dataset. The dataset contains order, customer, product, and geographical information. The goal is to clean, standardize, and validate the data to prepare it for data analysis, visualization, and reporting.

----
Key objectives:
----
* Identify and handle missing, blank, and malformed data.
* Standardize categorical and date fields.
* Create derived columns for better usability.
* Detect and report duplicate records.

Dataset
----
This project demonstrates SQL-based data cleaning and preprocessing using a real-world dataset.This datateset is gotten from kaggle.com The dataset used in this project is:
[sales_data_sample.csv](https://www.kaggle.com/datasets/kyanyoga/sample-sales-data)

| Column Name                  | Description                                 |
|------------------------------|---------------------------------------------|
| ORDERNUMBER                  | Unique order identifier                     |
| QUANTITYORDERED              | Quantity of products in the order           |
| PRICEEACH                    | Unit price of the product                   |
| ORDERLINENUMBER              | Line number in the order                    |
| SALES                        | Total sales amount                          |
| ORDERDATE                    | Date of the order                           |
| STATUS                       | Current status of the order                 |
| QTR_ID                       | Quarter of the order                        |
| MONTH_ID                     | Month number of the order                   |
| YEAR_ID                      | Year of the order                           |
| PRODUCTLINE                  | Category of the product                     |
| MSRP                         | Manufacturer's suggested retail price       |
| PRODUCTCODE                  | Unique product identifier                   |
| CUSTOMERNAME                 | Name of the customer                        |
| PHONE                        | Customer phone number                       |
| ADDRESSLINE1 / ADDRESSLINE2  | Customer address                            |
| CITY                         | City of the customer                        |
| STATE                        | State of the customer                       |
| POSTALCODE                   | Postal code                                 |
| COUNTRY                      | Country of the customer                     |
| TERRITORY                    | Sales territory                             |
| CONTACTFIRSTNAME / LASTNAME  | Customer contact name                       |
| DEALSIZE                     | Size of the deal                            |
----
Data Cleaning Process
----
## **1. Load Data**
Data was loaded into SQL Server Management Studio using the Import Data Wizard (Tasks → Import Data).

## **2. Handling NULL and Missing Values**
All empty strings ('') were converted to NULL using NULLIF() for consistent handling of missing data.
```sql
UPDATE sales_data_sample
SET 
ORDERNUMBER = NULLIF(ORDERNUMBER,''),
QUANTITYORDERED = NULLIF(QUANTITYORDERED,''),
PRICEEACH = NULLIF(PRICEEACH,''),
ORDERLINENUMBER = NULLIF(ORDERLINENUMBER,''),
SALES = NULLIF(SALES,''),
ORDERDATE = NULLIF(ORDERDATE,''),
`STATUS` = NULLIF(`STATUS`,''),
QTR_ID = NULLIF(QTR_ID,''),
MONTH_ID = NULLIF(MONTH_ID,''),
YEAR_ID = NULLIF(YEAR_ID,''),
PRODUCTLINE = NULLIF(PRODUCTLINE,''),
MSRP = NULLIF(MSRP,''),
PRODUCTCODE = NULLIF(PRODUCTCODE,''),
CUSTOMERNAME = NULLIF(CUSTOMERNAME,''),
PHONE = NULLIF(PHONE,''),
ADDRESSLINE1 = NULLIF(ADDRESSLINE1,''),
ADDRESSLINE2 = NULLIF(ADDRESSLINE2,''),
CITY = NULLIF(CITY,''),
STATE = NULLIF(STATE,''),
POSTALCODE = NULLIF(POSTALCODE,''),
COUNTRY = NULLIF(COUNTRY,''),
TERRITORY = NULLIF(TERRITORY,''),
CONTACTLASTNAME = NULLIF(CONTACTLASTNAME,''),
CONTACTFIRSTNAME = NULLIF(CONTACTFIRSTNAME,''),
DEALSIZE = NULLIF(DEALSIZE,'');
```
