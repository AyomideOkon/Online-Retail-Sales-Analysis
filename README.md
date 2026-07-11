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
This project uses the Online Retail II dataset from the UCI Machine Learning Repository. The dataset contains transactional data from a UK based online retailer, including invoices, products, quantities, prices, customer IDs, and countries.

Link: [Online Retail II Dataset](https://archive.ics.uci.edu/dataset/502/online+retail+ii?utm_source=chatgpt.com)

### Tools
- Big Query - Data Cleaning And Analysis [Download here](https://www.google.com/aclk?sa=L&ai=DChsSEwiQ1ZSIuMiVAxU_pVAGHSVxOMgYACICCAEQARoCZGc&ae=2&co=1&ase=2&gclid=Cj0KCQjwsMLSBhD9ARIsAIpUTDoFp9zUz4I9oUIch82IuUSqthaIs-la6BhhicCi6buyLOHJEnon1dUaAnvHEALw_wcB&cid=CAASuwHkaHUeRtYPXh2-XjbEIyN5mV4IQKLM2_9v2gjeebigqXvALRuPxh_yca1mqxUbdCSr69V5SdMbETP3hR5eu-cBIG8sQMzz8RizvVVkmk6NjH4Ei9da0rgZFV79hmaOGYzop6X-ov-uQDc_rL12CMvYG5ffPPZEIXeKQbPw3EVRORBS_sTST01FuSEswphZQBKfxF7e7UUs2V_rI4XmrjMPDIMZgZUVNuLvp7dHXZ08VMwIWxvTL4Fk8FAy&cce=2&category=acrcp_v1_71&sig=AOD64_3t8n4aBzuueq7GyshaM7ubBGE5pw&q&nis=4&adurl&ved=2ahUKEwiouo6IuMiVAxWDe0EAHeeZOV8Q0Qx6BAgVEAE)
- Tableau - Data Visualization [Download here](https://www.tableau.com/products/public)

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
     COUNT(DISTINCT `CustomerID`) AS Total_Customers
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
Top 10 Products By Revenue
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
LIMIT 10
```
Top 10 Product By Quantity Sold
```sql
SELECT
     Description,
     SUM(Quantity) AS Quantity_Sold
FROM
     `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
GROUP BY
       Description
ORDER BY
       Quantity_Sold DESC
LIMIT 10
```
Top 10 Customers By Revenue Generated
```sql
SELECT
      `Customer ID`
      SUM(Price*Quantity) AS Total_Revenue
FROM
     `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
GROUP BY
       `Customer ID`
ORDER BY
       Total_Revenue DESC
LIMIT 10
```
Sales By Country
```sql
SELECT
   Country
   SUM(Price*Quantity) AS Total_Revenue
FROM
     `project-b019bf3e-9e2f-47ca-9af.online_retail_dataset.cleaned_retail_data`
GROUP BY
      Country
ORDER BY
       Total_Revenue DESC
LIMIT 10
   

FROM
     `porflio-projrct-1.ibm_churn_analysis.clean_churn_table` 
GROUP BY
      PaymentMethod
ORDER BY
       PaymentMethod;
```

### Reults/Findings
The analysis results are summarized as follows:
-The business generated £16.51 million in total revenue during the analysis period.
- A total of thousands of customer orders were completed, demonstrating consistent transaction activity throughout the period.
- Revenue fluctuated throughout the year, with November recording the highest monthly revenue, indicating strong seasonal demand during the holiday shopping period.
- The United Kingdom contributed the highest share of total revenue, making it the company’s primary market.
- Regency Cakestand 3 Tier generated the highest revenue, making it the most profitable product.
- World War 2 Gliders ASSTD Designs recorded the highest quantity sold, indicating strong customer demand.
- A small number of customers contributed a significant proportion of total revenue. The highest-value customers included 18102, 14646, 14156, 14911, and 17450.
- Sales were concentrated among a relatively small number of high performing products, while many products generated comparatively lower revenue.
- The monthly revenue trend showed clear seasonal fluctuations, suggesting that customer purchasing behavior varies throughout the year.
- Product demand differed from product profitability, demonstrating that the highest selling products were not always the highest revenue-generating products.
- The analysis highlighted opportunities for the business to strengthen inventory planning, customer retention strategies, and targeted marketing campaigns based on sales trends and customer purchasing behavior.

### Recommendation
Based on the analysis the followinng actions are recommended to reduce churn rain and increase customer retention
Recommendations

#### 1. Leverage Seasonal Demand
With sales peaking in November, the business should implement a seasonal sales strategy by increasing inventory levels, expanding fulfillment capacity, and launching targeted marketing campaigns several weeks before the holiday period. This will help maximize revenue while reducing the risk of stockouts and delayed deliveries.
#### 2. Strengthen Customer Retention
The analysis shows that a small group of customers contributes a significant share of revenue. The business should prioritize retaining these high value customers through loyalty programs, exclusive discounts, personalized product recommendations, and early access to promotions to increase customer lifetime value.
#### 3. Optimize Product Portfolio
While some products generated the highest revenue, others achieved high sales volumes without generating equivalent revenue. The company should evaluate low margin, high volume products to improve pricing, bundle complementary items, or renegotiate supplier costs to increase profitability.
#### 4. Improve Inventory Forecasting
Historical monthly sales trends should be incorporated into demand forecasting models to ensure inventory aligns with expected customer demand. Maintaining adequate stock for consistently high performing products while reducing inventory for slow moving items can lower storage costs and improve cash flow.
#### 5. Reduce Market Concentration Risk
Since the majority of revenue originates from the United Kingdom, the business should expand its presence in underperforming international markets through localized marketing campaigns, strategic partnerships, and region-specific promotions to diversify revenue streams and reduce dependence on a single market.

 ### Limitations
Limitations
-The dataset contains historical transaction data only and does not reflect current sales performance or recent customer behavior.
- Customer demographic information such as age, gender, income, and location is unavailable, limiting the ability to perform customer segmentation or targeted marketing analysis.
- The dataset does not include product cost or profit margin information, so the analysis focuses on revenue rather than profitability.
- External factors such as promotions, discounts, holidays, economic conditions, and competitor activities are not included, making it difficult to determine the drivers behind changes in sales performance.
- The analysis is based solely on completed transaction records and does not account for customer satisfaction, product reviews, or reasons for product returns.
- The business is heavily represented by sales from the United Kingdom, which may limit the generalizability of insights to other markets.
- The project analyzes historical sales trends but does not include predictive or forecasting models to estimate future demand.
- The analysis focuses on individual product performance and does not examine purchasing relationships between products (e.g., frequently bought together), which could provide additional opportunities for cross-selling and product bundling.

### References
1. [Kaggle. (n.d.). Online Retail II Dataset.](https://www.kaggle.com/datasets/dvaser/online-retail-ii)
2. [Google BigQuery Documentation](https://www.google.com/aclk?sa=L&ai=DChsSEwihoL_0ucuVAxVpj1AGHTAeHE4YACICCAEQABoCZGc&ae=2&co=1&ase=2&gclid=CjwKCAjw08fSBhA7EiwAfbQTsE5SAh_-hQor8DDo2vGFgBKODCF7oS_CNAQgnmztUuXmsqeJS7n6gBoCa4sQAvD_BwE&cid=CAASugHkaFKi8pNNE1PqqPFMOWrPG8ZHTPUw0v6bXesOulYRf7LQvcamtqPVqUio5blfa6bjFk1kS_bD__mPGdm5NcFEhVbPj5mIuNzfP2XuxfvSkJUs6CZAs10wcbuSM-3-Ws7FBRmjmyvCiV4NQyf11M3OgaGWKzBNzQ8JKnE3ALcsCf9Ij6OBenrpPqw0tIOx6EjEwE85JafxYo1Z77OGjY0eHje7w21PGQ9H59-rL1PFFUcOI0HaYELKAeE&cce=2&category=acrcp_v1_71&sig=AOD64_2h8Qq6R0VgpCJwvxOSo_YB6a47PA&q&nis=4&adurl&ved=2ahUKEwj817j0ucuVAxVRWkEAHZb8EfsQ0Qx6BAgQEAE)
2. [Tableau. Tableau Public Documentation.](https://help.tableau.com/)
   
