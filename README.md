# End-to-End Data Engineering Project: Sales Analytics Pipeline

## 📌 Project Overview
This project demonstrates a full-lifecycle data engineering pipeline built on **Azure Databricks**. It utilizes the **Medallion Architecture** (Bronze, Silver, Gold) to transform raw sales data into a consumption-ready Star Schema for Power BI reporting.

The goal was to build a resilient pipeline that handles schema evolution, ensures data quality, and implements historical tracking (SCD Type 2) for product dimensions.

## 🏗️ Architecture & Tech Stack
**Cloud Platform:** Azure Databricks
**Processing Engine:** Apache Spark (PySpark)
**Storage Format:** Delta Lake
**Orchestration:** Databricks Workflows (Notebook chaining)
**Visualization:** Microsoft Power BI

### Medallion Architecture Layers:
1.  **Bronze Layer (Raw):** Ingests raw data using **Databricks Autoloader** to handle schema drift and quarantine bad data (`_rescued_data`).
2.  **Silver Layer (Cleansed):**
    * Data cleaning and deduplication.
    * Data quality enforcement (e.g., Validating email formats).
    * Separated into modular notebooks per entity (`Silver_Customers`, `Silver_Products`, etc.).
3.  **Gold Layer (Curated):**
    * Implements **Star Schema** modeling.
    * **SCD Type 2** logic applied to `dim_products` to track historical price changes.
    * Generation of Surrogate Keys (`_sk`) for joining Fact and Dimension tables.

## 🗂️ Data Model (Star Schema)
The final Gold layer is modeled for optimal reporting performance:

* **Fact Table:** `fact_orders` (Contains transactions, `total_amount`, `quantity`).
* **Dimensions:**
    * `dim_customers`: Enriched customer data with location info.
    * `dim_products`: Tracks brand, category, and historical pricing (SCD Type 2).
    * `dim_regions`: Geographic zoning data.

## 📊 Power BI Dashboard
A comprehensive dashboard connected to the Gold Layer to track KPIs.

![Dashboard Screenshot](power_bi/dashboard_screenshot.png)

**Key Visuals:**
1.  **Monthly Sales Trend (Line Chart):** Tracks revenue growth over time to identify seasonality.
2.  **Sales by Category (Bar Chart):** Analyzes top-performing product lines (e.g., Electronics vs. Furniture).
3.  **Revenue by Top 5 States:** Geospatial analysis to identify key markets.
4.  **Quantity Distribution by Brand:** Analysis of customer buying patterns per brand.

## 📂 Repository Structure
```text
/src
├── parameters.python           # Global configuration and widget setup
├── Bronze_Layer.python         # Raw data ingestion logic
├── Silver_Customers.python     # Customer data cleaning & validation
├── Silver_Products.python      # Product transformations
├── Silver_Orders.python        # Order processing
├── Silver_Regions.python       # Region standardization
├── Gold_Customers.python       # Dimension loading
├── Gold Products.python        # SCD Type 2 implementation
└── Gold Orders.python          # Fact table aggregations & joins

/power_bi
└── Sales_Dashboard.pbix        # Power BI project file
