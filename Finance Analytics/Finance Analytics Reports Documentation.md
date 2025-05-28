# 💸 Finance Analytics Reports
---

## Table of Contents
- [Finance Analytics Purpose](#Finance-Analytics-Purpose)
- [Reports List](#Reports-List)
- [Finance Analytics Reports Detailed Implementation](#Finance-Analytics-Reports-Detailed-Implementation)
  - [Import Finance Data](#Import-Finance-Data)
  - [P & L by Years Report](#P--L-by-Years-Report)
  - [Adding Months and Quarters in dim_date Data Model](#Adding-Months-and-Quarters-in-dim_date-Data-Model)
  - [P & L by Months Report](#P--L-by-Months-Report)
  - [P & L by Markets Report](#P--L-by-Markets-Report)
  - [GM% by Quarters Report](#GM-by-Quarters-Report)

---

## Finance Analytics Purpose:
Evaluating financial performance, aiding decision-making, and fostering stakeholder communication through benchmarking against industry peers, historical periods, and establishing the foundation for budgeting and forecasting.

**Outcome:** Aligning financial planning with strategic objectives and instilling confidence in the organization's financial outlook.

---

## Reports List:
1. [P&L - Financial Years Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/0a8f9ba586269aebf51c147c750946b3ae2526cf/Finance%20Analytics/P%26L%20COGS.pdf): Generated P&L reports categorized by markets and analyzed over metrics: NetSales, COGS, GrossMargin & GM% across financial years 2019, 2020 & 2021 with focus on Metric Increment in 2021 vs 2020.
2. [P&L - Financial Months Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/0a8f9ba586269aebf51c147c750946b3ae2526cf/Finance%20Analytics/P%26L%20By%20Fiscal%20Month%20for%20Year%2019%2020%2021.pdf): Generated P&L reports analyzed over metrics: NetSales, COGS, GrossMargin & GM% across financial months cycle starting from Sep to Aug.
3. [P&L - Markets Reports](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/0a8f9ba586269aebf51c147c750946b3ae2526cf/Finance%20Analytics/P%26L%20For%20All%20Market.pdf): Generated P&L reports categorized by markets and analyzed over metrics: NetSales, COGS, GrossMargin & GM% with a temporary filter context of FY 2021 (can be modified).
4. [GM% - Financial Quarters Report](https://github.com/rahulnshakya/Excel-Sales-Analytics-Reports/blob/0a8f9ba586269aebf51c147c750946b3ae2526cf/Finance%20Analytics/GM%20%25%20BY%20Quarters%20-%20sub_zone.pdf): Analyzed GM% metric categorized by sub zones across financial quarters Q1 to Q4.

---

## Finance Analytics Reports Detailed Implementation:

### Import Finance Data:
1. Create New Query to import data thorugh CSV file fact_sales_monthly_with_cost.csv. Rename it to finance_ref. Configure Load as Connection Only and Add to Data Model.
2. Configure Table Relationships:

|Primary Key| |Foreign Key|
|-|-|-|
|customer_code (PK, dim_customer)|→|customer_code (FK, finance_ref)|
|product_code (PK, dim_product)|→|product_code (FK, finance_ref)|
|date (PK, dim_date)|→|date (FK, finance_ref)|

### P & L by Years Report:
1. Insert Power Pivot from Insert Tab based on the Data Model created.
2. Add NetSales Measure to the Values Area. Add region (from dim_market), market (from dim_market) & division (from dim_product) to Filters Area in Pivot table. Enable Select Multiple Items for Filters.
3. Add FY Measure from dim_date table to the Columns Area in Pivot table.
4. Create a new column total_cogs in the finance_ref Data Model table by adding both freight cost and manufacturing cost.
5. Create a new measure COGS in the finance_ref table referencing the total_cogs column using DAX formula: SUM(finance_ref[total_cogs]). Set data type as $ Currency with 2 decimal accuracy. Add this measure to the Values Area in Pivot table.
6. Create a new measure GrossMargin in the finance_ref table using the DAX formula: [NetSales] - [COGS]. Set data type as $ Currency with 2 decimal accuracy. Add this measure to the Values Area in Pivot table.
7. Create a new measure GM% in the finance_ref table using the DAX formula: DIVIDE([GrossMargin],[NetSales],0). Set data type as Number Percentage with 2 decimal accuracy. Add this Measure to the Values Area in Pivot table.
8. Convert the NetSales, COGS & GrossMargin measures to Million denomination by changing the Value Field Number Format to Custom: #,##0.0,, "M" → This will give measures with 1 decimal accuracy in Millions.
9. Configure Conditional Formatting:

    i. For NetSales, COGS, Gross Margin & GM% Metrics set Highlight Cell Formatting with 3 colour scale: White for low values, Yellow for average values and Dark Yellow for high values.
    ii. For the 2021vs2020 Column set Data Bar Formatting with Orange Gradient fill. Set Red Data bar for Negative values to be shown from cell midpoint.

### Adding Months and Quarters in dim_date Data Model:
1. Add a new column mmm (represents 3 letter months) to the dim_date Data Model using formula: =FORMAT([date], "MMM")
2. Add a new column fy_month_nbr(represents fiscal month nbr) to the dim_date Data Model using formula: =MONTH(DATE(YEAR([date]), MONTH([date])+4, 1))
This will get us the month part of 4 months ahead of the date column.
3. Add a new column quarter(represents fiscal quarter) to the dim_date Data Model using formula: ="Q" & ROUNDUP([fy_month_nbr]/3,0)
Here, ROUNDUP function acts similar to CEIL function in SQL, it rounds up the value to the next integer. Then we concatenate the “Q” to the start.

### P & L by Months Report:
1. Copy the P&L by Year Power Pivot sheet to a new sheet and rename sheet and report title to P & L Months.
2. Add FY to the Filters Area. Add Quarter & mmm columns to the Columns Area.
3. To order the mmm column values in the Pivot table by fiscal year i.e Sep to Aug, we’ll sort the mmm column in the Data Model by the  fy_month_nbr column in ascending order.
4. Configure Conditional Formatting: For NetSales, COGS, Gross Margin & GM% Metrics set Highlight Cell Formatting with 3 colour scale: Light Yellow for low values, Yellow for average values and Dark Yellow for high values.
5. We want the same Pivot Table for all 3 years. Select the entire table from Pivot Table Design copy 3 times and set the different years for each table.
6. Add the titles for NetSales Metric comparison between 2021 vs 2020 and 2020 vs 2019. Calculate Growth by using basic cell references, dividing the difference in values with the older value and converting it to percentage with 1 decimal accuracy.

### P & L by Markets Report:
1. Copy the P&L by Year Power Pivot sheet to a new sheet and rename sheet and report title to P & L Markets.
2. Add sub_zone to the Filters Area. Add NetSales, COGS, Grossmargin & GM% metrics to the Columns Area.
3. Convert the NetSales, COGS & GrossMargin measures to Million denomination by changing the Value Field Number Format to Custom: #,##0.0,, "M" → This will give measures with 1 decimal accuracy in Millions.
4. Configure Conditional Formatting: For NetSales, COGS, Gross Margin & GM% Metrics set Highlight Cell Formatting with 3 colour scale: White for low values, Yellow for average values and Dark Yellow for high values.

### GM% by Quarters Report:
1. Copy the P&L by Month Power Pivot sheet to a new sheet and rename sheet and report title to  GM% by Quarters (sub_zone).
2. Keep only FY in Filters Area. Keep quarter in Columns Area. Keep sub_zone in Rows Area. Keep GM% in Values Area. Add a filter for FY 2019.
3. Configure Conditional Formatting: For GM% Metric set Highlight Cell Formatting with 3 colour scale: White for low values, Yellow for average values and Dark Yellow for high values.
4. Copy the FY 2019 Power Pivot Table 2 times and set FY filters as 2020 and 2021 respectively.

---
