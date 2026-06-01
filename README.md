# 🛒 Zepto Inventory & Sales Data Analysis using SQL

## 📌 Project Overview

This project analyzes a Zepto product inventory dataset using SQL. The objective is to perform data exploration, data cleaning, and business analysis to uncover meaningful insights related to pricing, discounts, inventory, and revenue.

## 🛠️ Technologies Used

- PostgreSQL
- SQL
- Data Analysis
- Data Cleaning
- Business Intelligence

---

## 📂 Database Schema

```sql
DROP TABLE IF EXISTS zepto;

CREATE TABLE zepto (
    sku_id SERIAL PRIMARY KEY,
    category VARCHAR(120),
    name VARCHAR(150) NOT NULL,
    mrp NUMERIC(8,2),
    discountPercent NUMERIC(5,2),
    availableQuantity INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms INTEGER,
    outOfStock BOOLEAN,
    quantity INTEGER
);
```

---

## 🔍 Data Exploration

### Total Number of Products

```sql
SELECT COUNT(*) FROM zepto;
```

### View Sample Data

```sql
SELECT * FROM zepto
LIMIT 10;
```

### Check for Null Values

```sql
SELECT *
FROM zepto
WHERE name IS NULL
   OR category IS NULL
   OR mrp IS NULL
   OR discountPercent IS NULL
   OR discountedSellingPrice IS NULL
   OR weightInGms IS NULL
   OR availableQuantity IS NULL
   OR outOfStock IS NULL
   OR quantity IS NULL;
```

### Unique Product Categories

```sql
SELECT DISTINCT category
FROM zepto
ORDER BY category;
```

### Products In Stock vs Out of Stock

```sql
SELECT outOfStock, COUNT(sku_id)
FROM zepto
GROUP BY outOfStock;
```

### Products Appearing Multiple Times

```sql
SELECT name,
       COUNT(sku_id) AS number_of_skus
FROM zepto
GROUP BY name
HAVING COUNT(sku_id) > 1
ORDER BY COUNT(sku_id) DESC;
```

---

## 🧹 Data Cleaning

### Find Products with Invalid Prices

```sql
SELECT *
FROM zepto
WHERE mrp = 0
   OR discountedSellingPrice = 0;
```

### Remove Products with MRP = 0

```sql
DELETE FROM zepto
WHERE mrp = 0;
```

### Convert Prices from Paise to Rupees

```sql
UPDATE zepto
SET mrp = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```

---

## 📊 Business Analysis

### 1️⃣ Top 10 Best-Value Products by Discount Percentage

```sql
SELECT DISTINCT name,
       mrp,
       discountPercent
FROM zepto
ORDER BY discountPercent DESC
LIMIT 10;
```

### 2️⃣ High-MRP Products That Are Out of Stock

```sql
SELECT DISTINCT name,
       mrp
FROM zepto
WHERE outOfStock = TRUE
  AND mrp > 300
ORDER BY mrp DESC;
```

### 3️⃣ Estimated Revenue by Category

```sql
SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS total_revenue
FROM zepto
GROUP BY category
ORDER BY total_revenue DESC;
```

### 4️⃣ Products with MRP Above ₹500 and Discount Below 10%

```sql
SELECT DISTINCT name,
       mrp,
       discountPercent
FROM zepto
WHERE mrp > 500
  AND discountPercent < 10
ORDER BY mrp DESC;
```

### 5️⃣ Top 5 Categories with Highest Average Discounts

```sql
SELECT category,
       ROUND(AVG(discountPercent), 2) AS avg_discount
FROM zepto
GROUP BY category
ORDER BY avg_discount DESC
LIMIT 5;
```

### 6️⃣ Best Value Products by Price per Gram

```sql
SELECT DISTINCT name,
       weightInGms,
       discountedSellingPrice,
       ROUND(discountedSellingPrice / weightInGms, 2) AS price_per_gram
FROM zepto
WHERE weightInGms >= 100
ORDER BY price_per_gram;
```

### 7️⃣ Categorize Products by Weight

```sql
SELECT DISTINCT name,
       weightInGms,
       CASE
           WHEN weightInGms < 1000 THEN 'Low'
           WHEN weightInGms < 5000 THEN 'Medium'
           ELSE 'Bulk'
       END AS weight_category
FROM zepto;
```

### 8️⃣ Total Inventory Weight per Category

```sql
SELECT category,
       SUM(weightInGms * availableQuantity) AS total_weight
FROM zepto
GROUP BY category
ORDER BY total_weight DESC;
```

---

## 📈 Key Insights

- Identified products with the highest discounts.
- Detected premium products currently out of stock.
- Estimated category-wise revenue contribution.
- Found categories offering the highest average discounts.
- Calculated price-per-gram to determine best-value products.
- Segmented products into weight-based categories.
- Measured total inventory weight across categories.

---

## 🎯 SQL Concepts Used

- CREATE TABLE
- DROP TABLE
- SELECT
- WHERE
- GROUP BY
- ORDER BY
- HAVING
- DISTINCT
- Aggregate Functions (SUM, AVG, COUNT)
- CASE Statements
- UPDATE
- DELETE

---

## 👨‍💻 Author

**Om Vanra**

Aspiring Data Analyst skilled in SQL, Python, Power BI, and Data Visualization.

📧 Open to internships and entry-level opportunities in Data Analytics.
