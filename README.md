# Pizza Sales BI Pipeline

End-to-end Business Intelligence pipeline transforming transactional sales data into a Power BI dashboard using a PostgreSQL star-schema data warehouse and automated Talend ETL pipeline.

<p>
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white"/>
  <img src="https://img.shields.io/badge/Talend%20Open%20Studio-FF6D70?style=for-the-badge&logo=talend&logoColor=white"/>
  <img src="https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black"/>
  <img src="https://img.shields.io/badge/SQL-025E8C?style=for-the-badge&logo=databricks&logoColor=white"/>
</p>

---

## Business Problem

A pizza restaurant recorded 2015 transactional data across separate operational tables without an analytical framework to evaluate sales trends, peak ordering hours, or category revenue distribution. This project establishes an end-to-end data pipeline: extracting raw source data, modeling a star-schema data warehouse, automating ETL execution, and deploying an interactive reporting dashboard.

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
