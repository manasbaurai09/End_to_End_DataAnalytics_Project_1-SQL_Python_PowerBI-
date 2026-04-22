# Customer Behavior — Power BI Dashboard

<img width="1347" height="736" alt="image" src="https://github.com/user-attachments/assets/691d587c-7a2b-470b-8d6f-c9fb9c5cbd9a" />



An interactive Power BI dashboard built on the cleaned customer dataset, providing a visual overview of revenue, orders, subscriptions, and customer segmentation.

---

## Dashboard Filters (Slicers)

| Filter | Options |
|---|---|
| Subscription Status | Yes / No |
| Gender | Female / Male |
| Category | Accessories, Clothing, Footwear, Outerwear |
| Shipping Type | 2-Day Shipping, Express, Free Shipping, Next Day Air |

All visuals respond dynamically to these slicers.

---

## KPI Cards

| Metric | Value |
|---|---|
| Total Orders | 3.9K |
| Average Purchase Amount | $59.8 |
| Average Review Rating | 3.75 |
| Total Subscribers | 1.1K |

---

## Visuals

### % of Subscribed Customers *(Donut Chart)*
- **73%** of customers are non-subscribers
- **27%** are active subscribers

### Revenue by Category *(Bar Chart)*
| Category | Revenue |
|---|---|
| Clothing | $104K |
| Accessories | $74K |
| Footwear | $36K |
| Outerwear | $19K |

### Orders by Category *(Bar Chart)*
| Category | Orders |
|---|---|
| Clothing | 1.7K |
| Accessories | 1.2K |
| Footwear | 0.6K |
| Outerwear | 0.3K |

### Revenue by Age Group *(Horizontal Bar Chart)*
| Age Group | Revenue |
|---|---|
| Young Adult | $62K |
| Middle Age | $59K |
| Adult | $56K |
| Senior | $56K |

### Orders by Age Group *(Horizontal Bar Chart)*
| Age Group | Orders |
|---|---|
| Young Adult | 1.0K |
| Middle Age | 1.0K |
| Senior | 0.9K |
| Adult | 0.9K |

---

## Key Insights

- **Clothing** dominates both revenue and orders across all categories
- **73%** of customers are non-subscribers, indicating a large upsell opportunity
- Revenue is fairly evenly distributed across age groups, with **Young Adults** slightly ahead
- Average purchase amount holds steady at **$59.8** regardless of segment

---

## Tools Used

- **Power BI Desktop** — dashboard creation
- **Data Source** — `customer` table from PostgreSQL (`customer_behaviour_analysis`)
