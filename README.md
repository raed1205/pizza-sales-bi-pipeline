# Pizza Sales BI Pipeline

End-to-end Business Intelligence pipeline transforming transactional pizza sales data into a Power BI dashboard, built on a PostgreSQL star-schema data warehouse with an automated Talend ETL layer.

<p>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Talend%20Open%20Studio-FF6D70?style=for-the-badge&logo=talend&logoColor=white"/>
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/SQL-025E8C?style=for-the-badge&logo=databricks&logoColor=white"/>
</p>

---

## Documentation & Reports

 **[View Full Project Report (PDF)](database/PizzaSales_BI_Project.pdf)**

---

## Business Problem

A pizza restaurant collected a full year (2015) of sales data across separate operational database tables. Without a central reporting system in place, management could not easily track monthly trends, peak ordering hours, or product performance. This project builds an end-to-end BI solution: modeling a star-schema data warehouse, automating data transformation with Talend, and delivering an interactive Power BI dashboard.

---

## Architecture

```mermaid
flowchart LR
    A[("PostgreSQL<br/>Source DB<br/>pizza_source")] -->|Extract| B["Talend Open Studio<br/>ETL Pipeline"]
    B -->|Transform & Load| C[("PostgreSQL<br/>Star-Schema DW<br/>pizza_dw")]
    C -->|Connect & Model| D["Power BI<br/>Dashboard"]

    style A fill:#4169E1,color:#fff
    style B fill:#FF6D70,color:#fff
    style C fill:#336791,color:#fff
    style D fill:#F2C811,color:#000
```

| Component | Technology | Purpose |
|---|---|---|
| **Source DB** | PostgreSQL 16 | Operational database hosting raw transactional records |
| **ETL Layer** | Talend Open Studio | 4 automated jobs for data cleaning, mapping, and loading |
| **Data Warehouse** | PostgreSQL | Star-schema repository optimized for analytical queries |
| **Analytics Layer** | Power BI Desktop | Direct connectivity for DAX calculations and executive dashboards |

---

## Dataset Overview

Source dataset contains 48,620 transactional order records from 2015.

| File | Records | Key Columns | Description |
|---|---|---|---|
| `orders.csv` | 21,350 | `order_id`, `date`, `time` | Order timestamps |
| `order_details.csv` | 48,620 | `order_details_id`, `order_id`, `pizza_id`, `quantity` | Itemized order quantities |
| `pizzas.csv` | 96 | `pizza_id`, `pizza_type_id`, `size`, `price` | Pizza size variants and prices |
| `pizza_types.csv` | 32 | `pizza_type_id`, `name`, `category`, `ingredients` | Pizza categories and ingredients |

---

## Data Warehouse Model

Star-schema configuration optimized for analytical querying, featuring a central fact table surrounded by three dimensional tables.

```mermaid
erDiagram
    dim_date ||--o{ fact_sales : "date_id"
    dim_order ||--o{ fact_sales : "order_id"
    dim_pizza ||--o{ fact_sales : "pizza_id"

    fact_sales {
        int fact_id PK
        int order_details_id
        int order_id FK
        int date_id FK
        int pizza_id FK
        int quantity
        decimal total_price
    }
    dim_date {
        int date_id PK
        date full_date
        int day
        int month
        string month_name
        int quarter
        int year
        string day_of_week
    }
    dim_order {
        int order_id PK
        date order_date
    }
    dim_pizza {
        int pizza_id PK
        int pizza_type_id
        string name
        string category
        string size
        decimal price
        string ingredients
    }
```

| Table | Type | Primary Key | Foreign Keys |
|---|---|---|---|
| `fact_sales` | Fact | `fact_id` | `order_id`, `date_id`, `pizza_id` |
| `dim_date` | Dimension | `date_id` | N/A |
| `dim_order` | Dimension | `order_id` | N/A |
| `dim_pizza` | Dimension | `pizza_id` | N/A |

---

## ETL Pipeline Summary

Implemented using four dedicated Talend Open Studio jobs:

| Job | Description | Source → Target | Rows Loaded |
|---|---|---|---|
| `Load_DIM_DATE` | Extracts unique dates and calculates calendar fields like day, month, quarter, and year | `pizza_source` → `dim_date` | 358 |
| `Load_DIM_ORDER` | Cleans order records and normalizes timestamps | `pizza_source` → `dim_order` | 21,350 |
| `Load_DIM_PIZZA` | Combines pizza items with their category metadata | `pizza_source` → `dim_pizza` | 96 |
| `Load_FACT_SALES` | Performs lookup joins against dimensions and calculates total sales (`quantity * price`) | `pizza_source` + `pizza_dw` → `fact_sales` | 48,620 |

---

## Core Analytics & DAX Measures

```dax
Total Revenue = SUM(fact_sales[total_price])

Total Orders = DISTINCTCOUNT(fact_sales[order_id])

Total Quantity = SUM(fact_sales[quantity])

Avg Order Value = DIVIDE([Total Revenue], [Total Orders])
```

---

## Dashboard Preview

![Pizza Sales Dashboard](database/Pizza.png)

---

## Key Findings

* **Seasonality:** Revenue peaks during May and July, while September and October see the lowest sales numbers of the year.
* **Quarterly Trends:** Sales stay steady throughout the year (around 25% per quarter), with Q1 and Q2 slightly ahead.
* **Category Performance:** Classic pizzas bring in the highest overall revenue, followed by Supreme, Chicken, and Veggie categories.
* **Busiest Days:** Friday is by far the busiest day for sales, followed by Thursday and Wednesday. Sunday generates the lowest revenue.

---

## Business Recommendations

* **Boost Off-Peak Months:** Run targeted promotions and special discounts during September and October to maintain steady revenue during slow months.
* **Optimize Staffing and Inventory:** Schedule extra kitchen staff and prepare popular ingredients ahead of time for peak order volumes on Thursday and Friday evenings.
* **Focus on Best Sellers:** Direct marketing campaigns toward the top-performing Classic pizza category to capitalize on established customer preferences.
* **Increase Sunday Orders:** Introduce Sunday-only meal deals or family delivery specials to lift order volume on the lowest-performing day of the week.

---

## Tools Used

| Tool | Purpose |
|---|---|
| PostgreSQL 16 | Relational operational database and star-schema warehouse |
| Talend Open Studio | Data extraction, transformation, and automated loading |
| Power BI Desktop | Data modeling, DAX measures, and interactive reporting |

---

## Authors

Academic project developed for Modelisation et entrepôt des données curriculum at **Esprit School of Business (ESB)**.

* **Raed Meddeb:** Data Warehouse Architecture and ETL Pipeline Development
* **Wajdi Riahi:** Data Warehouse Schema Design
* **Noureddine Chehimi:** Power BI Dashboard and DAX Measures Implementation

*Supervised by Mrs. Dalila Amara.*
