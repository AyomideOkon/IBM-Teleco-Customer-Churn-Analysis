# IBM-Teleco-Customer-Churn-Analysis

### Project Overview

This project analyzes customer churn using the IBM Telco Customer Churn dataset to identify the key factors influencing customer retention and attrition. The analysis was performed using SQL for data cleaning, Excel for KPI calculations, and Tableau for building an interactive dashboard. The objective of the project is to uncover the main drivers of churn, visualize customer behavior, and provide actionable insights that can help businesses improve customer retention and make data-driven decisions.

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
    SUM(CASE WHEN Churn = true THEN 1 else 0 END)* 100/ count(*),2) AS churn_rate,
    SUM (CASE WHEN Churn = true THEN 1 else 0 END) AS Total_churn
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
    SUM(case WHEN Churn = true THEN 1 ELSE 0 END) ASS Total_churn
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





  
