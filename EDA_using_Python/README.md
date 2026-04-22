# Customer Shopping Behavior — EDA

Exploratory Data Analysis on customer shopping behavior data using Python and Pandas, with final data loaded into a PostgreSQL database.

---

## Dataset

**Source file:** [Dataset](Dataset)

**Loaded into:** PostgreSQL → table `customer` in database `customer_behaviour_analysis`

---

## Steps Performed

### 1. Load Data
```python
import pandas as pd
df = pd.read_csv('customer_shopping_behavior.csv')
```

### 2. Handle Missing Values
Filled missing `Review Rating` values with the **median per category**:
```python
df['Review Rating'] = df.groupby('Category')['Review Rating'].transform(
    lambda x: x.fillna(x.median())
)
```

### 3. Clean Column Names
```python
df.columns = df.columns.str.lower().str.replace(' ', '_')
df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})
```

### 4. Feature Engineering

**Age Groups** — binned into 4 quartile-based groups:
```python
labels = ['Young_Adult', 'Adult', 'Middle_Aged', 'Senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)
```

**Purchase Frequency (in days)** — mapped text frequency to numeric days:
```python
frequency_mapping = {
    'Weekly': 7, 'Fortnightly': 14, 'Bi-Weekly': 14,
    'Monthly': 30, 'Quarterly': 90, 'Every 3 Months': 90, 'Annually': 365
}
df['purchase_frequency_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

### 5. Remove Redundant Column
`discount_applied` and `promo_code_used` were found to be identical, so `promo_code_used` was dropped:
```python
(df['discount_applied'] == df['promo_code_used']).all()  # True
df = df.drop('promo_code_used', axis=1)
```

### 6. Export Data

**PostgreSQL:**
```python
from sqlalchemy import create_engine
engine = create_engine("postgresql+psycopg2://postgres:password@localhost:5432/customer_behaviour_analysis")
df.to_sql('customer', engine, if_exists='replace', index=False)
```

**CSV backup:**
```python
df.to_csv('customer_data.csv', index=False)
```

---

## Key Libraries

| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, transformation |
| `sqlalchemy` | PostgreSQL connection |
| `psycopg2` | PostgreSQL driver |

---

## Final Columns

After cleaning, key columns include: `age`, `age_group`, `category`, `purchase_amount`, `review_rating`, `frequency_of_purchases`, `purchase_frequency_days`, `discount_applied`, and more.
