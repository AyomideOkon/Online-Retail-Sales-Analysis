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

The dataset used in this project is the IBM Telco Customer Churn dataset, obtained from Kaggle. It contains customer demographic, service, billing, and churn information for 7,043 telecom customers. Dataset:https://www.kaggle.com/datasets/blastchar/telco-customer-churn

### Tools
- Excel- Data Cleaning [Download here](https://www.microsoft.com/en/microsoft-365/excel)
- SQL- Data Analysis [Download here](https://www.google.com/aclk?sa=L&ai=DChsSEwiQ1ZSIuMiVAxU_pVAGHSVxOMgYACICCAEQARoCZGc&ae=2&co=1&ase=2&gclid=Cj0KCQjwsMLSBhD9ARIsAIpUTDoFp9zUz4I9oUIch82IuUSqthaIs-la6BhhicCi6buyLOHJEnon1dUaAnvHEALw_wcB&cid=CAASuwHkaHUeRtYPXh2-XjbEIyN5mV4IQKLM2_9v2gjeebigqXvALRuPxh_yca1mqxUbdCSr69V5SdMbETP3hR5eu-cBIG8sQMzz8RizvVVkmk6NjH4Ei9da0rgZFV79hmaOGYzop6X-ov-uQDc_rL12CMvYG5ffPPZEIXeKQbPw3EVRORBS_sTST01FuSEswphZQBKfxF7e7UUs2V_rI4XmrjMPDIMZgZUVNuLvp7dHXZ08VMwIWxvTL4Fk8FAy&cce=2&category=acrcp_v1_71&sig=AOD64_3t8n4aBzuueq7GyshaM7ubBGE5pw&q&nis=4&adurl&ved=2ahUKEwiouo6IuMiVAxWDe0EAHeeZOV8Q0Qx6BAgVEAE)
- Tableau- Data Visualization [Download here](https://www.tableau.com/products/public)

### Data Cleaning/Preparation
The dataset was cleaned and prepared in Microsoft Excel before analysis. The following data cleaning steps were performed:
- Checked the dataset for duplicate records and removed duplicates where necessary.
- Identified and handled missing or blank values.
- Standardized data formats to ensure consistency across all columns.
- Verified data types and ensured numerical fields were correctly formatted.
- Checked for inconsistencies and data entry errors.
- Verified the accuracy of the cleaned dataset before analysis.

### Exploratory Data Analysis
The exploratory analysis focused on identifying the key factors influencing customer churn. The analysis included:
- Calculating the overall customer churn rate.
- Comparing churn rates across different contract types.
- Analyzing churn by customer tenure groups.
- Examining the relationship between monthly charges and churn.
- Evaluating churn across different payment methods.
- Comparing churn rates by internet service type.
- Analyzing the impact of Tech Support and Online Security on customer churn.
- Comparing churn across customer demographics such as Senior Citizen status and Gender.
- Identifying patterns and trends to uncover the primary drivers of customer churn.

### Data Analysis Query
Churn Rate
  ```sql
  SELECT
     SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END) * 100 / COUNT(*) AS churn_rate
  FROM 
    `porflio-projrct-1.ibm_churn_analysis.clean_churn_table`
  ```
  
Total customers 
```sql
SELECT
     COUNT(*)
FROM 
    `porflio-projrct-1.ibm_churn_analysis.clean_churn_table`
```
Churned Customers
```sql
SELECT
     COUNTIF(Churn, "true")
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table`
```
Retained Customers
```sql
SELECT
     COUNTIF(Churn, "false")
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table`
```
Churn by contract 
```sql
SELECT
    Contract,
    COUNT(*) total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ count(*) AS Churn_rate,
    SUM (CASE WHEN Churn = true then 1 ELSE 0 END) AS Total_churn
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
       Contract
ORDER BY 
       Churn_rate desc;
```
Churn Rate By Tenure Group
```sql
SELECT
    CASE
    WHEN tenure >= 0 AND tenure < 12 THEN '0-12'
    WHEN tenure >= 12 AND tenure < 24 THEN '12-24'
    WHEN tenure >= 24 AND tenure < 36 THEN '36-48'
    WHEN tenure >= 48 AND tenure < 60 THEN '48-60'
    ELSE '60+'
    END as Tenure_Group,
    COUNT(*) total_customers,
    SUM(CASE WHEN Churn = true THEN 1 ELSE 0 END)* 100/ count(*),2) AS churn_rate,
    SUM (CASE WHEN Churn = true THEN 1 ELSE 0 END) AS Total_churn
FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
       Tenure_Group
GROUP BY
       Tenure_Group ;
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
