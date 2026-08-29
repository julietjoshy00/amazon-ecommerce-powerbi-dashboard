# 🛒 Amazon E-Commerce Performance & Revenue Dashboard

An interactive, multi-page Power BI dashboard designed to analyze e-commerce product performance, pricing sensitivity, customer rating dynamics, and the impact of promotional badges (e.g., *Best Seller*). 

Built using a cleaned dataset derived from 42,000+ raw Amazon product listings, this project showcases end-to-end business intelligence workflow—from raw data transformation in Excel/Power Query to advanced DAX modeling, bookmarks, drill-downs, and drill-through architecture.

---

## 📌 Executive Summary

* **Total Revenue Analyzed:** ~$14.76M
* **Average Product Price:** $157.10
* **Data Volume:** Reduced raw dataset noise from 42,000+ rows down to **8,920 high-integrity analytical records** via aggressive deduplication and data cleaning.
* **Key Finding:** Products priced in the lower-to-mid price bins ($10–$30) drive the highest overall unit volume and concentration of Best Seller badges, whereas premium ticket items ($50–$100+) generate peak revenue density.

---

## 🛠️ Technical Stack & Tools

* **Data Cleaning & Transformation:** Microsoft Excel, Power Query
* **Data Modeling & Visualization:** Power BI Desktop
* **Calculations & Analytics:** DAX (Data Analysis Expressions)
* **Design & UX:** Selection Panes, Bookmarks, Page Navigation Buttons, Drill-down, and Drill-through Context Passing

---

## ⚙️ Data Preprocessing & Pipeline Workflow

### 1. Data Cleaning (Excel & Power Query)
* **Deduplication & Schema Optimization:** Filtered duplicate listings using unique product URLs. Dropped irrelevant or low-quality columns (including `Image URL`, `Availability`, and `Sustainability`—which contained over 80% missing entries), streamlining the schema from **17 columns down to 10**.
* **String Standardization & Cleansing:** Cleaned customer rating values (e.g., converting `"4.6 out of 5"` to a clean numerical `4.6`) and removed string noise artifacts (`"more"`, `"was"`, `"this"`) from monthly sales volume fields.
* **Data Type Conversions & Error Handling:** Stripped non-numeric symbols, converted price columns into currency formats, replaced text non-values (like `"No Discount"`) with proper `null` representations, and cast string parsing errors to `0`.
* **Feature Engineering:** Implemented Power Query Conditional Columns to create boolean metrics (`is_bestseller` = `True`/`False`), standardizing promo labels like `"No Badge"` to `False`.

### 2. DAX Measures & Modeling
Explicit DAX measures were engineered for key KPI indicators to maintain model flexibility and precision:

```dax
// Total Revenue Calculation
Total Revenue = SUM('Amazon_Data'[estimated_revenue])

// Average Product Price
Avg Price = AVERAGE('Amazon_Data'[current/discounted_price])

// Bestseller Revenue Contribution Share
Bestseller Rev % = DIVIDE(
    CALCULATE([Total Revenue], 'Amazon_Data'[is_bestseller] = TRUE()), 
    [Total Revenue], 
    0
)
