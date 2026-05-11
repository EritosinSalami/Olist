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

## 5. Data Workflow

[Data Source]
      >
[Ingestion]
      >
[Cleaning & Transformation]
      >
[Analysis & Modelling]
      >
[Visualisation & Reporting]

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

## 7. ERD - Entity Relationship Diagram

(https://github.com/EritosinSalami/Olist/blob/main/visuals/ERD.png)
*Core schema of the Olist dataset – orders as the central fact table connected to customers, payments, reviews, and order items, which join to products and sellers. Geolocation links to customers and sellers via zip codes. Marketing leads join to closed deals.*

## 8. Analysis & Metrics

<!--
  Explain what you measured and how - before you share what you found.

  WHAT GOOD LOOKS LIKE:
  Metric: "Customer Return Rate"
  Definition: "Number of transactions flagged as returns divided by total
               transactions, calculated at product-category and regional grain."
  Why It Matters: "Return rate - not sales volume - was hypothesised to
                  explain regional revenue gaps. This metric tests that hypothesis."

  WHAT TO AVOID:
  ❌ Defining a metric only in code: SUM(returns) / COUNT(transaction_id)
     That's an implementation. Write the plain-language definition here.
     Both belong in your project - the definition in the README,
     the implementation in the code.
-->

### Analytical Approach

[Describe how you approached the analysis. Were you exploring patterns? Testing a hypothesis? Building and validating a pipeline? Be honest about your method - exploratory work is valid, just call it that.]

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|--------|--------------------------|----------------|
| `[Metric 1]` | [What it measures, in one sentence] | [What decision or question it answers] |
| `[Metric 2]` | [What it measures, in one sentence] | [What decision or question it answers] |
| `[Metric 3]` | [What it measures, in one sentence] | [What decision or question it answers] |

### Methods Used

- [e.g., Descriptive statistics - distribution, central tendency, outlier detection]
- [e.g., Trend analysis across [time period]]
- [e.g., Segmentation / group comparison by [dimension]]
- [e.g., Correlation analysis between [variable A] and [variable B]]
- [e.g., SQL window functions for [specific aggregation]]
- [e.g., Custom aggregation or transformation logic in [tool]]

---

## 9. Key Insights

<!--
  Findings + implications. Not just what happened - what it means.

  WHAT GOOD LOOKS LIKE:
  ✅ "Return rates, not sales volume, explain Region A's underperformance.
      Region A's return rate on home goods was 34% - more than double the
      company average. Revenue was not lost at the point of sale; it was
      lost post-sale through refunds. This points to a fulfilment or
      product quality issue specific to that region, not a demand problem."

  WHAT TO AVOID:
  ❌ "Region A had lower revenue than other regions in Q4."
     (That's an observation. It describes what happened.
      An insight says what it means and where to look next.)

  Aim for 3–6 insights. Quality over quantity.
-->

**Insight 1: [Short descriptive headline]**
[What you found + what it suggests. One short paragraph.]

**Insight 2: [Short descriptive headline]**
[What you found + what it suggests.]

**Insight 3: [Short descriptive headline]**
[What you found + what it suggests.]

**Insight 4 (if applicable): [Short descriptive headline]**
[What you found + what it suggests.]

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

## 11. Assumptions & Limitations

<!--
  WHAT GOOD LOOKS LIKE:
  Assumption: "Transaction records were assumed to be complete for all five regions.
               No validation was performed against source system record counts."
  Limitation: "The analysis cannot distinguish between returns initiated by
               the customer vs. returns initiated by the business (e.g., recalls).
               If business-initiated returns are concentrated in Region A, the
               return rate finding may reflect a policy decision, not a quality issue."

  WHAT TO AVOID:
  ❌ Leaving this section blank or writing "None known."
     Every project has limitations. Documenting them is a sign of
     analytical maturity - not a confession of failure.
-->

### Assumptions
- [What did you treat as true without being able to verify?]
- [What simplifications did you make for scope or feasibility?]
- [What domain rules or definitions did you accept as given?]

### Limitations
- [What gaps exist in the data?]
- [What analysis was out of scope but could affect interpretation?]
- [What would a more rigorous version of this project include?]
- [Are there known biases in the data source or collection method?]

> *The goal here is pre-emptive Q&A. What would a thoughtful skeptic push back on? Document the answer here, before they ask.*

---

## 12. Future Enhancements

<!--
  WHAT GOOD LOOKS LIKE:
  ✅ "Automate the monthly data pull from the POS export folder using
      a scheduled Python script, replacing the current manual process."
  ✅ "Expand the return rate analysis to include carrier-level data,
      which was unavailable in this dataset but exists in the logistics system."

  WHAT TO AVOID:
  ❌ "Add a machine learning model."
     (Vague, and disconnected from the actual findings of this project.)
  ❌ Listing aspirational features that don't follow logically from the work.
-->

- [ ] [Enhancement 1 - specific and traceable to a real gap in this project]
- [ ] [Enhancement 2]
- [ ] [Enhancement 3]
- [ ] [Enhancement 4]

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
