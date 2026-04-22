# Customer Behavior — SQL Analysis

SQL-based analysis on the `customer` table in PostgreSQL, covering revenue, product ratings, shipping, subscriptions, and customer segmentation.

---

## Database

**Database:** `customer_behaviour_analysis`  
**Table:** `customer`  
**Tool:** PostgreSQL

---

## Queries Overview

### Q1. Revenue by Gender
```sql
SELECT gender, SUM(purchase_amount) AS revenue
FROM customer
GROUP BY gender;
```

---

### Q2. Discount Users Who Spent Above Average
```sql
SELECT customer_id, purchase_amount 
FROM customer 
WHERE discount_applied = 'Yes' 
  AND purchase_amount >= (SELECT AVG(purchase_amount) FROM customer);
```

---

### Q3. Top 5 Products by Average Review Rating
```sql
SELECT item_purchased, ROUND(AVG(review_rating::numeric), 2) AS "Average Product Rating"
FROM customer
GROUP BY item_purchased
ORDER BY avg(review_rating) DESC
LIMIT 5;
```

---

### Q4. Average Purchase Amount — Standard vs Express Shipping
```sql
SELECT shipping_type, ROUND(AVG(purchase_amount), 2)
FROM customer
WHERE shipping_type IN ('Standard', 'Express')
GROUP BY shipping_type;
```

---

### Q5. Subscriber vs Non-Subscriber Spend
```sql
SELECT subscription_status,
       COUNT(customer_id) AS total_customers,
       ROUND(AVG(purchase_amount), 2) AS avg_spend,
       ROUND(SUM(purchase_amount), 2) AS total_revenue
FROM customer
GROUP BY subscription_status
ORDER BY total_revenue, avg_spend DESC;
```

---

### Q6. Top 5 Products by Discount Rate
```sql
SELECT item_purchased,
       ROUND(100 * SUM(CASE WHEN discount_applied = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) AS discount_rate
FROM customer
GROUP BY item_purchased
ORDER BY discount_rate DESC
LIMIT 5;
```

---

### Q7. Customer Segmentation — New, Returning, Loyal
Segments based on `previous_purchases`:
- **New** → 1 purchase
- **Returning** → 2–10 purchases
- **Loyal** → 11+ purchases

```sql
WITH customer_type AS (
    SELECT customer_id, previous_purchases,
        CASE 
            WHEN previous_purchases = 1 THEN 'New'
            WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
            ELSE 'Loyal'
        END AS customer_segment
    FROM customer
)
SELECT customer_segment, COUNT(*) AS "Number of Customers"
FROM customer_type
GROUP BY customer_segment;
```

---

### Q8. Top 3 Products per Category
Uses `ROW_NUMBER()` window function partitioned by category:
```sql
WITH item_counts AS (
    SELECT category, item_purchased,
           COUNT(customer_id) AS total_orders,
           ROW_NUMBER() OVER (PARTITION BY category ORDER BY COUNT(customer_id) DESC) AS item_rank
    FROM customer
    GROUP BY category, item_purchased
)
SELECT item_rank, category, item_purchased, total_orders
FROM item_counts
WHERE item_rank <= 3;
```

---

### Q9. Repeat Buyers & Subscription Likelihood
Filters customers with more than 5 previous purchases:
```sql
SELECT subscription_status, COUNT(customer_id) AS repeat_buyers
FROM customer
WHERE previous_purchases > 5
GROUP BY subscription_status;
```

---

### Q10. Revenue by Age Group
```sql
SELECT age_group, SUM(purchase_amount) AS total_revenue
FROM customer
GROUP BY age_group
ORDER BY total_revenue DESC;
```

---

## Key SQL Concepts Used

| Concept | Used In |
|---|---|
| Aggregate functions (`SUM`, `AVG`, `COUNT`) | Q1, Q2, Q4, Q5, Q9, Q10 |
| Subquery | Q2 |
| `CASE WHEN` | Q6, Q7 |
| CTE (`WITH`) | Q7, Q8 |
| Window function (`ROW_NUMBER OVER PARTITION BY`) | Q8 |
| `ROUND`, type casting (`::numeric`) | Q3, Q4, Q5, Q6 |
