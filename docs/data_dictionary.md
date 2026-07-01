# Data Dictionary - Brazilian E-Commerce Dataset

## Project Overview
- **Dataset:** Brazilian E-Commerce Public Dataset by Olist
- **Source:** Kaggle
- **License:** CC BY-NC-SA 4.0
- **Time Period:** 2016-2018
- **Key Tables:** 9 CSV files

---

## Silver Layer Tables

### silver_olist_customers_dataset

| Column | Type | Description |
|--------|------|-------------|
| customer_id | VARCHAR(100) | Unique customer identifier |
| customer_unique_id | VARCHAR(100) | Customer identifier across sessions |
| customer_zip_code_prefix | VARCHAR(15) | First 5 digits of Brazilian zip code |
| customer_city | VARCHAR(50) | Customer's city |
| customer_state | VARCHAR(10) | Customer's state (2-letter code) |

### silver_olist_orders_dataset

| Column | Type | Description |
|--------|------|-------------|
| order_id | VARCHAR(50) | Unique order identifier |
| customer_id | VARCHAR(100) | Links to customers table |
| order_status | VARCHAR(50) | delivered, shipped, canceled, etc. |
| order_purchase_timestamp | DATETIME | Date/time of purchase |
| order_approved_at | DATETIME | Payment approval time |
| order_delivered_customer_date | DATETIME | Actual delivery date |
| order_estimated_delivery_date | DATETIME | Estimated delivery date |

### silver_olist_products_dataset

| Column | Type | Description |
|--------|------|-------------|
| product_id | VARCHAR(50) | Unique product identifier |
| product_category_name | VARCHAR(50) | Category name in Portuguese |
| product_weight_g | FLOAT | Product weight in grams |
| product_length_cm | FLOAT | Product length in cm |
| product_height_cm | FLOAT | Product height in cm |
| product_width_cm | FLOAT | Product width in cm |

### silver_olist_sellers_dataset

| Column | Type | Description |
|--------|------|-------------|
| seller_id | VARCHAR(50) | Unique seller identifier |
| seller_city | VARCHAR(50) | Seller's city |
| seller_state | VARCHAR(10) | Seller's state (2-letter code) |

---

## Gold Layer (Warehouse) Tables

### Sales.Dim_Customers

| Column | Type | Description |
|--------|------|-------------|
| customer_id | VARCHAR(100) | Primary key |
| customer_unique_id | VARCHAR(100) | Customer identifier across sessions |
| customer_zip_code_prefix | VARCHAR(15) | First 5 digits of Brazilian zip code |
| customer_city | VARCHAR(50) | Customer's city |
| customer_state | VARCHAR(10) | Customer's state |

### Sales.Dim_Products

| Column | Type | Description |
|--------|------|-------------|
| product_id | VARCHAR(50) | Primary key |
| product_category_name | VARCHAR(50) | Category in Portuguese |
| product_category_name_english | VARCHAR(100) | Category in English |
| product_weight_g | FLOAT | Product weight in grams |
| product_length_cm | FLOAT | Product length in cm |
| product_height_cm | FLOAT | Product height in cm |
| product_width_cm | FLOAT | Product width in cm |

### Sales.Fact_Orders

| Column | Type | Description |
|--------|------|-------------|
| order_id | VARCHAR(50) | Primary key |
| customer_id | VARCHAR(100) | Foreign key to Dim_Customers |
| order_status | VARCHAR(50) | Current order status |
| order_purchase_timestamp | DATETIME | Purchase date/time |
| order_approved_at | DATETIME | Approval date/time |
| order_delivered_customer_date | DATETIME | Delivery date/time |

### Sales.Fact_Order_Items

| Column | Type | Description |
|--------|------|-------------|
| order_item_id | INT | Line item number within order |
| order_id | VARCHAR(50) | Foreign key to Fact_Orders |
| product_id | VARCHAR(50) | Foreign key to Dim_Products |
| seller_id | VARCHAR(50) | Foreign key to Dim_Sellers |
| price | FLOAT | Item price (BRL) |
| freight_value | FLOAT | Shipping cost (BRL) |
| total_revenue | FLOAT | Price + Freight (calculated) |

---

## Key Relationships (Star Schema)

- Dim_Customers → Fact_Orders (one-to-many)
- Fact_Orders → Fact_Order_Items (one-to-many)
- Dim_Products → Fact_Order_Items (one-to-many)
- Dim_Sellers → Fact_Order_Items (one-to-many)

---

## Data Quality Notes

- **Geolocation:** Deduplicated from 1M+ rows to ~20,000 unique zip codes
- **Customers:** No duplicates found, no null values
- **Orders:** No duplicates found, minimal nulls in some date fields
- **Products:** Some nulls in category translations (handled with LEFT JOIN)
