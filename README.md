# ShopNest-Cloud-Analytics-Platform

**End-to-End Cloud Analytics Pipeline | AWS · Redshift · Power BI**

Transforming fragmented e-commerce data into trusted business insights through a fully automated, event-driven analytics platform.

---

#  Project Snapshot

| Metric | Value |
|---------|-------|
| Transactional Records Processed | **5M+** |
| Source Tables | **7** |
| AWS Services Used | **7** |
| Dashboard Pages | **6** |
| Data Range | **2020 — 2026** |


---

# Business Challenge

ShopNest is an e-commerce retailer that manages customers, products, orders, payments, shipments, and warehouse operations across its business ecosystem.

As the volume of operational data increased, information became fragmented across multiple systems, making reporting slower, less reliable, and increasingly difficult to manage.

## Key Challenges

- Business data was scattered across multiple operational systems.
- Manual data preparation delayed reporting and business insights.
- Decision-makers lacked a single, trusted source for analytics.

---

# Solution

To address these challenges, I designed and built an end-to-end cloud analytics platform that automates the complete analytics workflow — from data processing to interactive business dashboards.

## Platform Capabilities

- Automates data validation, transformation, and warehouse loading.
- Executes an event-driven ETL workflow with AWS serverless services.
- Builds a centralized dimensional data warehouse for analytics.
- Delivers interactive Power BI dashboards for business reporting and decision-making.

<p align="center">
<img src="Shopnest architecture.png" alt="Solution Architecture" width="100%">
</p>

---

# End-to-End Workflow

### **1. Raw Data Upload**
Source datasets — Customers, Products, Orders, Order Items, Payments, Shipments, and Warehouses — are uploaded to the Amazon S3 Raw Zone.

⬇️

### **2. Event Trigger**
Amazon S3 Event Notification automatically invokes an AWS Lambda function, which starts the corresponding AWS Glue ETL job.

⬇️

### **3. Data Processing**
AWS Glue extracts the raw CSV files, performs data validation, cleansing, standardization, data type conversion, and quality checks before generating a unique Batch ID.

⬇️

### **4. Parquet Conversion**
The cleaned data is converted into Parquet format and stored in the Amazon S3 Processed Zone using batch-specific folders.

⬇️

### **5. Batch Registration**
The latest processed batch path is stored in AWS Systems Manager Parameter Store, enabling downstream processes to always access the newest dataset.

⬇️

### **6. Warehouse Loading**
After the Glue job completes successfully, Amazon EventBridge triggers a second AWS Lambda function, which retrieves the latest batch path and loads the processed Parquet files into Amazon Redshift staging tables using the COPY command.

⬇️

### **7. Audit Monitoring**
Each successful table load is recorded in the Audit Load Status table, capturing the batch ID, table name, load status, and timestamp.

⬇️

### **8. Data Warehouse Population**
Once all staging tables are successfully loaded, Amazon Redshift stored procedures populate the Star Schema Data Warehouse, including SCD Type 2 implementation for Customer and Product dimensions before loading the fact tables.

⬇️

### **9. Business Intelligence**
Power BI connects directly to Amazon Redshift Serverless using DirectQuery, where the semantic model, DAX measures, KPIs, and interactive dashboards are built.

⬇️

### **10. Report Publishing**
The completed Power BI report is published to the Power BI Service, enabling business users to access interactive dashboards and enterprise reporting through the web.

---

# Analytics Views & Business Insights

## Customer Analytics

Provides insights into customer acquisition, loyalty, engagement, and revenue contribution to improve customer retention.

<p align="center">
<img src="Customer analysis.png" alt="Solution Architecture" width="100%">
</p>


### Business Value

- Analyze customer growth, repeat purchases, loyalty, and purchasing behavior.
- Identify high-value customer segments and inactive customers.
- Measure customer engagement through purchase frequency and activity status.

###  Insights

- 68% of customers are inactive for over 180 days → Launch targeted win-back campaigns segmented by last purchase category.
- Top 20% of customers contribute 37% of total revenue → Build a loyalty program targeting this segment to protect revenue concentration.

---

## Sales Performance

Provides visibility into revenue, profitability, product sales, payment behavior, and sales trends.

<p align="center">
<img src="Sales performance.png" alt="Solution Architecture" width="100%">
</p>


### Business Value

- Monitor revenue, profit, orders, and sales growth.
- Analyze product category performance and payment preferences.
- Support operational and strategic sales decisions.

###  Insights

- ₹482M lost due to cancellations and returns → Investigate return patterns by category and tighten return policy.
- UPI accounts for 40% of payments while COD remains at 22% → Incentivize digital payments to reduce cash handling operational costs.

---

## Product Analytics

Provides product-level insights into sales performance, profitability, pricing, and category contribution.

### Dashboard Overview

<p align="center">
  <img src="Product analysis view.png" alt="Product Analytics Dashboard" width="100%">
</p>

### Interactive Product Tooltip

Demonstrates a custom tooltip that provides additional context by categorizing products into **Star Products**, **Hidden Gems**, **Margin Concerns**, and **Underperformers** without cluttering the main dashboard.

<p align="center">
  <img src="Product analysis with tooltip.png" alt="Product Analytics Tooltip" width="100%">
</p>

### Business Value

- Identify best-selling and highest-profit products.
- Evaluate category profitability and pricing effectiveness.
- Analyze product performance through interactive drill-downs and custom tooltips.
- Classify products into **Star Products**, **Hidden Gems**, **Margin Concerns**, and **Underperformers** for faster business decisions.

###  Insights

- Bottom-performing products generate less than ₹19K in revenue → Consider discontinuation or repositioning to free up inventory capital.
- Paper Boat Aamras ranks among the highest-profit products despite lower revenue → Increase visibility and promotions — untapped hidden gem.

---

## Logistics & Operations

Provides operational visibility into shipment performance, delivery efficiency, and shipping costs.

<p align="center">
<img src="Logistics and operation.png" alt="Solution Architecture" width="100%">
</p>

### Business Value

- Monitor shipment volume, delivery performance, and logistics costs.
- Analyze shipping mode distribution and warehouse fulfillment efficiency.
- Identify delivery delays affecting customer satisfaction and retention.

###  Insights

- 21.35% of shipments (78K) are delayed → Audit bottom-performing warehouses and reroute to faster shipping modes.
- Express shipping at 34% share is driving ₹47.41M in total shipping cost → Negotiate bulk rates or shift mid-distance routes to Two-Day to reduce spend.

---


## Executive Overview

Provides executives with a consolidated view of business performance by monitoring financial KPIs, growth trends, and category performance.

<p align="center">
<img src="Executive view.png" alt="Solution Architecture" width="100%">
</p>

### Business Value

- Monitor revenue, profit, profit margin, customer loyalty, and business growth.
- Identify top-performing products and revenue-driving categories.
- Track YoY and MoM performance for strategic planning.

###  Insights

- Revenue grew 37.9% YoY but profit margin remained flat at 27.23% → Review pricing and discount strategies to improve profitability.
- Grocery drives the highest revenue while Fashion delivers stronger margins → Increase investment in high-margin categories to convert growth into profit.

---

# Business Impact

The analytics platform transforms raw operational data into actionable business insights by providing a centralized, automated, and analytics-ready reporting environment.

- Establishes a single source of truth for business reporting.
- Eliminates repetitive manual ETL and reporting processes through automation.
- Enables faster access to KPIs, trends, and operational insights.
- Supports data-driven decision-making across sales, customers, products, and logistics.
- Provides a scalable foundation for future analytics initiatives.

---

# Engineering Decisions

The following architectural decisions were made to build a scalable, maintainable, and analytics-focused platform while keeping the project aligned with real-world data engineering practices.

### Manual Source Data Upload

In production, data would typically be ingested directly from operational systems such as web applications, ERP systems, or transactional databases. Since this project focuses on the analytics platform rather than source-system integration, datasets are manually uploaded to Amazon S3 to simulate production data. From that point onward, the entire pipeline executes automatically.

### Event-Driven Architecture

The pipeline is orchestrated using Amazon S3 Event Notifications, AWS Lambda, and Amazon EventBridge instead of manual execution. This ensures every processing stage starts automatically only after the previous stage completes successfully.

### Parquet over CSV

Raw CSV files are transformed into Parquet format during the ETL process. Parquet provides columnar storage, better compression, faster analytical queries, and lower storage costs, making it ideal for analytics workloads.

### AWS Systems Manager Parameter Store

The latest processed batch path is stored in AWS Systems Manager Parameter Store rather than being hardcoded. This allows downstream processes to dynamically identify and load only the most recent processed dataset.

### Audit-Driven Pipeline Control

An Audit Load Status table tracks the loading status of every staging table, including the batch ID, table name, status, and timestamp. Warehouse loading begins only after all required datasets have been successfully loaded, improving pipeline reliability and traceability.

### Star Schema Data Warehouse

Amazon Redshift follows a Star Schema design consisting of dimension and fact tables. This structure simplifies analytical queries, improves reporting performance, and provides a clean semantic model for Power BI.

### Slowly Changing Dimension (SCD Type 2)

Customer and Product dimensions implement SCD Type 2 to preserve historical attribute changes, enabling historical reporting and trend analysis without overwriting previous business records. dim_warehouse uses SCD Type 1 as warehouse attribute changes represent corrections, not meaningful historical events.

### Power BI DirectQuery

Power BI connects directly to Amazon Redshift Serverless using DirectQuery, ensuring dashboards always reflect the latest warehouse data without requiring dataset imports or scheduled refreshes.

---

# Technology Stack

| Component | Technology |
|-----------|------------|
| Cloud Platform | Amazon Web Services (AWS) |
| Object Storage | Amazon S3 |
| ETL Processing | AWS Glue |
| Workflow Orchestration | AWS Lambda, Amazon EventBridge |
| Configuration Management | AWS Systems Manager Parameter Store |
| Data Warehouse | Amazon Redshift Serverless |
| Storage Format | CSV, Parquet |
| Data Modeling | Star Schema, SCD Type 1 & Type 2 |
| Query Language | SQL, PL/pgSQL (Stored Procedures) |
| Business Intelligence | Microsoft Power BI, DAX |

---

# What This Project Demonstrates

This project demonstrates my readiness for an Analytics Engineer role through the following:

- Designing and implementing an end-to-end cloud analytics platform on AWS.
- Building automated, event-driven ETL pipelines using serverless services.
- Processing and transforming large-scale datasets into analytics-ready formats.
- Designing a dimensional data warehouse using Star Schema and SCD Type 2.
- Developing SQL stored procedures for automated warehouse loading and orchestration.
- Creating interactive Power BI dashboards using advanced DAX, dynamic titles, KPI cards, bookmarks, page navigation, toggle switching, drillthrough, custom tooltips, slicers, cross-filtering, conditional formatting, and time intelligence.
- Delivering business-focused dashboards that support data-driven decision-making across sales, customers, products, and logistics.
- Applying analytics engineering best practices to build scalable, reliable, maintainable, and automated analytics solutions.

---

# ShopNest Analytics Platform

**Built with Python · SQL · AWS · Power BI**
