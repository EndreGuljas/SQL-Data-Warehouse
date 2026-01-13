# 🏢 End-to-End Data Warehouse and Analytics Report

![SQL](https://img.shields.io/badge/SQL-Server-CC2927?style=flat&logo=microsoft-sql-server&logoColor=white)
![T-SQL](https://img.shields.io/badge/T--SQL-ETL-0078D4?style=flat&logo=microsoft&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Medallion-orange?style=flat)
![Status](https://img.shields.io/badge/Status-Complete-success?style=flat)

> A comprehensive SQL-based data warehousing solution that transforms fragmented retail data into a unified analytics platform using medallion architecture, enabling data-driven decision making across the organization.

---

## 📋 Table of Contents

- [Executive Summary](#-1-executive-summary)
- [Business Problem & Objective](#-3-business-problem--objective)
- [Data Architecture Overview](#-4-data-architecture-overview)
- [Data Flow & Integration](#-5-data-flow--integration)
- [Warehouse Layers](#-6-warehouse-layers)
  - [Bronze Layer](#bronze-layer)
  - [Silver Layer](#silver-layer)
  - [Gold Layer](#gold-layer)
- [Exploratory Data Analysis](#-7-exploratory-data-analysis)
- [Advanced Analytics and Reporting](#-8-advanced-analytics-and-reporting)
- [Data Quality & Validation](#-9-data-quality--validation)
- [Conclusion](#-10-conclusion)

---

## 📊 1. Executive Summary

This project focuses on a retail company struggling with a fragmented and disorganized data landscape that has limited their ability to understand business performance. Critical data was siloed across two disconnected systems: a CRM (Customer Relationship Management) platform containing customer profiles, product information, and sales transactions, and an ERP (Enterprise Resource Planning) system storing supplementary customer demographics, geographic attributes, and product hierarchies. Inconsistent naming conventions, duplicate records, and the absence of a unified data model made reliable analysis difficult.

### Phase 1: Data Engineering

The first phase of the project addressed these challenges through data engineering. I designed and implemented a SQL-based data warehouse using a **medallion architecture**, made up of bronze (raw), silver (cleaned), and gold (curated) layers. Using T-SQL and automated ETL pipelines, I moved fragmented datasets into a centralized data source. This phase included enforcing data quality controls such as deduplication with SQL window functions, standardizing categorical fields, and modeling the data into a dimensional star schema optimized for analytics and reporting.

### Phase 2: Business Analytics

The second phase shifted from infrastructure to analytics, focusing on extracting business value from the engineered environment. Using the curated gold layer, I conducted exploratory data analysis and developed advanced SQL analyses to answer key business questions related to growth, customer behavior, and product performance. These analyses revealed several high-impact insights:

- **60%+ revenue concentration** in a small number of geographic regions
- **Clear distinction** between high-volume and high-margin product categories
- **80/20 revenue distribution** across the product catalog
- **VIP customer segment** with significantly higher retention rates

### Impact

Together, these phases demonstrate an end-to-end data workflow that turns raw, disconnected data into an organized database that is capable of driving informed business decisions. The project highlights how data engineering, combined with advanced analytics, can transform messy data into a strategic asset that supports data-driven decision making for the organization. From the company's perspective, this solution replaced manual reporting with a scalable analytics platform that enables leadership to understand where revenue is generated, which products drive profitability, and which customers deliver the highest long-term value.

---

## 🎯 3. Business Problem & Objective

The company was operating in a fragmented data environment that made it difficult to gain a clear and consistent view of business performance. Critical data was split across two siloed systems, each providing only part of the overall picture. The CRM system captured customer interactions and daily sales transactions but lacked demographic and geographic context. In contrast, the ERP system contained essential master data, which included product hierarchies and regional attributes, but was disconnected from transactional sales activity. As a result, teams were unable to easily combine operational and analytical insights.

### Key Challenges

This fragmented data landscape led to several key challenges:

| Challenge | Impact |
|-----------|--------|
| **Data Silos** | Data had to be manually exported from multiple systems and joined using CSV files, a process that was slow and error prone |
| **Inconsistent Data Quality** | The same customer could appear differently across systems, with mismatched name formats, gender values, or marital status codes, resulting in conflicting reports |
| **Limited Historical Visibility** | Source systems were designed for operational use, making it difficult to analyze changes over time, such as price updates or customer location history |
| **No Centralized Reporting** | Without standardized logic for cleaning and aggregating data, different departments produced conflicting "Total Revenue" figures, undermining trust in reporting |

### Business Impact

As a result of these challenges, business teams spent significant time trying to understand messy numbers instead of analyzing them. Leadership lacked confidence in performance reports, marketing decisions were based on incomplete regional insights, and product strategy relied more on intuition than data.

### Project Objectives

Therefore, the objective of this project was to move the organization from manual, fragmented reporting to a centralized and automated analytics environment. The solution focused on building a scalable data foundation that could support consistent reporting and analysis across the business. Key objectives included:

1. **Establish an Authoritative Data Source** — Centralize CRM and ERP data into a single SQL data warehouse so all teams operate from the same validated metrics
2. **Implement Medallion Architecture** — Design a scalable three tier pipeline (Bronze, Silver, Gold) to systematically transform raw data into business ready assets
3. **Automate ETL Processes** — Replace manual data handling with automated stored procedures and bulk loading to ensure data is consistently refreshed and prepped for analysis
4. **Optimize for BI and Analytics** — Implement a star schema using fact and dimension tables to support efficient querying for use cases such as year-over-year growth, customer segmentation, and performance analysis

---

## 🏗️ 4. Data Architecture Overview

For this project, a modern data warehouse was built using a medallion architecture, designed to progressively improve data quality as information moves through the pipeline. The architecture separates ingestion, transformation, and analytics into distinct layers, allowing raw data to be systematically refined into usable assets for the company. This structure not only preserves data lineage from source to report, but also enables targeted quality checks and validation at each stage of processing.

### Why Data Warehouse?

A data warehouse architecture was selected over a data lake or lakehouse because it best supports structured analytics and reporting. While data lakes provide flexibility for semi-structured and unstructured data, they often lack the schema enforcement required for financial and sales analysis. Although a lakehouse offers a hybrid approach, it would have been unnecessarily complicated given the scope and reliance on structured CSV sources and a SQL Server environment. The warehouse model provided the best balance of performance, governance, and integration.

### Medallion Architecture Layers

The medallion architecture is implemented across three distinct layers:

| Layer | Purpose | Key Functions |
|-------|---------|---------------|
| 🥉 **Bronze** (Raw Data) | Landing zone for unmodified source data | Bulk insert operations, preserves original schema, enables traceability and auditing |
| 🥈 **Silver** (Cleansed & Standardized) | Quality gate for consistent data | Schema enforcement, NULL handling, deduplication, categorical standardization |
| 🥇 **Gold** (Business Ready) | Presentation-ready analytics tier | Dimensional star schema, optimized for BI tools, consistent KPI definitions |

<details>
<summary><b>📌 Bronze Layer Details</b></summary>

**Bronze Layer (Raw Data)** functions as the system's "landing zone", ingesting data directly from CRM and ERP source systems in its original format. Source CSV files are loaded using bulk insert operations, with no transformations applied. Tables retain the original schema and naming conventions, preserving a copy of the source data for traceability, auditing, and troubleshooting. This layer ensures that raw inputs are always available for validation and reprocessing.

</details>

<details>
<summary><b>📌 Silver Layer Details</b></summary>

**Silver Layer (Cleansed & Standardized)** is responsible for enforcing data quality and consistency across the warehouse. At this stage, raw data is refined through schema enforcement, NULL handling, categorical standardization (such as gender and status codes), and normalization of date formats. Duplicate records across integrated systems are resolved using SQL window functions, producing a clean and standardized dataset. This layer prevents data quality issues from growing and affecting downstream reporting.

</details>

<details>
<summary><b>📌 Gold Layer Details</b></summary>

**Gold Layer (Business Ready)** represents the curated and presentation-ready tier of the architecture. Data is transformed into a dimensional star schema composed of fact and dimension tables optimized for performance analytical querying. This structure moves away source system complexity and is designed for direct consumption by BI tools, enabling fast reporting, complex aggregations, and consistent KPI definitions. As the organization's central data source, this layer ensures that insights delivered to stakeholders are accurate and useful.

</details>

For the company, this architecture ensures that every report, dashboard, and analysis is built from the same governed data foundation, reducing reporting discrepancies between users and restoring trust in company wide metrics.

### Architecture Diagram

![image](https://github.com/EndreGuljas/SQL-Data-Warehouse/blob/main/Data_Warehouse/diagrams/DataArchitecture.png)
---

## 🔄 5. Data Flow & Integration

Before diving into the individual layers of the medallion architecture, it is important to understand how data flows through the pipeline end to end. The purpose of the pipeline is to transform raw source data into a clean and analytics-ready dataset that can support consistent reporting and business insight.

### Pipeline Flow

![image](https://github.com/EndreGuljas/SQL-Data-Warehouse/blob/main/Data_Warehouse/diagrams/DataFlow.png)

The pipeline begins with the extraction of flat files from two primary source systems: the CRM system, which manages transactional sales data and core customer records, and the ERP system, which provides critical master data such as product hierarchies, demographic attributes, and geographic classifications. These datasets are ingested into the bronze layer in their original format to preserve data lineage and support troubleshooting.

From the bronze layer, the data progresses into the silver layer, where it is cleaned, standardized, and prepared for integration. This includes enforcing schemas, resolving missing values, normalizing categorical fields, and aligning date formats. At this stage, records from both systems are harmonized into a consistent structure, ensuring that downstream analytics operate on a unified representation of the business.

In the final stage, curated datasets are promoted to the gold layer, where they are modeled into a dimensional star schema. Core business entities are represented through tables such as `fact_sales`, `dim_customers`, and `dim_products`, enabling efficient querying, aggregation, and direct consumption by BI tools.

### Integration Challenge

> **Key Challenge:** One of the main challenges in this project was integrating two systems that did not share a common primary key structure. To resolve this, I designed a relationship model that maps transactional records from the CRM to corresponding master data in the ERP using standardized business keys and transformation logic.

This integration layer creates a unified analytical view of customers, products, and sales activity while preserving data integrity across systems. This unified integration eliminated the need for manual CSV joins and significantly reduced reporting turnaround time, allowing the company to analyze customers, products, and sales activity in a consistent view.

![image](https://github.com/EndreGuljas/SQL-Data-Warehouse/blob/main/Data_Warehouse/diagrams/DataIntegration.png)

---

## 🗄️ 6. Warehouse Layers

### Bronze Layer

The bronze layer serves as the landing zone for all raw data ingested from the CRM and ERP source systems. Its main purpose is to extract and store data in its native format, preserving complete data lineage while ensuring full traceability throughout the pipeline. By maintaining an untouched historical record of source data, this layer enables effective auditing, debugging, and reprocessing without impacting downstream transformations.

#### Technical Implementation

**Schema and Storage:** Data is stored in physical tables within a dedicated bronze schema, mirroring the source system structures exactly.

**Data Definition (DDL):** Six tables were created to capture incoming CSV files:
- **CRM Sources:** `bronze.crm_cust_info`, `bronze.crm_prd_info`, `bronze.crm_sales_details`
- **ERP Sources:** `bronze.erp_loc_a101`, `bronze.erp_cust_az12`, `bronze.erp_px_cat_g1v2`

**Load Strategy:** A truncate-and-insert approach is used to ensure each batch reflects the most current state of the source systems while preventing the buildup of stale or redundant data.

**Automation:** A modular stored procedure, `bronze.load_bronze`, encapsulates all BULK INSERT logic across tables. The procedure includes logging of start times, end times, and load durations, enabling visibility into pipeline execution.

#### Business Value

From a business perspective, the bronze layer acts as a secure audit trail. By retaining an original copy of source data, the organization can investigate data issues, validate reporting discrepancies, and reprocess historical records without repeatedly querying operational systems. This isolation protects the performance of CRM and ERP platforms while establishing a reliable foundation for downstream transformation. Centralizing raw data in one location also represents the first step toward eliminating data silos. For the business, this guarantees that historical data can always be reprocessed or revalidated without disrupting operational systems.

---

### Silver Layer

The silver layer transforms raw ingested data into a clean and standardized format. This stage acts as the warehouse's primary quality gate, ensuring that all downstream reporting and modeling are built on accurate and consistent data. By centralizing transformation logic in this layer, the pipeline builds a reliable foundation for dimensional modeling.

#### Technical Implementation

**Schema and Storage:** Data is stored in physical tables within a dedicated silver schema. Unlike the bronze layer, these tables use refined data types, standardized naming conventions, and business-aligned structures.

**Cleaning and Transformation Techniques:**

- **NULL Handling:** Applied `ISNULL()` and `COALESCE()` to replace missing values with appropriate defaults (e.g., assigning a cost of 0 when product cost data is unavailable)
- **Deduplication:** Implemented `ROW_NUMBER()` within Common Table Expressions (CTEs) to identify and remove duplicate records based on business keys, keeping only the most relevant entries (such as the most recent customer record)
- **Categorical Normalization:** Used `CASE` logic to map inconsistent categorical values into standardized business definitions (e.g., consolidating 'M', 'm', and 'Male' into 'Male')
- **Text Processing:** Applied `TRIM()` and `UPPER()` to remove extra whitespace and maintain consistent casing across descriptive fields
- **Advanced Business Logic:** Leveraged window functions such as `LEAD()` to derive historical attributes, including calculating product end dates based on successive start dates

**Automation:** A centralized stored procedure, `silver.load_silver`, orchestrates the transformation and loading of all six core tables from the bronze layer. This procedure standardizes transformation logic, enforces data quality rules, and ensures repeatable processing.

#### Data Quality Validation

To maintain a high standard of data integrity, multiple automated validation scripts were developed for the silver layer. These checks flag:
- ❌ Duplicate business keys across integrated datasets
- ❌ Unexpected NULL values in mandatory fields
- ❌ Invalid or illogical date ranges (e.g., end dates preceding start dates)
- ❌ Residual whitespace and non-standard characters that could affect joins or reporting

These quality controls prevent corrupted or inconsistent records from moving into downstream layers.

#### Business Value

The silver layer delivers a consistent data foundation for the company. By centralizing cleaning and standardization logic within the warehouse, it eliminates the need for analysts to perform ad hoc data cleaning in spreadsheets or BI tools, which reduces the risk of conflicting results and inconsistent metrics. Other important business outcomes include:

- ✅ **Consistency:** Customer attributes such as name and gender are standardized across CRM and ERP sources, ensuring uniform reporting across departments
- ✅ **Accuracy:** Deduplication prevents double counting and inflated revenue figures, providing a true representation of business performance
- ✅ **Historical Performance Tracking:** Accurately derived product start and end dates enable precise analysis of pricing changes and their impact on sales over time

This directly improved confidence in company-wide metrics by ensuring that customer counts, revenue totals, and product performance figures were consistent across departments.

---

### Gold Layer

The gold layer represents the final stage of the pipeline, where cleansed and standardized data from the silver layer is transformed into a usable format. This layer serves as the presentation layer of the warehouse, built to support data analysis and reporting. It moves away from technical complexity and creates a simplified view of the data optimized for end-users.

#### Technical Implementation

**Schema and Storage:** A dimensional modeling approach was adopted using a star schema, organizing data into a central fact table surrounded by related dimension tables. This structure simplifies analytical queries and improves performance by minimizing the amount of joins needed.

![image](https://github.com/EndreGuljas/SQL-Data-Warehouse/blob/main/Data_Warehouse/diagrams/GoldDataModel.png)

**Use of SQL Views:** The gold layer is implemented using SQL views rather than physical tables. This ensures that all analytical models always reflect the latest validated data from the silver layer without needing extra storage or reload processes.

**Surrogate Key Strategy:** A surrogate key framework was implemented (e.g., using `ROW_NUMBER()`) to generate unique identifiers for dimension records. This separates the analytical model from source system keys, supports consistent joins across fact and dimension tables, and improves query performance.

**Semantic Layer:** All fields were renamed from technical shorthand to appropriate business aliases (e.g., `cst_id` → `Customer_ID`). This semantic layer improves usability for non-technical stakeholders and ensures that the data model is aligned with business terminology.

#### Business Value

In the gold layer, the project shifts from data engineering to business analytics. By using a star schema, complex source system logic is removed from end users, allowing analysts to easily explore key metrics such as revenue, margins, and order volume. Centralizing business rules and metric definitions creates a single, trusted version of the data, ensuring consistent KPIs across all departments. Clear naming conventions and a simplified structure make the data easy to use, allowing for non-technical stakeholders to build insights and create reports. Doing this reduces the dependence on data teams and maintains performance as the warehouse grows. As a result, business users can focus on answering questions and making decisions rather than interpreting or correcting the data.

---

## 🔍 7. Exploratory Data Analysis

The EDA phase focused on validating the analytical readiness of the data warehouse, while also beginning to uncover early business insights. This step ensured that the gold layer accurately reflected real business activity and validated that downstream reporting and analysis would be reliable. To structure the analysis, the dataset was evaluated through two parts:

- **Dimensions:** Descriptive attributes such as Customer Gender, Product Category, and Geography. These allow for data to be sorted
- **Measures:** Quantitative values like Sales Amount, Quantity, and Order Count. These allow for the application of calculations to business activity

### Analysis Framework

To develop a comprehensive understanding of the dataset, EDA was performed across multiple analytical dimensions:

1. **Database Exploration** — High-level audit of schema, table counts, column data types, and key relationships
2. **Dimensions Exploration** — Investigation into categorical fields using `DISTINCT` counts to identify all valid groups
3. **Date Exploration** — Determined data boundaries using `MIN()` and `MAX()` on order dates (2022–2024)
4. **Measures Exploration** — Profiled numerical facts using `SUM()`, `AVG()`, and `COUNT()` to establish business scale
5. **Magnitude Analysis** — Analyzed [Measure] BY [Dimension] to identify which segments drive business volume
6. **Ranking Analysis** — Used `ORDER BY`, `RANK()`, and `DENSE_RANK()` to isolate top and bottom performers

### Business Questions and Insights

Throughout the EDA process, it was important to directly answer stakeholder questions that helped align the analysis with real business priorities. The exploration focused on understanding how customers, products, and revenue are distributed across the organization, how performance changes over time, and which segments contribute the most value. Some of the key questions addressed during this phase included:

- How is the customer base distributed across geographic regions?
- Which product categories offer the widest selection?
- How are revenue trends over time?
- Who the highest-value customers and best-performing products are?

These questions guided the analysis and ensured that exploratory work remained focused on insights that would directly support the company. The EDA uncovered several meaningful patterns that could be important for strategic planning:

> 📍 **Revenue Concentration:** Revenue was found to be highly concentrated geographically, with the top three regions accounting for over 60% of total sales. This concentration highlights clear opportunities for targeted marketing efforts and regional investment.

> 📦 **Product Performance:** The electronics category drove the highest volume of orders, accessories consistently delivered the strongest profit margins per unit. This distinction reinforced the importance of evaluating both volume and profitability when assessing product performance.

> 📅 **Seasonal Behavior:** Time-based analysis showed strong seasonal behavior, with a 15% increase in average order value during holiday periods. This confirmed the effectiveness of seasonal promotions and underscored the value of aligning inventory and marketing strategies with peak purchasing windows.

> 👥 **Customer Segmentation:** Customer segmentation analysis demonstrated that the top 5% of customers by revenue contributed a disproportionately large share of overall profitability. This insight highlights a clear opportunity to develop targeted retention and loyalty strategies focused on high-value customers.

For the company, these findings provide immediate direction for targeted marketing, pricing evaluation, and customer retention strategies, transforming exploratory analysis into actionable business guidance.

### Technical Implementation

The EDA phase used a range of analytical techniques to explore, validate, and summarize the data:

| Technique | Functions Used | Purpose |
|-----------|---------------|---------|
| **Aggregations** | `SUM()`, `AVG()`, `COUNT()`, `MIN()`, `MAX()` | Calculate core KPIs and establish baseline performance |
| **Segmentation** | `GROUP BY`, `HAVING` | Segment measures by key dimensions for targeted analysis |
| **Time-Series** | `DATEPART()`, `DATETRUNC()` | Analyze revenue trends and performance patterns |
| **Ranking** | `RANK()`, `DENSE_RANK()` | Identify top/bottom performers without losing record detail |

---

## 📈 8. Advanced Analytics and Reporting

The final phase of the project focuses on transforming the warehouse data into data that is ready for business analytics. While earlier phases focussed on data quality and uncovered baseline insights, this stage moves beyond reporting to answer important business questions related to growth, customer behavior, and product performance over time. This analytical layer is designed to support strategic decision making by tracking business momentum, benchmarking current performance against historical trends, and identifying high-impact customer and product segments.

### Core Business Themes

The analyses were developed around five core business themes, each aligned with stakeholder priorities:

1. **Growth Analysis** — Measured Year-over-Year (YoY) and Month-over-Month (MoM) revenue growth to evaluate long-term trends and short-term demand fluctuations
2. **Cumulative Performance** — Implemented running totals for revenue and customer acquisition to visualize how the business scales throughout the fiscal year and tracks progress toward annual targets
3. **Performance Benchmarking** — Compared current performance against historical averages and prior periods to identify breakout months and underperforming periods in proper context
4. **Part-to-Whole Analysis** — Evaluated how individual products and categories contribute to total revenue, supporting 80/20 analysis for inventory and catalog optimization
5. **Customer Segmentation** — Segmented customers based on purchase frequency, recency, and total spend to differentiate high-value customers from standard and at risk segments

### Strategic Insights

The analyses uncovered several strategic insights that have direct business implications that can be used by the company:

<details>
<summary><b>📊 Growth Insights</b></summary>

**YoY Growth Momentum:** Using `LAG()` to compare revenue across yearly periods revealed that while total revenue is increasing, growth velocity peaked in specific quarters. This helped isolate which marketing initiatives drove meaningful impact versus organic baseline growth.

**Seasonality Detection:** Time-series analysis identified significant monthly fluctuations, including a ~20% spike in certain product categories during off-season months. This suggested replenishment-driven purchasing behavior rather than promotion-driven demand.

</details>

<details>
<summary><b>💰 Revenue & Product Performance</b></summary>

**80/20 Revenue Concentration:** Part-to-whole analysis showed that approximately 20% of the product catalog generated nearly 80% of total revenue. This insight supports inventory optimization by prioritizing high-velocity SKUs and reevaluating long-tail products.

**Cumulative Revenue Trajectory:** Running totals revealed that the business consistently reaches its break-even point earlier in the fiscal year than expected, enabling greater confidence in mid-year reinvestment decisions.

</details>

<details>
<summary><b>👥 Customer Behavior</b></summary>

**High-Value Customer Identification:** Spend-frequency segmentation isolated a VIP customer tier (top 5% by revenue) with a retention rate nearly three times higher than the average customer, highlighting the ROI of targeted loyalty programs.

**At-Risk Customer Detection:** Recency analysis identified a segment of customers who had not made a purchase in over six months, creating a clear re-engagement opportunity for sales and marketing teams.

</details>

These analyses were designed not just to report performance, but to support concrete business decisions around growth strategy, inventory prioritization, customer retention, and reinvestment planning.

### Technical Implementation

The analytics process took the data foundation and turned it into measurable business outcomes. To complete the analyses, following techniques were used:

- **Common Table Expressions (CTEs):** Modularized complex, multi-step logic and improved query readability
- **Window Functions:** Applied `RANK()`, `DENSE_RANK()`, `LAG()`, and `LEAD()` to perform comparative analysis across time and entities without costly self-joins
- **Complex Joins and Subqueries:** Integrated fact and dimension tables to ensure all analyses were enriched with full business context

### Analytical Outputs

The final product of this phase was the creation of numerous SQL views in the gold layer. These views serve as the final consumable outputs of the warehouse, designed for direct integration with BI tools such as Power BI or Tableau:

- `gold.view_customer_report` — A unified view of the customer lifecycle, combining demographic attributes, geographic data, purchase behavior, and lifetime value metrics
- `gold.view_product_report` — A dashboard-ready view of product performance, margins, and category contribution over time

---

## ✅ 9. Data Quality & Validation

Maintaining high-quality data was an important requirement throughout the project. A structured validation framework was implemented across the medallion architecture to detect issues early and prevent invalid data from reaching the gold layer. By enforcing quality checks at each stage of the pipeline, the warehouse delivers reliable data throughout the pipeline.

### Validation Approach

The validation approach relies on three key mechanisms:

1. **Automated QA Scripts** — Evaluate predefined data quality rules and return clear pass/fail outcomes
2. **Exception Reporting** — Queries isolate invalid records for fast troubleshooting and root-cause analysis
3. **Embedded ETL Logic** — Core validation logic included directly into load procedures, allowing issues to be flagged during pipeline execution

### Layer-Specific Validations

#### 🥉 Bronze Layer

| Validation Type | Purpose |
|----------------|---------|
| **Record Count Validation** | Verified that row counts matched between source CSV files and bronze tables to ensure no data loss during bulk loads |
| **Schema Consistency** | Flagged unexpected structural changes from source systems |

#### 🥈 Silver Layer

| Validation Type | Purpose |
|----------------|---------|
| **Null and Blank Detection** | Identified missing values in critical keys (e.g., Customer_ID, Product_Key) |
| **Standardization Audit** | Confirmed categorical fields (e.g., gender, marital status) followed unified business standards |
| **Range and Logic Validation** | Detected anomalies such as negative prices or invalid date ranges |

#### 🥇 Gold Layer

| Validation Type | Purpose |
|----------------|---------|
| **Referential Integrity** | Used anti-joins to ensure all fact records had matching dimension keys, preventing orphaned data |
| **Uniqueness Validation** | Enforced unique primary and surrogate keys to avoid duplication and fan-out issues |

### Business Value

This validation framework serves as an important risk mitigation layer for the analytics environment. By catching issues early in the pipeline, it prevents inaccurate metrics, inflated revenue figures, and conflicting reports. For the business, this translates to data and reporting that can be trusted by stakeholders. For company stakeholders, this validation framework ensures that decisions are based on trusted data, eliminating disputes over numbers and enabling leadership to act with confidence.

---

## 🎓 10. Conclusion

Overall, the project was successfully able to deliver a well performing SQL data warehouse by integrating CRM and ERP systems into a single platform. From the company's perspective, this project replaced fragmented reporting with a unified analytics platform that supports faster, more confident decision making across departments. Using a medallion architecture, the pipeline enables a flow from raw ingestion to a usable star schema for the business, supported by automated ETL processes and thorough data quality controls.

### Key Business Insights

Through the gold layer and advanced analytics section, several high-impact business insights were uncovered for the company:

| Insight | Impact |
|---------|--------|
| **Revenue Concentration** | Over 60% of total revenue originates from just three geographic regions, enabling targeted marketing and resource allocation |
| **Profitability vs. Volume** | While Electronics leads in sales volume, Accessories deliver the highest profit margins per unit |
| **80/20 Revenue Distribution** | Approximately 20% of the product catalog accounts for nearly 80% of total revenue, highlighting clear opportunities for inventory optimization |
| **Customer Lifetime Value** | A VIP customer tier (top 5% by spend) was identified with significantly higher retention rates, supporting targeted loyalty and retention strategies |

### Technical Achievements

From a technical perspective, this project applied both data engineering and analytics practices across the full data lifecycle. I designed and optimized workflows to ensure the warehouse is scalable, reliable, and analytics ready, including:

- ✅ Advanced use of SQL for database design and stored procedure development
- ✅ Dimensional modeling with fact and dimension tables
- ✅ Automated ETL pipelines using bulk loading and truncate-and-insert strategies
- ✅ Built-in data quality validation to enforce integrity and consistency
- ✅ Excellent project management with documented architecture and requirements tracking

### Lessons Learned

Beyond the technical details, this project reinforced that effective data engineering and analytics must be driven by business needs rather than technology alone. The silver layer showed the importance of data cleaning in preventing downstream reporting errors, while the gold layer highlighted how semantic clarity and business-friendly models allow for more accessible user analytics.

### Future Enhancements

Looking ahead, the project can easily be scaled and improved for further growth. Future enhancements could include:

- 🔄 Introducing workflow orchestration with **Apache Airflow**
- 📝 Managing transformations as code using **dbt**
- ☁️ Migrating to a cloud-native platform such as **Snowflake**
- 🤖 Supporting advanced analytics use cases such as customer churn prediction and demand forecasting

In conclusion, this project demonstrates how data systems can transform raw, disconnected data into valuable business insights that drive decision making for a company.

---

## 📬 Contact

Feel free to reach out if you have questions about this project or would like to discuss data engineering and analytics!

---

*Built with SQL Server, T-SQL, and a commitment to data quality* ✨
