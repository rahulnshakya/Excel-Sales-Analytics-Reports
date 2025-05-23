# <img src="https://miro.medium.com/v2/resize:fit:1400/1*8bUjUiCWk0VhS8-lgAj0Og.png" width="4%" height="4%"> Atliq Hardware Business Analytics

![image](https://github.com/user-attachments/assets/a801a608-a51f-46e2-9615-e31f83df1ac2)



This repository documents my work on the AtliQ Hardware Sales & Finance Analytics project, completed using Microsoft Excel 2019. It was undertaken as a self-paced learning initiative with guidance from Codebasics.

Please note that project files are not included in this repository, in accordance with Codebasics’ Data & Content Distribution Policy.


---

## Contents:
Please find the sectional links for the project below:
- [Introduction](#introduction)
  - [Problem Statement](#problem-statement)
- [AtliQ Hardware Compiled Report](https://github.com/5ifar/AtliQHardware_Sales_and_Finance_Analytics/blob/main/AtliQ%20Hardware%20Compiled%20Report.pdf)
- [Sales Analytics Reports](https://github.com/5ifar/Sales_and_Finance_Analytics_of_AtliQHardwares/tree/main/Sales%20Analytics%20Reports/Sales%20Analytics%20Reports%20Files)
  - [Sales Analytics Reports Documentation](https://github.com/5ifar/Sales_and_Finance_Analytics_of_AtliQHardwares/blob/main/Sales%20Analytics%20Reports/Sales%20Analytics%20Reports%20Documentation.md)
- [Finance Analytics Reports](https://github.com/5ifar/Sales_and_Finance_Analytics_of_AtliQHardwares/tree/main/Finance%20Analytics%20Reports/Finance%20Analytics%20Reports%20Files)
  - [Finance Analytics Reports Documentation](https://github.com/5ifar/Sales_and_Finance_Analytics_of_AtliQHardwares/blob/main/Finance%20Analytics%20Reports/Finance%20Analytics%20Reports%20Documentation.md)
- [AtliQ Hardware Report Presentation](https://github.com/5ifar/AtliQHardware_Sales_and_Finance_Analytics/blob/main/AtliQ%20Hardware%20Report%20Presentation.pptx)
- [Tools used & Methodologies implemented](#tools-used)
- [About the Dataset](#about-the-dataset)
  - [Data Integrity](#data-integrity)
- [Data Model - ERD](#data-model)
- [Analysis Insights](#analysis-insights)

---

## Introduction:
**Domain:** FMCG | **Functions:** Sales & Finance

AtliQ Hardwares is a global provider of computer hardware and peripherals such as PCs, printers, and mice. 
The company primarily operates under a B2B model, supplying products to major retail partners like Croma, Best Buy, Staples, and Flipkart, who then sell to end consumers.

  
- Sales are conducted through three primary channels: Retailer, Direct, and Distributor.

  * Retailer Channel:
   AtliQ’s customers in this channel include:
  
  
  Brick & Mortar Stores (e.g., Croma, Best Buy)
  
  
  E-commerce Platforms (e.g., Amazon, Flipkart)
  
  
  * Direct Channel:
   The company also follows a smaller B2C model through its own outlets, including:
  
  
  AtliQ E-store
  
  
  AtliQ Exclusive
  
  
  * Distributor Channel:
   In regions with restricted trade policies, AtliQ partners with local distributors, such as Neptune.

## Problem Statement:
 AtliQ Hardwares has been experiencing substantial financial losses in recent years. One of the key issues identified is their continued reliance on handwritten reports for critical business decisions. This outdated approach has hindered their ability to make timely and data-driven decisions, creating an urgent need for actionable insights.
 
**Business Requirement:**
 To address this challenge, AtliQ’s business team has assigned the Data Analyst team the task of developing a comprehensive Excel-based analysis report. The focus is on evaluating Sales and Financial performance by consolidating and analyzing data from multiple sources, comprising over 1.5 million records. The objective is to extract meaningful insights that will support strategic decision-making and drive business growth.


## Tools used:
1. Microsoft Excel: for Data Cleaning, Data Analysis & Visualization
2. Microsoft Powerpoint: for creating Project Presentation
3. DataWrapper: for Insights Visuals
4. GitHub - for Documentation

## Skills & Methodologies implemented:
1. Data Cleaning: **ETL, Power Query**
2. Data Manipulation: **VLOOKUP/INDEX-MATCH/XLOOKUP Table Joining, DAX Measures & Columns**
3. Data Modelling and Normalization
4. Data Visualization: **Pivot Table, Power Pivot, Conditional Formatting**
5. Documentation

---

## About the Dataset:
### Data Sources: Sales & Finance

![image](https://github.com/5ifar/AtliQHardwares/assets/146955609/fc7d37af-acbf-4596-83dc-8335ee737b0b)

- dim_customer: 189 records | 5 columns
- dim_market: 23 records | 3 columns
- dim_product: 298 records | 6 columns
- fact_sales_monthly: 799962 records | 5 columns
- ns_targets_2021: 276 records | 3 columns
- fact_sales_monthly_with_cost: 799962 records | 7 columns

### Data Dictionary:
-to be added-

## Data Integrity:
ROCCC Evaluation:
- Reliability: MED - The raw dataset is created and updated by Codebasics. It has 6 files.
- Originality: HIGH - First party provider (Codebasics)
- Comprehensiveness: MED - Total 6 CSV Files were provided. Dataset contains multiple parameters for Customers, Products & Markets as well as comprehensive Sales & Finance transaction data.
- Current: LOW - Dataset was updated upto 2021, almost 3 years old. So its obsolete & not very relevant.
- Citation: LOW - No official citation/reference available.

---

## Data Model:
### Entity Relationship Diagram (ERD):

<img src="https://github.com/5ifar/AtliQHardware_Sales_and_Finance_Analytics/blob/main/Assets/AtliQ%20Hardware%20Data%20Model%20ERD%20Final.JPG" width="100%" height="100%">

---

## Analysis Insights:

<img src="https://datawrapper.dwcdn.net/MYLHC/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/qU7i7/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/aLbnS/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/qxZsY/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/j22dx/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/Xj08J/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/OdCYE/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/mcbgG/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/SxD4Y/full.png" alt="" />

<img src="https://datawrapper.dwcdn.net/ITvwk/full.png" alt="" />

---
