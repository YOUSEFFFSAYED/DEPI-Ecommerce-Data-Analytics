# E-Commerce Marketplace Analytics (2024–2026)

> An end-to-end data analytics project exploring multi-region e-commerce marketplace performance using Python, SQL, and Power BI — moving from raw database queries to interactive dashboard visualization and executive reporting.

---

## 👥 Team & Ownership

This project was built collaboratively by a 6-person team as part of the Digital Egypt Pioneers Initiative (DEPI), with each member owning a distinct analytics domain:

| Member | Domain & Primary Contributions |
| :--- | :--- |
| **Yousef Sayed** | **Customer Lifecycle & Activation Analysis**<br>Evaluated time-to-first-order cadences, activation velocity, and repeat purchase frequencies. |
| **Rojeh Tamer** | **Data Model Architecture, DAX Calculations & Power BI Dashboard**<br>Designed relational data modeling, implemented core DAX measures, and developed interactive Power BI dashboard layouts. |
| **Hady Abohany** | **Revenue & Profitability Analysis & Presentation Lead**<br>Analyzed monthly revenue velocity, discount policy stability, and regional profit margin distributions. |
| **Ahmed Mohamed** | **Customer Valuation & Top Spender Profiling**<br>Segmented top customer cohorts, purchase distribution metrics, and customer recency trends. |
| **Ibrahim Desouky** | **Product Portfolio & Device Channel Performance**<br>Mapped SKU category concentrations, desktop vs. mobile app splits, and basket size metrics. |
| **Ahmed Shaban** | **Return Rate Drivers & Operational Risk Analysis**<br>Identified top return triggers (damaged goods, size mismatches) and regional return percentages. |

---

## 📁 Repository Structure

```text
├── data/
│   ├── ecommerce_data.sql                    # SQL schema setup & analytical queries
│   └── ecommerce_data.xlsx                   # Complete raw marketplace dataset
├── notebooks/
│   └── ecommerce_project2_notebook_4.ipynb   # Exploratory Data Analysis (EDA) & Python workflows
├── dashboard/
│   ├── E_Commerce_Dashboard.pbix             # Dynamic Power BI dashboard workbook
│   └── assets/                               # Custom UI elements, background graphics & icons
│       ├── checklist.png
│       ├── dashboard_gradient_background.png
│       ├── dollar-cost-average.png
│       ├── financial-state-ment.png
│       ├── hand_holding_money.png
│       ├── profit-margin.png
│       └── trolley.png
├── presentation/
│   └── E-Commerce_Analytics_Presentation.pdf # Executive slide deck prepared for C-level reporting
└── README.md                                 # Project documentation
```
## 📊 Dataset Scope & Data Model

The analysis covers 32 months of operational data across 6 regions (**Cairo, Giza, Alexandria, Jeddah, Riyadh, Abu Dhabi**) and 2 device channels (**Web / App**).

```text
   ┌─────────────────┐             ┌─────────────────┐
   │    p2_users     │             │    p2_orders    │
   ├─────────────────┤             ├─────────────────┤
   │ PK  user_id     │1───────────*│ PK  order_id    │
   │     region      │             │ FK  user_id     │
   │     channel     │             │     order_dt    │
   │     signup_dt   │             │     gmv         │
   └─────────────────┘             │     discount    │
                                   │     cogs        │
                                   │     shipping    │
                                   │     region      │
                                   │     device      │
                                   └────────┬────────┘
                                            │ 1
                                            │
                                            │ *
   ┌─────────────────┐             ┌────────┴────────┐
   │   p2_returns    │             │ p2_order_items  │
   ├─────────────────┤             ├─────────────────┤
   │ PK,FK order_id  │*───────────1│ PK,FK order_id  │
   │       return_dt │             │ PK    sku       │
   │       reason    │             │       qty       │
   └─────────────────┘             │       price     │
                                   └─────────────────┘
```
| Table | Records | Key Fields | Purpose |
| :--- | :--- | :--- | :--- |
| **`p2_users`** | 3,000 | `user_id`, `region`, `channel`, `signup_dt` | Customer directory containing registration dates, channel source, and home region[cite: 1, 2]. |
| **`p2_orders`** | 15,000 | `order_id`, `user_id`, `order_dt`, `gmv`, `discount`, `cogs`, `shipping_cost`, `region`, `device` | Main order header data tracking financials, device type, and shipping destination[cite: 1, 2]. |
| **`p2_order_items`** | 38,655 | `order_id`, `sku`, `qty`, `price` | Line-item details capturing item quantities, price points, and SKU categories[cite: 1, 2]. |
| **`p2_returns`** | 1,531 | `order_id`, `return_dt`, `reason` | Returned order logs recording timestamps and categorized return reasons[cite: 1, 2]. |

## 📓 Notebook Pipeline (Python EDA)

The Jupyter Notebook (`notebooks/ecommerce_project2_notebook_4.ipynb`) structures the analytical workflow[cite: 1, 2]:

1. **Data Cleaning & Type Casting:** Parsed timestamps, validated missing values, and established relational integrity across tables.
2. **Customer Lifecycle Evaluation:** Measured time-to-first-order (avg. ~75.3 days) and repeat order gap (~72.8 days)[cite: 1, 2].
3. **Revenue Distribution:** Computed monthly revenue growth from Jan 2024 ($5.5K) to peak Mar 2025 ($245.4K)[cite: 1, 2].
4. **Product Analysis:** Evaluated revenue volume per product category and calculated average item price profiles[cite: 1, 2].

## 🗄️ SQL Analytics

The repository includes SQL scripts (`data/ecommerce_data.sql`) designed for aggregate analytics and business questions[cite: 1, 2]:

```sql
-- Monthly Revenue Growth (Before vs. After Discounts)
SELECT 
    strftime('%Y-%m', order_dt) AS month, 
    SUM(gmv) AS gross_revenue, 
    SUM(gmv - discount) AS net_revenue,
    ROUND((SUM(discount) * 100.0 / SUM(gmv)), 2) AS discount_rate_pct
FROM p2_orders 
GROUP BY month 
ORDER BY month ASC
```
## 📈 Power BI Dashboard & DAX Modeling

The interactive Power BI report (`dashboard/E_Commerce_Dashboard.pbix`) leverages explicit DAX measures for real-time aggregation and cross-filtering[cite: 2]:

### Core DAX Measures

* **Net Revenue:**
  ```dax
  Net Revenue = SUM(p2_orders[gmv]) - SUM(p2_orders[discount])
  ```
* **Total Margin:**
  ```dax
  Total Margin = SUM(p2_orders[gmv]) - SUM(p2_orders[discount]) - SUM(p2_orders[cogs]) - SUM(p2_orders[shipping_cost])
  ```
* **Margin Rate %:**
  ```dax
  Margin Rate % = DIVIDE([Total Margin], [Net Revenue], 0)
  ```
* **Return Rate %:**
  ```dax
  Return Rate % = DIVIDE(DISTINCTCOUNT(p2_returns[order_id]), DISTINCTCOUNT(p2_orders[order_id]), 0)
  ```
## 🎯 Executive Business Insights

* **Revenue Velocity:** GMV grew 45x from $5.5K (Jan 2024) to a peak of $245.4K (Mar 2025). Discount policy remained stable at ~10.5% throughout the growth window.
* **Profitability Consistency:** Net profit margins across all 6 regions are tightly aligned between **20.4% and 21.5%** (Alexandria highest at 21.50%, Abu Dhabi lowest at 20.40%).
* **High Customer Retention:** **96.6%** of purchasing customers placed repeat orders. Repeat transactions surpassed new order volume starting mid-2024.
* **Category Dominance:** Electronics (`ELEC`) is the primary revenue engine ($1.59M+ total sales) with an average item price of ~$165, double the next category (**SPORT** at ~$80).
* **Return Drivers:** Overall order return rate averages ~10%. Over 50% of total return volume stems from **"Damaged"** (441 items) and **"Wrong size"** (394 items).

---

## 🛠️ Tech Stack & Tools

* **Exploratory Data Analysis:** Python (`pandas`, `matplotlib`, `seaborn`)
* **Database Querying:** SQL (`SQLite`)
* **Business Intelligence & Reporting:** Power BI (`DAX`, `Data Modeling`), Microsoft Excel
* **Documentation & Design:** Markdown, Custom PNG Canvas UI Assets

---

## 👤 Maintainer

**Yousef Sayed**  
* *GitHub:* [@YOUSEFFFSAYED](https://github.com/YOUSEFFFSAYED)
