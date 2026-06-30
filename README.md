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

![Architecture Diagram](./docs/architecture_diagram.png)

### Medallion Architecture

