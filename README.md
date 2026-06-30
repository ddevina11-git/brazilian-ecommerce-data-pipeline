# Brazilian E-Commerce Data Pipeline (Olist Dataset)

## Project Overview

This project is an end-to-end data engineering pipeline built on **Microsoft Fabric** using the **medallion architecture** (Bronze → Silver → Gold). We analyzed the Brazilian E-Commerce Public Dataset by Olist, containing 100,000+ orders from 2016-2018.

**Key Analyses:**
- Top 10 products by revenue
- Top 10 sellers by revenue
- Geographic revenue distribution (states/cities)
- Customer segmentation (High, Medium, Low value)

## Business Problem

Olist wants to understand their business performance to make data-driven decisions. Specifically, they need to identify:
1. Which products generate the highest revenue?
2. Which sellers contribute the most revenue?
3. Which geographic areas generate the highest revenue?
4. How can customers be segmented based on spending?

## Technology Stack

| Tool | Purpose |
|------|---------|
| Microsoft Fabric | End-to-end analytics platform |
| Bronze Lakehouse | Raw data storage (immutable) |
| Silver Lakehouse | Cleaned and transformed data |
| Gold Warehouse | Structured SQL tables for analysis |
| PySpark | ETL transformations |
| Power BI | Dashboards and reporting |
| Kaggle | Data source (Olist dataset) |

## Architecture Diagram

<img width="1024" height="1536" alt="Final Project Architecture Diagram" src="https://github.com/user-attachments/assets/79c2b604-f25d-4bae-ac17-92437b277f32" />

### Medallion Architecture

Bronze Layer (Raw Data) --> Silver Layer (Cleaned, Validated, Deduplicated) --> Gold Layer (Business Aggregates, Star Schema) --> Power BI Dashboard

## ETL Process

### 1. Extract (Bronze)
- Downloaded 9 CSV files from Kaggle
- Uploaded raw files to Bronze Lakehouse (immutable)

### 2. Transform (Silver)
- Loaded data from Bronze Lakehouse tables
- Cleaned data: handled nulls, removed duplicates
- Deduplicated geolocation data (1M+ → ~20,000 unique entries)
- Validated data quality (completeness, consistency)
- Saved cleaned data to Silver Lakehouse

### 3. Load (Gold)
- Created star schema in Gold Warehouse
- Built dimension and fact tables
- Created views for analytics
- Optimized for Power BI reporting

## Database Schema (ERD)

<img width="1297" height="1018" alt="Final Project ERD Diagram" src="https://github.com/user-attachments/assets/d1d12893-a398-445f-8d40-ca44b07588ff" />

**Key Tables:**

| Table | Description |
|-------|-------------|
| Dim_Customers | Customer dimension |
| Dim_Products | Product dimension |
| Dim_Sellers | Seller dimension |
| Dim_Geolocation | Location dimension |
| Fact_Orders | Order fact table |
| Fact_Order_Items | Order line items |

## Key Findings

### Top 5 Product Categories by Revenue

| Category | Revenue Share |
|----------|---------------|
| health_beauty | 23.33% |
| watches_gifts | ~20% |
| bed_bath_table | ~19% |
| sports_leisure | ~18% |
| computers_accessories | 17.06% |

### Geographic Revenue Concentration

- **São Paulo (SP):** 51.22% of total revenue
- **Top 3 states (SP, RJ, MG):** 85% of market share

### Customer Segmentation

| Segment | Revenue Contribution |
|---------|---------------------|
| High Value | 80th percentile+ |
| Medium Value | 20th-80th percentile |
| Low Value | Below 20th percentile |

## Business Recommendations

1. **Regional Micro-Fulfillment Hubs:** Deploy priority hub in São Paulo to slash logistics costs and delivery times
2. **Tiered Marketing:** 75/25 split – retain high-value customers, test new markets
3. **Shipping Incentives:** Dynamic free-shipping thresholds to increase basket size
4. **Catalog Rationalization:** Clean low-performing categories to reduce overhead

## How to Run This Project

### Prerequisites
- Microsoft Fabric workspace access
- Kaggle account (for data download)

### Setup

1. Download Olist dataset from Kaggle
2. Upload CSV files to Bronze Lakehouse
3. Run PySpark notebooks for Silver transformation
4. Create Gold Warehouse tables using SQL
5. Connect Power BI to Gold Warehouse

### Sample SQL Query

```sql
-- Top 10 products by revenue
SELECT 
    p.product_category_name,
    SUM(oi.price) AS total_revenue
FROM Sales.Fact_Order_Items oi
JOIN Sales.Dim_Products p ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY total_revenue DESC
LIMIT 10;
```

## Challenges & Solutions

| Challenge |	Solution |
|-----------|----------|
| Choosing the right platform |	Selected Microsoft Fabric for unified Lakehouse + Warehouse + Power BI |
|Silver layer logic error |	Fixed to read from Bronze tables (not raw CSV) |
|Data cleanup location confusion |	All cleaning moved to Silver layer (medallion best practice) |
|Geolocation duplicates |	Grouped by zip code prefix, averaged lat/long (1M+ → ~20,000 rows) |

## Limitations

- Data covers 2016-2018 only (not real-time)
- Limited Fabric architecture knowledge (bootcamp students)
- Geolocation precision reduced (averaged coordinates)

## Next Steps

- Build interactive Power BI dashboard with maps and filters
- Automate pipeline with scheduled notebook execution
- Add time-series analysis (monthly trends)
- Implement RFM (Recency, Frequency, Monetary) analysis
- Add partitioning to large fact tables

## Acknowledgments
- Data source: Olist Brazilian E-Commerce Public Dataset (Kaggle)
- Completed as part of Generation Singapore Junior Data Engineering programme (Microsoft & Temasek Polytechnic)
