# [Olist E-Commerce Performance Analysis]
> *Uncovering the Drivers of Revenue, Customer Churn, and Logistics Inefficiencies to Drive Data‑Informed Business Decisions.*

---

## ⚙️ Project Type Flags

- Data Cleaning
- Exploratory Data Analysis (EDA)
- SQL Analysis
- Data Visualization

---

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [ERD - Entity Relationship Diagram](#7-erd--entity-relationship-diagram) *(SQL projects)*
8. [Analysis & Metrics](#8-analysis--metrics)
9. [Key Insights](#9-key-insights)
10. [Recommendations](#10-recommendations)
11. [Assumptions & Limitations](#11-assumptions--limitations)
12. [Future Enhancements](#12-future-enhancements)
13. [Deliverables](#13-deliverables)
14. [Author](#14-author)

---

## 1. Project Overview

Olist is a Brazilian marketplace connecting sellers to customers across multiple product categories. As an online platform, it faces common e‑commerce challenges: understanding revenue drivers, reducing customer churn, optimizing delivery logistics, and identifying profitable product segments. Part of he analysis of this project revealed that the over-estimation of the delivery days prompted some of the customers to cancel their orders which caused the business a potential profit. This project was motivated by the need to turn raw transactional data into actionable business intelligence.

**Problem Statement:** How can Olist increase revenue, improve customer retention, and reduce logistics inefficiencies?  
Specifically:  
- Which products and regions drive revenue?  
- Why do customers not return after their first purchase?  
- What is causing orders cancellations by customers?
- How do freight costs vary by region and product category, and what does that imply for profitability?


**Approach:** I performed end‑to‑end analysis using SQL (MySQL Workbench) for data extraction, cleaning and analysis, Power BI for interactive dashboards. The project included RFM segmentation, geospatial distance calculations, and a detailed review of sales, customer behaviour, product performance, logistics, and payment patterns. All filtered to delivered orders for accurate revenue metrics.

**Outcome:** 
- **Sales & Revenue:** Identified top‑performing products and regions, discovered that revenue growth stalled in late 2018 due to no data, with R$95,235 lost to cancellations.
- **Customer Behaviour:** Conversion rate at 10%, 90% churn rate. All customers are one‑time buyers; RFM segmentation revealed “High‑Value New” and “At Risk” segments for targeted retention.
- **Logistics:** 93% on‑time delivery but over‑estimated delivery days caused R$10,650 freight revenue loss. Sellers concentrated in Sao Paulo, leading to more than 20-day deliveries and high freight costs in remote regions.
- **Actionable Recommendations:** Adjust delivery estimation algorithm, launch loyalty programs, rationalize product portfolio, and recruit sellers in underserved areas.


---

## 2. Objectives

- **Primary Objective:** To identify the key drivers of revenue, customer churn, and logistics inefficiencies in the Olist marketplace
- **Secondary Objective 1:** To quantify sales performance
- **Secondary Objective 2:** To evaluate customer retention and logistics effectiveness

---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|-----------|---------|
| **In Scope** | Olist Brazilian E‑commerce Dataset (public), Analysis covers Revenue, Customer behaviour, Logistics and Product category performance |
| **Out of Scope** | Sellers' profitability, Customer demographics and Marketing spend data were excluded |
| **Time Period** | Sep 2016 - Oct 2018 |
| **Granularity** | order_items (each product in an order), reviews (each review).  **Order‑level:** orders (each order), payments (each payment method). **Customer‑level:** customers (for RFM and churn).  **monthly aggregates** for time‑series charts (revenue trends, growth rates). |

### Tools & Technologies

| Category | Tool(s) Used |
|----------|-------------|
| Data Storage | CSV files |
| Data Processing | SQL, Excel |
| Analysis | SQL queries |
| Visualization | Power BI |
| Version Control | GitHub |
| Documentation | Markdown |

---

## 5. Data Workflow

Data Source
      >
Ingestion
      >
Cleaning & Transformation
      >
Analysis & Modelling
      >
Visualisation & Reporting

1. **Source:**  The Olist Brazilian E‑commerce dataset (publicly available on Kaggle).
                 **Format:** 9 interconnected CSV files.
                **Tables used:** orders, order_items, products, customers, sellers, geolocation, order_payments, order_reviews, marketing_qualified_leads, closed_deals.  
                **Time period:** September 2016 – October 2018.

2. **Ingestion:** **CSV → SQL:** CSV files were imported into MySQL Workbench using LOAD IMPORT WIZARD.  
                  Also loaded into Power BI via “Get Data → Text/CSV” 

3. **Cleaning:** **Missing dates:** Replaced `"NULL"` with `n/a` in delivery date columns.  
                 **Data types:** Converted price, freight_value, payment_value to DECIMAL; date columns to DATETIME.  
                 **Duplicates:** Removed duplicate order rows.  
                 **Null categories:** Filled empty product_category_name with `"n/a"`.  
                 **Outliers:** Flagged orders with price = 0 or negative for investigation (excluded from revenue metrics).

4. **Transformation:** DeliveryDays (delivered date – purchase date)   EstDeliveryDays (estimated delivery date – purchase date)  
   -Distance_km (Haversine formula between seller and customer geolocations)  
   - Revenue R$ - InstallmentGroup (1 = “Full payment”, 2‑3 = “Short term”, 4‑6 = “Medium”, 7-12 = “Long”, 13+ = “Extended”)  
   - **Aggregated tables:**  
   - RFM table (customer‑level: Recency, Frequency, Monetary)  
   - SalesMonthly (revenue, orders, AOV by month)  
   - CategoryPerformance (revenue, units, freight % by product category)  
   - **Star schema:** Built Date table related to orders on order_purchase_timestamp.

5. **Analysis:** **Exploratory Data Analysis (EDA):** Distribution plots, time series (SQL + Power BI).  
   - **RFM segmentation:** Ntile (Recency, Frequency, Monetary) to identify customer segments.
   - **Geospatial analysis:** Distance calculation to compare estimated delivery days vs. actual distance (scatter plot).  
   - **Statistical summaries:** Median, Ntiles, averages for delivery times, freight costs, review scores.
   - **Business KPI measures (DAX):**  - Total Revenue, Total Orders, AOV, OnTimeDeliveryRate, Churn Rate, Revenue per Lead, etc.  
   - **Hypothesis testing:** Proved that over‑estimated delivery days for short distances drive cancellations in São Paulo.

6. **Output:** **Interactive Power BI dashboard** (4 pages) 
  - Executive Summary (KPIs, revenue trend, top products)  
  - Sales & Revenue (monthly / yearly, category, region)  
  - Customer Behaviour (RFM segments, churn rate, conversion funnel)  
  - Product Performance (price vs. volume matrix, review scores)  
  - Logistics & Delivery (on‑time rate, map of delivery days, freight % by region/category)  
  - Payment & Marketing (payment distribution, conversion rate by channel, revenue per lead)  
  - **SQL scripts** (GitHub) – all queries used for extraction, cleaning, and analysis.  
  - **Executable DAX measures** (ready to copy into any Power BI model).  
  - **This documentation** – complete pipeline, findings, and recommendations.

---

## 7. ERD - Entity Relationship Diagram

(https://github.com/EritosinSalami/Olist/blob/main/visuals/ERD.png)

**Core schema of the Olist dataset** – orders as the central fact table connected to customers, payments, reviews, and order items, which join to products and sellers. Geolocation links to customers and sellers via zip codes. Marketing leads join to closed deals.

---

## 9. Key Insights

**Insight 1: Revenue is volume‑driven, not price‑driven – most products are low‑price, low‑volume.**

 **Findings:** A scatter plot of price vs. quantity (bubble size = total revenue) showed that the vast majority of products cluster in the bottom‑left quadrant (low price, low volume). Only a handful of products drive revenue through high volume (bottom‑right) or high price (top‑left). The top 10 products by revenue and units sold account for a disproportionate share of sales.

 **Meaning:** Olist’s catalog is dominated by slow‑moving, low‑value items. The business relies on a few “hero” products. This creates vulnerability: if those products face stockouts or competition, overall revenue could drop significantly. Rationalising the portfolio, discontinue or discount bottom‑left products, promote heavy‑hitters, and experiment with bundling to move volume.


**Insight 2: 90% of customers never return, zero repeat purchases, over‑estimated delivery days and geographic concentration.**

**Findings:** Churn rate is 90% (customers with no purchase in the last 90 days). All customers are one‑time buyers. Cancellations in Sao Paulo (the largest market) are strongly correlated with estimated delivery days being far too high, even when the seller is geographically close. Scatter plot of distance (km) vs. estimated delivery days shows a cluster of canceled orders at short distance (<500 km) with high estimates (>15 days).

 **Meaning:** The delivery estimation algorithm is broken for short distances. Customers trust the platform but are forced to cancel when they see unrealistic long promises. The lack of repeat purchases also signals no loyalty programme, no post‑purchase engagement, and no incentive to return. Fixing the estimate logic (e.g., reduce to 3‑5 days for short distances) could recover a significant portion of lost revenue and potentially improve retention.


**Insight 3: Freight costs eat disproportionately into revenue for remote regions and heavy product categories.**

**Findings:** Freight cost as a percentage of product price is 2× higher in northern states (AM, RR, PA) than in Sao Paulo, even for identical products. Heavy categories (furniture, electronics) have freight percentages >20% in remote areas. Despite that, sellers are heavily concentrated in Sao Paulo, forcing long, expensive shipments.

**Meaning:** The current logistics model is unfair to both customers and sellers in remote regions. Olist is missing out on potential demand because shipping is prohibitively expensive and slow. Opening regional fulfilment centres (e.g., Manaus, Fortaleza) and incentivising local sellers could slash delivery times and freight costs, making those markets profitable.


**Insight 4: Credit cards dominate, but 52% of orders use instalments and long‑term instalments carry higher default risk.**

**Findings:** 75% of orders use credit cards, and 52% of orders are paid in instalments (1‑12+ instalments). Orders with 7+ instalments (12% of total) have 40% higher average order value but also show a higher rate of cancelled payments (as inferred from payment approval delays and cancellations). Full payment (1 instalment) accounts for 48% of orders.

**Meaning:** While instalments drive higher basket sizes, they also introduce financial risk. Olist should implement tiered fraud checks: flag orders with >6 instalments, high value, and new accounts for manual review. Also, offering a small discount for full payment could improve cash flow and reduce default exposure. The data supports that most customers can afford to pay upfront – 48% already do.

---

## 10. Recommendations

<!--
  Action-oriented. Addressed to a real audience.
  Tied explicitly to the insight that supports each one.

  WHAT GOOD LOOKS LIKE:
  Priority: High
  Recommendation: "Conduct a fulfilment audit for home goods deliveries
                   in Region A - specifically investigating whether returns
                   correlate with a particular warehouse, carrier, or SKU batch."
  Based On: Insight 1 - return rate anomaly in Region A
  Owner: Operations / Supply Chain team

  WHAT TO AVOID:
  ❌ "Improve the return rate."
     (Not actionable. Doesn't say who, how, or where to start.)
  ❌ "Further analysis is needed."
     (This is a placeholder, not a recommendation.)
-->

| Priority | Recommendation | Based On | Suggested Owner |
|----------|---------------|----------|-----------------|
| High | [Specific, actionable step] | [Insight it comes from] | [Who should act] |
| Medium | [Specific, actionable step] | [Insight it comes from] | [Who should act] |
| Low | [Exploratory or longer-term suggestion] | [Insight it comes from] | [Who should act] |

---



## 13. Deliverables

| Deliverable | Description | Location |
|-------------|-------------|----------|
| [Name] | [What it contains] | [`/path/to/file`] |
| [Name] | [What it contains] | [`/path/to/file`] |
| [Name] | [What it contains] | [`/path/to/file`] |

---

## 14. Author

**[Eritosin Salami]**
[Data Analyst]

- 🔗 [www.linkedin.com/in/eritosin-salami]
- 💼 [https://github.com/EritosinSalami]
- 📧 [salamieritosinlearn@gmail.com]

---

*Last updated: [Month YYYY]*
*If this template helped you, consider starring the repository.*
