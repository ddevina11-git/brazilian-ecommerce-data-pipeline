# Challenges & Learnings - Brazilian E-Commerce Project

## Challenge 1: Choosing the Right Platform

**The Problem:** Our team initially didn't know how to approach analyzing 9 CSV files. Should we use local Python, Google Colab, or cloud?

**Our Solution:** After discussing with our lecturer, we chose Microsoft Fabric because it provides Lakehouse, Warehouse, and Power BI in one platform. This eliminated the need to manage separate tools for storage, transformation, and visualization.

**What I Learned:** Unified platforms reduce tool management overhead and make it easier to build end-to-end data pipelines. Understanding the trade-offs between different platforms is an important skill for data engineers.

---

## Challenge 2: Silver Layer Logic Error

**The Problem:** We created tables in Silver Lakehouse by reading directly from CSV files instead of reading from Bronze Lakehouse tables. This broke the medallion architecture principle.

**Our Solution:** Our lecturer pointed out the gap. We reworked our approach:
- Wrong: spark.read.csv("raw_files/orders.csv") → Silver
- Correct: spark.read.table("Bronze.orders") → Clean → Silver.orders

**What I Learned:** Bronze should always hold untouched raw data. Silver should always read from Bronze, not from the original source. This ensures data lineage and allows for recovery if something goes wrong.

---

## Challenge 3: Data Cleanup Location Confusion

**The Problem:** We thought data cleaning should happen in Gold Lakehouse or Gold Warehouse. We created an extra Gold Lakehouse layer unnecessarily.

**Our Solution:** Our lecturer explained the medallion best practices:
- Bronze = Raw (no changes, immutable)
- Silver = Cleaned, validated, deduplicated (ALL cleaning happens here)
- Gold = Business-level aggregates, ready for reporting (no further cleaning)

**What I Learned:** I now understand the medallion architecture layers clearly. Silver is where all cleaning happens. Gold should only contain aggregate data ready for Power BI.

---

## Challenge 4: Geolocation Duplicates

**The Problem:** The geolocation CSV has multiple latitude/longitude entries for the same zip code prefix (sometimes up to 5-10 variations).

**Our Solution:** We used GROUP BY with AVG() to calculate the average latitude and longitude for each unique zip code prefix, reducing 1 million+ rows to ~20,000 unique entries.

**What I Learned:** Real-world data often has duplicates that require aggregation. This is a common challenge in data engineering. Understanding how to handle these cases is a valuable skill.

---

## Challenge 5: Understanding Star Schema

**The Problem:** We weren't sure how to structure the Gold Warehouse for optimal Power BI performance.

**Our Solution:** We learned about star schema design:
- Fact tables at the center (Fact_Orders, Fact_Order_Items)
- Dimension tables surrounding them (Dim_Customers, Dim_Products, Dim_Sellers)

**What I Learned:** Star schema is the industry standard for analytics. Fact tables contain metrics, dimension tables contain descriptive attributes. This design makes Power BI queries fast and intuitive.

---

## Key Takeaways

1. **Always follow medallion architecture:** Bronze → Silver → Gold
2. **Silver is where all cleaning happens:** Don't clean in Gold
3. **Document everything:** Data dictionaries and documentation are essential
4. **Test data quality early:** Check nulls, duplicates, and outliers
5. **Use star schema for analytics:** Fact + Dimension tables
6. **Leverage cloud platforms:** Microsoft Fabric unifies the data pipeline

---
