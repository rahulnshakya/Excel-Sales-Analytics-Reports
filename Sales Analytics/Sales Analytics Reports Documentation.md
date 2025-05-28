# 🛒 Sales Analytics Reports
---

## Table of Contents
- [Sales Analytics Purpose](#Sales-Analytics-Purpose)
- [Reports List](#Reports-List)
- [Sales Analytics Reports Detailed Implementation](#Sales-Analytics-Reports-Detailed-Implementation)
  - [ETL](#ETL)
  - [Business Report Design Components](#Business-Report-Design-Components)
  - [Data Modelling](#Data-Modelling)
  - [New dim_date table using Power Query](#New-dim_date-table-using-Power-Query)
  - [Customer Sales Performance Report](#Customer-Sales-Performance-Report)
  - [Market Sales Performance vs Target Report](#Market-Sales-Performance-vs-Target-Report)
  - [Top 10 Products by Sales Increment Report](#Top-10-Products-by-Sales-Increment-Report)
  - [Division Sales Report](#Division-Sales-Report)
  - [Top & Bottom 5 Products by Qty Report](#top--bottom-5-products-by-qty-report)
  - [New Products 2021 Report](#new-products-2021-report)
  - [Top 5 Country 2021 Report](#Top-5-Country-2021-Report)

---

## Sales Analytics Purpose: 
Empowering businesses to monitor, evaluate, and enhance their sales activities and outcomes by unveiling sales patterns, tracking essential performance indicators (KPIs) and driving informed decisions.

**Outcome:** Determine effective customer discounts, facilitate negotiations, and identify expansion opportunities in potential global markets.

---

## Reports List:
1. [Customer Performance Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Customer%20Performance%20Report.pdf): Assessed Customer Net Sales performance across 2019, 2020, 2021 year period.
2. [Market Performance vs Targets Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Market%20Performace%20vs%20Target.pdf): Analyzed Market Net Sales performance against Net Sales Targets in 2021.
3. [Top 10 Products Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Top%2010%20Products.pdf): Identified Top 10 Products by Net Sales Increment % in 2021 vs 2020.
4. [Division Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Division%20Level%20Report.pdf): Documented Divison level Net Sales performance across 2020 & 2021 with a focus on Net Sales Increment % in 2021 vs 2020.
5. [Top & Bottom 5 Products Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Top%20and%20Bottom%205%20Products%20-%20Sold.pdf): Identified the Top and Bottom 5 products by Qty sold across the 3 year period.
6. [New Products 2021 Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/New%20Products%20-%202021.pdf): Assessed Net Sales performance of 16 new products launched in 2021.
7. [Top 5 Country Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/13c397f6864dd9714699ab0687031f2fbda8ccc7/Sales%20Analytics/Top%205%20Countries(Net%20Sales-2021).pdf): Listed Top 5 countries leading Net Sales figures in 2021.

---

## Sales Analytics Reports Detailed Implementation:

### ETL:
1. Extract Data directly from the Sales folder containing 4 files: dim_customer.csv, dim_market.csv, dim_product.csv, fact_sales_monthly.csv (dim: Dimension/Lookup Table, fact: Fact Table). 
All files in the folder will be combined in Power Query. To separate them we’ll right click on main file and create references for all the individual files, this way if any data changes are done in the main file it will auto replicate in all the other files as well.
2. Data Cleaning: Check Data Quality: View → Enable Column Distribution & Column Quality. For Primary Keys (customer_code, product_code, market), Distinct and Unique values in column should be the same.
3. Data Transformation:
  i.] In dim_customer: Find and Replace values: AltiQ → AtliQ; Atliq → AtliQ
  ii.] In dim_market, when extracting region/subzone NA (North America) got interpreted as Not Available value: Find and Replace values: nan → NA
  iii.] In fact_sales_monthly, somewhere in the data pipeline negative values got introduced in the qty column: Column → Transform → Scientific → Absolute value
4. Data Load: Save & Load To all 4 files as Connection Only and load then to the Data Model.

### Business Report Design Components:
- Net Sales: We’ll get net_sales data from the fact_sales_monthly table from the net_sales_amount column.
- Year: We’ll extract year data from the fact_sales_monthly table from the date column.
- Division: We’ll get division data from the dim_product table from the division column.
- Country: We’ll get country data from the dim_market table from the market column.
- Region: We’ll get region data from the dim_market table from the region column.
- Customer: We’ll get customer data from the dim_customer table from the customer column.

### Data Modelling:
1. In Power Pivot Diagram View, Arrange the 4 tables in a Star Schema with the fact_sales_monthly table in the center. There will be Snowflake Schema between dim_customer and dim_market.
2. Configure Table Relationships:

|Primary Key| |Foreign Key|
|-|-|-|
|customer_code (PK, dim_customer)|→|customer_code (FK, fact_sales_monthly)|
|product_code (PK, dim_product)|→|product_code (FK, fact_sales_monthly)|
|market (PK, dim_market)|→|market (FK, dim_customer)|

### New dim_date table using Power Query:
- This will provide us with the Year data and an added advantage of implementing date hierarchy when required.
1. Power Query → New Query/New Source → Other Sources → Blank Query → Rename as dim_date → Add Query: = {Number.From(#date(2018,1,1))..Number.From(#date(2018,12,31))} → Edit Start Date as (2018,9,1) and End Date as (2021,8,1) as per the Sales data → Change column type to Date → Rename column to date → Extract Start of Month to new column, Rename to month → Extract Year to new column, Set Data type as text to avoid future aggregation
2. AtliQ Hardware follows a fiscal year that runs from September through August. If the Fiscal Year starts from Sep to Aug, then we need to convert the Calendar Year to the Fiscal year, to do this in this case we’ll have to add 4 months to the Calendar Date and the Year part of the new date will tell us the Fiscal year. E.g. Sep 2021 + 4 months = Jan 2022 → FY 22
3. We’ll add a custom column FY Month to dim_date to add the 4 months using DAX Formula: Date.AddMonths([month],4)
4. We’ll then add a custom column FY to get the Year part from the FY Month column using DAX Formula: Date.Year([FY Month]). This can also be done from Add Column Tab, Date → Year.
Remove year & FY Month column to avoid confusion.
5. In Power Pivot Data Model Diagram View, we’ll now configure the table relationship: date (PK, dim_date) → date (FK, fact_sales_monthly)

### Customer Sales Performance Report:
1. Insert Power Pivot from Insert Tab based on the Data Model created.
2. Add customer (from dim_customer) field to the Rows Area. Add region (from dim_market), market (from dim_market) & division (from dim_product) to Filters Area. Enable Select Multiple Items for Filters.
3. Create Net Sales Measure using DAX formula: SUM(fact_sales_monthly[net_sales_amount]) in the fact_sales_monthly table. Set data type as $ Currency with 2 decimal accuracy.
4. Now we’ll create separate Net Sales Measures for each FY. For this we’ll use the CALCULATE formulate to filter NetSales Measure in the Filter Context of FY column from dim_date table.

    Measure: NetSales2019 → =CALCULATE([NetSales], dim_date[FY]="2019")

    Measure: NetSales2020 → =CALCULATE([NetSales], dim_date[FY]="2020")

    Measure: NetSales2021 → =CALCULATE([NetSales], dim_date[FY]="2021")

    (Since we had set the FY column of type text to avoid aggregation we need to put them in double quotes as string.)

5. Add a comparison Measure NetSales2021vs2020 using DAX formula: =DIVIDE([NetSales2021], [NetSales2020],0) and format it as Number Percentage with 1 decimal accuracy.
6. Add the NetSales2019, NetSales2020, NetSales2021 & NetSales2021vs2020 Measures to the Values Area in Power Pivot.
7. To boost User Readability:

    i. Convert all NetSales Measures to Million denomination by changing the Value Field Number Format to Custom: $ #,##0.0,, "M" → This will give NetSales with 1 decimal accuracy in Million        Dollars.
   
    ii. Remove Gridlines and set Page Layout in View Tab along with Border setup.
   
8. Sort the Pivot Table by Descending NetSales 2021vs2020 Column values for best stakeholder experience in identifying Customers with high Sales Growth %. 
9. Configure Conditional Formatting:

    i. For all 3 NetSales Columns set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.
   
    ii. For the NetSales 2021vs2020 Column set Data Bar Formatting with Orange Gradient fill.
   
10. Add AtliQ Hardware Logo and “AtliQ Hardwares” as the Report Header Title. Add “Customer Net Sales Performance Report” as the Report Title.

### Market Sales Performance vs Target Report:
To Do: 1. Add Target Data 2. Connect it to the data model
1. Copy the Customer Net Sales Performance Power Pivot sheet to a new sheet and rename sheet and report title to Market Performance vs Target Report.
2. Remove irrelevant columns like Sales 2021 vs 2020. Replace Customers in Rows Area by Market (i.e. Countries) and rename the Column header to reflect the same.
3. Create New Query to import data thorugh CSV file ns_targets_2021.csv. Configure Load as Connection Only and Add to Data Model.
4. In Power Pivot Data Model Diagram View, we’ll now configure the table relationships:

|Primary Key| |Foreign Key|
|-|-|-|
|market (PK, dim_market)|→|market (FK, ns_targets_2021)|
|date (PK, dim_date)|→|date (FK, ns_targets_2021)|

5. Create SalesTarget2021 Measure using DAX formula: SUM(ns_targets_2021[ns_target]) in the ns_targets_2021 table. Set data type as $ Currency with 2 decimal accuracy.
6. Create Sales2021 - SalesTarget2021 Measure using DAX formula: [NetSales2021] - [SalesTarget2021] in the ns_targets_2021 table. Set data type as $ Currency with 2 decimal accuracy. Add this Measure to the Values Area in Pivot table.
7. Convert both the above Measures to Million denomination by changing the Value Field Number Format to Custom: $ #,##0.0,, "M" → This will give Sales with 1 decimal accuracy in Million Dollars.
8. Create Sales2021 - SalesTarget2021 % Measure using DAX formula: DIVIDE([Sales2021 - SalesTarget2021], [SalesTarget2021], 0) in the ns_targets_2021 table. Set data type as Number Percentage with 1 decimal accuracy. Add this Measure to the Values Area in Pivot table.
9. Sort the Pivot Table by Ascending Sales2021 - SalesTarget2021 % Column values for best stakeholder experience in identifying Markets with high gap in Target and Actual Net Sales in 2021.
10. Configure Conditional Formatting:

    i. For the Sales2021 - SalesTarget2021 % Column set Data Bar Formatting with Red Gradient fill since it has negative values.
    
    ii. For Sales2021 - SalesTarget2021 Column set Highlight Cell Formatting with 3 colour scale: Dark Yellow for low values, Light Yellow for average values and White for high values since          the values are negative.

### Top 10 Products by Sales Increment Report:
To Do: Add Product Value Filter for Top 10 products.
1. Copy the Customer Net Sales Performance Power Pivot sheet to a new sheet and rename sheet and report title to Top 10 Products by Sales Increment Report.
2. Remove irrelevant columns like Sales 2019. Replace Customers in Rows Area by Product and rename the Column header to reflect the same. Replace Market Filter by Customer in Filters Area. Enable Select Multiple Items for Filters.
3. Create SalesIncrement2021vs2020% Measure using DAX formula: DIVIDE([NetSales2021]-[NetSales2020],[NetSales2020],0) in the fact_sales_monthly table. Set data type as Number Percentage with 1 decimal accuracy. Add this Measure to the Values Area in Pivot table.
4. Configure a Top 10 Value Filter on Products based on SalesIncrement2021vs2020% Measure column.
5. Sort the Pivot Table by Descending SalesIncrement2021vs2020% Column values for best stakeholder experience in identifying Products with high Sales Increment % in 2021.
6. Configure Conditional Formatting:

    i. For Sales 2020 & Sales2021 Column set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.
   
    ii. For the SalesIncrement2021vs2020% Column set Data Bar Formatting with Orange Gradient fill.

### Division Sales Report:
1. Copy the Top 10 Products by Sales Increment Power Pivot sheet to a new sheet and rename sheet and report title to Division Sales Report. Remove the preset Top 10 value Filter on Product field.
2. Replace Product in Rows Area by Division and rename the Column header to reflect the same.
3. Sort the Pivot Table by Descending SalesIncrement2021vs2020% Column values for best stakeholder experience in identifying Divisions with high Sales Increment % in 2021.
4. Configure Conditional Formatting:

    i. For Sales 2020 & Sales2021 Column set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.
   
    ii. For the SalesIncrement2021vs2020% Column set Data Bar Formatting with Orange Gradient fill.

### Top & Bottom 5 Products by Qty Report:
To Do: Add Qty Value Filter for Top 5 and Bottom 5 products.

*For Top 5 Products:*
1. Copy the Division Sales Power Pivot sheet to a new sheet and rename sheet and report title to Top & Bottom 5 Products by Qty Report.
2. Replace Division in Rows Area by Product and rename the Column header to reflect the same. Move Division to Filters Area. Enable Select Multiple Items for Filters.
3. Remove all irrelevant columns like Sales 2020, Sales 2021 & SalesIncrement2021vs2020%. Add Qty to the Values Area aggregated as SUM.
4. Convert the Qty column to Million denomination by changing the Value Field Number Format to Custom: #,##0.0,, "M" → This will give Qty with 1 decimal accuracy in Millions.
5. Configure a Top 5 Value Filter on Products based on Qty column.
6. Sort the Pivot Table by Descending Qty Column values for best stakeholder experience in identifying high Qty Sold Products.
7. Configure Conditional Formatting: For Qty Column set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.

*For Bottom 5 Products:*
1. Copy the Top 5 Products Power Pivot Table to a new cell reference.
2. Modify to Bottom 5 Value Filter on Products based on Qty column.
3. Convert the Qty column to Thousands denomination by changing the Value Field Number Format to Custom: #,##0.0, "K" → This will give Qty with 1 decimal accuracy in Thousands.
4. Sort the Pivot Table by Ascending Qty Column values for best stakeholder experience in identifying low Qty Sold Products.
5. Configure Conditional Formatting: For Qty Column set Highlight Cell Formatting with 3 colour scale: Dark Yellow for low values, Light Yellow for average values and White for high values.

### New Products 2021 Report:
To Do: Filter the SalesIncrement2021vs2020% value for 0%.
1. Copy the Top 10 Products by Sales Power Pivot sheet to a new sheet and rename sheet and report title to New Products 2021 by Sales Report.
2. Remove the preset Top 10 Value filter on Products field. Set a new Value filter to show Products for which SalesIncrement2021vs2020% column value is 0% i.e they did not make any sales in 2020.
3. Remove the SalesIncrement2021vs2020% field from Values Area.
4. Sort the Pivot Table by Descending NetSales2021 Column values for best stakeholder experience in identifying New Products with high Sales figures.
5. Configure Conditional Formatting: For NetSales2021 Column set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.

### Top 5 Country 2021 Report:
To Do: Add NetSales2021 Value Filter for Top 5 Markets.
1. Copy the Market Sales Performance Power Pivot sheet to a new sheet and rename sheet and report title to Top 5 Country - 2021 Report.
2. Remove all irrelevant field from the Values Area except the NetSales2021 field. Replace Division by Customer in Filters Area. Enable Select Multiple Items for Filters.
3. Configure a Top 5 Value Filter on Markets based on NetSales2021 column.
4. Sort the Pivot Table by Descending NetSales2021 Column values for best stakeholder experience in identifying Countries with high Sales figures.
5. Configure Conditional Formatting: For NetSales2021 Column set Highlight Cell Formatting with 3 colour scale: White for low values, Light Yellow for average values and Dark Yellow for high values.

---
