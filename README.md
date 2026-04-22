# Customer Behavior Analysis

An end-to-end data analysis project on customer shopping behavior covering data cleaning, exploratory analysis, SQL-based querying, and an interactive Power BI dashboard.

---

## Project Structure

```
Customer_Behavior_Analysis/
│
├── Dataset/                  # Raw and cleaned CSV data files
├── EDA_using_Python/         # Jupyter Notebook with EDA and data preprocessing
├── SQL_Queries/              # PostgreSQL queries for business analysis
└── PowerBI_Dashboard/        # Power BI .pbix dashboard file
```

---

## Dataset

**Folder:** [Dataset](Dataset)  
**File:** `customer_shopping_behavior.csv` → cleaned and exported as `customer_data.csv`  
**Database:** PostgreSQL — table `customer` in `customer_behaviour_analysis`

Key columns: `customer_id`, `age`, `age_group`, `gender`, `category`, `item_purchased`, `purchase_amount`, `review_rating`, `subscription_status`, `discount_applied`, `shipping_type`, `frequency_of_purchases`, `purchase_frequency_days`, `previous_purchases`

---

## EDA using Python

**Folder:** [EDA_using_Python](EDA_using_Python)  
**File:** `Customer_behavior_EDA.ipynb`  
**Libraries:** `pandas`, `sqlalchemy`, `psycopg2`

### Steps Performed

**1. Load Data**
```python
df = pd.read_csv('customer_shopping_behavior.csv')
```

**2. Handle Missing Values** — filled `review_rating` with median per category
```python
df['Review Rating'] = df.groupby('Category')['Review Rating'].transform(
    lambda x: x.fillna(x.median())
)
```

**3. Clean Column Names**
```python
df.columns = df.columns.str.lower().str.replace(' ', '_')
df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})
```

**4. Feature Engineering**
```python
# Age Groups
labels = ['Young_Adult', 'Adult', 'Middle_Aged', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)

# Purchase Frequency in days
frequency_mapping = {
    'Weekly': 7, 'Fortnightly': 14, 'Bi-Weekly': 14,
    'Monthly': 30, 'Quarterly': 90, 'Every 3 Months': 90, 'Annually': 365
}
df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

**5. Drop Redundant Column** — `promo_code_used` was identical to `discount_applied`
```python
df = df.drop('promo_code_used', axis=1)
```

**6. Export**
```python
df.to_sql('customer', engine, if_exists='replace', index=False)  # PostgreSQL
df.to_csv('customer_data.csv', index=False)                      # CSV backup
```

---

## SQL Queries

**Folder:** [SQL_Queries](SQL_Queries)  
**File:** `Customer_Behavior_SQL_queries.sql`  
**Database:** PostgreSQL

| # | Question |
|---|---|
| Q1 | Total revenue by gender |
| Q2 | Discount users who spent above average |
| Q3 | Top 5 products by average review rating |
| Q4 | Average purchase — Standard vs Express shipping |
| Q5 | Subscriber vs non-subscriber spend comparison |
| Q6 | Top 5 products by discount rate |
| Q7 | Customer segmentation — New, Returning, Loyal |
| Q8 | Top 3 products per category |
| Q9 | Repeat buyers and subscription likelihood |
| Q10 | Revenue contribution by age group |

**Key SQL concepts used:** Aggregate functions, Subqueries, `CASE WHEN`, CTEs (`WITH`), Window functions (`ROW_NUMBER OVER PARTITION BY`)

---

## Power BI Dashboard

**Folder:** [PowerBI_Dashboard](PowerBI_Dashboard)  
**Tool:** Power BI Desktop  
**Data Source:** PostgreSQL → `customer` table

### KPIs
| Metric | Value |
|---|---|
| Total Orders | 3.9K |
| Average Purchase Amount | $59.8 |
| Average Review Rating | 3.75 |
| Total Subscribers | 1.1K |

### Visuals
- **% of Subscribed Customers** — 73% No, 27% Yes *(Donut Chart)*
- **Revenue by Category** — Clothing $104K, Accessories $74K, Footwear $36K, Outerwear $19K
- **Orders by Category** — Clothing 1.7K, Accessories 1.2K, Footwear 0.6K, Outerwear 0.3K
- **Revenue by Age Group** — Young Adult $62K, Middle Age $59K, Adult $56K, Senior $56K
- **Orders by Age Group** — evenly split ~1.0K per group

### Slicers
Subscription Status · Gender · Category · Shipping Type

---

## Key Insights

- **Clothing** is the top-performing category in both revenue and orders
- **73%** of customers are non-subscribers — a significant upsell opportunity
- Customers using discounts still match or exceed the average spend of $59.8
- Revenue is nearly uniform across age groups, with **Young Adults** slightly leading
- Repeat buyers (5+ purchases) show a higher tendency to subscribe

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python (Pandas) | Data cleaning & feature engineering |
| PostgreSQL | Data storage & SQL analysis |
| SQLAlchemy | Python–PostgreSQL connection |
| Power BI | Interactive dashboard |

---

## Conclusion

This project delivered a full-cycle analysis of customer shopping behavior. EDA ensured clean, well-structured data with meaningful features like age groups and purchase frequency. SQL queries revealed key patterns — discount users consistently spend above average, repeat buyers are more likely to subscribe, and **Clothing** dominates across all categories. Power BI made these findings accessible visually, confirming that revenue is evenly spread across age groups with Young Adults slightly ahead. The most critical takeaway: **73% of customers remain non-subscribers**, representing a major untapped opportunity for targeted retention and subscription-conversion strategies.
