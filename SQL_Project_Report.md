# 🧾 SQL Project Report – Retail Certification Sales

This report contains SQL queries executed on the `RetailSalesDB.sqlite` database, designed around real-world certifications and personal learning paths.

---

## 📊 Query 1: Most Popular Certifications by Quantity Sold

```sql
SELECT 
    p.name AS certification,
    SUM(oi.quantity) AS total_enrolled
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.name
ORDER BY total_enrolled DESC;
```

**Purpose:** This query returns the most popular certifications based on the number of enrollments (purchases). Useful for identifying best-sellers.

---

## 📈 Query 2: Monthly Revenue Trend

```sql
SELECT 
    strftime('%Y-%m', payment_date) AS month,
    ROUND(SUM(amount), 2) AS total_revenue
FROM payments
GROUP BY month
ORDER BY month;
```

**Purpose:** Aggregates total revenue by month using payment dates. A staple in business reporting.

---

## 🏅 Query 3: Tagging VIP Students Based on Total Spend

```sql
SELECT 
    c.name,
    ROUND(SUM(p.amount), 2) AS total_spent,
    CASE 
        WHEN SUM(p.amount) > 1000 THEN 'VIP'
        ELSE 'Regular'
    END AS customer_type
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN payments p ON o.order_id = p.order_id
GROUP BY c.name;
```

**Purpose:** Identifies high-spending customers using a CASE statement. Often used in loyalty programs.

---

## 💰 Query 4: Customers Who Spent Above the Average

```sql
SELECT 
    c.name,
    SUM(p.amount) AS total_spent
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN payments p ON o.order_id = p.order_id
GROUP BY c.name
HAVING total_spent > (
    SELECT AVG(amount)
    FROM payments
);
```

**Purpose:** Uses a subquery in the HAVING clause to find customers who spent more than the average order payment.

---

## 🥇 Query 5: Top 3 Certifications by Enrollments Using CTE

```sql
WITH cert_counts AS (
    SELECT 
        p.name,
        SUM(oi.quantity) AS total_enrolled
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY p.name
)
SELECT * 
FROM cert_counts
ORDER BY total_enrolled DESC
LIMIT 3;
```

**Purpose:** Uses a Common Table Expression (CTE) to simplify and modularize the query logic for ranking top certifications.

