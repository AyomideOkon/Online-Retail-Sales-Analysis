# Online-Retail-Sales-Analysis

### Table Of Content
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis Query](#data-analysis-query)
- [Recommendation](#recommendation)
- [Limitations](#limitations)
- [References](#references)

### Project Overview

Project Overview
This project analyzes online retail transaction data to uncover insights into sales performance, customer behavior, and product trends. Using Excel for data cleaning, SQL for analysis, and Tableau for visualization, the project delivers an interactive dashboard that helps identify key business metrics, top-performing products, valuable customers, and revenue trends to support data-driven decision-making.

<img width="750" height="450" alt="Online Retail Performance Dashboard" src="https://github.com/user-attachments/assets/956eaf10-68e7-45cc-a3d1-f8195ab681f0" />

### Data Source
This project uses the Online Retail II dataset from the UCI Machine Learning Repository. The dataset contains transactional data from a UK-based online retailer, including invoices, products, quantities, prices, customer IDs, and countries. It is widely used for data analytics, customer behavior, and sales analysis projects.

Link: [Online Retail II Dataset](https://archive.ics.uci.edu/dataset/502/online+retail+ii?utm_source=chatgpt.com)

### Tools
- SQL- Data Cleaning And Analysis [Download here](https://www.google.com/aclk?sa=L&ai=DChsSEwiQ1ZSIuMiVAxU_pVAGHSVxOMgYACICCAEQARoCZGc&ae=2&co=1&ase=2&gclid=Cj0KCQjwsMLSBhD9ARIsAIpUTDoFp9zUz4I9oUIch82IuUSqthaIs-la6BhhicCi6buyLOHJEnon1dUaAnvHEALw_wcB&cid=CAASuwHkaHUeRtYPXh2-XjbEIyN5mV4IQKLM2_9v2gjeebigqXvALRuPxh_yca1mqxUbdCSr69V5SdMbETP3hR5eu-cBIG8sQMzz8RizvVVkmk6NjH4Ei9da0rgZFV79hmaOGYzop6X-ov-uQDc_rL12CMvYG5ffPPZEIXeKQbPw3EVRORBS_sTST01FuSEswphZQBKfxF7e7UUs2V_rI4XmrjMPDIMZgZUVNuLvp7dHXZ08VMwIWxvTL4Fk8FAy&cce=2&category=acrcp_v1_71&sig=AOD64_3t8n4aBzuueq7GyshaM7ubBGE5pw&q&nis=4&adurl&ved=2ahUKEwiouo6IuMiVAxWDe0EAHeeZOV8Q0Qx6BAgVEAE)
- Tableau- Data Visualization [Download here](https://www.tableau.com/products/public)

### Data Cleaning/Preparation
The following data cleaning steps were performed in SQL before analysis:
- Removed records with missing CustomerID values, as these transactions could not be linked to individual customers.
- Removed duplicate records to ensure each transaction was represented only once.
- Filtered out records where Quantity ≤ 0, as these represented returned, cancelled, or invalid transactions.
- Filtered out records where UnitPrice ≤ 0, removing transactions with invalid or zero prices.
- Standardized the InvoiceDate field into the appropriate date/time format for time-based analysis.
- Verified data consistency by checking data types and ensuring key fields contained valid values.
- Created a cleaned dataset containing only valid customer transactions for subsequent analysis and visualization.

### Exploratory Data Analysis
The following analyses were conducted to answer key business questions:
-Calculated total revenue generated from all valid transactions.
- Analyzed monthly revenue trends to identify seasonal sales patterns.
- Identified the top-selling products by revenue.
- Identified the most purchased products based on quantity sold.
- Determined the top customers based on total revenue generated.
- Analyzed sales performance by country.
- Calculated the total quantity of products sold.
- Calculated the total number of unique customers.
- Calculated the total number of orders (invoices).
- Explored product sales performance to identify best-performing products.
- Compared revenue contributions across different countries.
- Identified peak and low sales periods using transaction dates.
- 
### Data Analysis Query
Total Revenue
  ```sql
  SELECT
     SUM(Price*Quantity) AS Total_Revenue
  FROM 
   `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
  ```
  
Total Orders 
```sql
SELECT
     COUNT(DISTINCT Invoice) AS Total_Orders
FROM 
    `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
```
Total Customers
```sql
SELECT
     COUNT(DISTINCT `CustomerID`) AS Total_Custmers
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table`
```
Total Quantity Sold
```sql
SELECT
     SUM(Quantity) AS Total_Quantity
FROM
     `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
```
Monthly Revenue Trend
```sql
SELECT
    DATE_TRUNC(DATE(InvoiceDate), MONTH) AS Month,
    ROUND(SUM(Quantity * Price), 2) AS Monthly_Revenue
FROM
    `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
GROUP BY Month
ORDER BY Month;
```
Top Products By Revenue
```sql
SELECT
     Description
     SUM(Price*Quantity) AS Total_Revenue
FROM
     `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
GROUP BY
       Description
ORDER BY
       Total_Revenue DESC
```
Churn by Monthy Charges
```sql
SELECT
    CASE
    WHEN MonthlyCharges >= 0 AND MonthlyCharges < 20 THEN '0-20'
    WHEN MonthlyCharges >= 20 AND MonthlyCharges < 40 THEN '20-40'
    WHEN MonthlyCharges >= 40 AND MonthlyCharges < 60 THEN '40-60'
    WHEN MonthlyCharges >= 60 AND MonthlyCharges < 80 THEN '60-80'
    ELSE '80+'
    END AS Monthly_Charge_Group,
    COUNT(*) total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ count(*) AS churn_rate,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) ASS Total_churn
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
      Monthly_Charge_Group
ORDER BY 
       Monthly_Charge_Group;
```
Churn Rate By Internet Service
```sql
SELECT
    InternetService,
    COUNT(*) total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ COUNT(*) AS churn_rate,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) AS Total_churn

FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
      InternetService
ORDER BY 
       InternetService
```
Churn Rate By Payment Method
```sql
SELECT
    PaymentMethod,
    COUNT(*) total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ COUNT(*),2) AS churn_rate,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) AS Total_churn

FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
      PaymentMethod
ORDER BY
       PaymentMethod;
```
Churn Rate By Tech Support
```sql
SELECT
    TechSupport,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ COUNT(*),2) AS churn_rate,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) AS Total_churn

FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
      TechSupport
ORDER BY
       TechSupport;
```
Churn Rate By Online Security
```sql
SELECT
    OnlineSecurity,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ COUNT(*),2) AS churn_rate,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) AS Total_churn

FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
       OnlineSecurity
ORDER BY
        OnlineSecurity
```
### Reults/Findings
The analysis results are summarized as follows:
- The analysis identified an overall customer churn rate of 26.54%, indicating that approximately one in four customers left the company.
- Customers on Month-to-month contracts exhibited the highest churn rate, while customers on one-year and two-year contracts were significantly more likely to remain with the company.
- Customers with shorter tenures were more likely to churn, suggesting that the highest risk of attrition occurs during the early stages of the customer lifecycle.
- Higher monthly charges were associated with increased customer churn, indicating that pricing may influence customer retention.
- Electronic Check was the payment method with the highest churn rate compared to other payment options.
- Customers using Fiber Optic internet service experienced the highest churn rate among all internet service categories.
- Customers without Tech Support showed a higher churn rate than those who subscribed to the service.
- Customers without Online Security were more likely to churn than customers who had the service.
- The interactive Tableau dashboard enables users to filter and explore churn trends across multiple customer segments, supporting data-driven business decisions.

### Recommendation
Based on the analysis the followinng actions are recommended to reduce churn rain and increase customer retention
- Reduce month to month churn by offering incentives such as discounted pricing, loyalty rewards, or free service upgrades for customers who switch to one or twoyear contracts.
 Strengthen the first 12 months of the customer journey by implementing a structured onboarding program, welcome emails, check in calls, and proactive customer support to improve early retention.
- Review high monthly pricing plans and introduce flexible pricing tiers or bundled packages to increase perceived value for customers with higher monthly charges.
- Encourage customers to switch from Electronic Check to automatic payments by offering bill discounts, cashback, or reward points, as customers using automatic payment methods exhibit lower churn.
- Investigate the high churn among Fiber Optic customers by collecting customer feedback, monitoring service quality, and addressing network reliability or pricing concerns where necessary.
- Increase adoption of Tech Support and Online Security by bundling these services with internet plans, offering free trial periods, or providing discounted add-ons to customers who do not currently subscribe.
- Develop an early-warning retention program that flags customers with high-risk characteristics (e.g., month-to-month contracts, short tenure, high monthly charges, and no value-added services) so targeted retention campaigns can be launched before they churn.

 ### Limitations
- The analysis is based on a single historical dataset and may not reflect current customer behavior or market conditions.
- The dataset does not include information such as customer satisfaction, service quality, competitor activity, or reasons for churn, limiting the ability to explain why customers left.
- The analysis identifies relationships between variables and churn but does not establish causation.
- Customer segments were analyzed using grouped categories (e.g., tenure and monthly charges), which may mask more detailed patterns within individual customers.
- This project focuses on descriptive and exploratory analysis and does not include predictive modeling to forecast future customer churn.

### References
References

1. [IBM Telco Customer Churn Dataset. Kaggle.](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
2. [Tableau. Tableau Public Documentation.](https://help.tableau.com/)
3. [Microsoft. Microsoft Excel Documentation.](https://support.microsoft.com/excel)
