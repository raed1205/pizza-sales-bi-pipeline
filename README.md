# Pizza Sales BI Pipeline — End-to-End Analytics & Data Warehouse

An end-to-end business intelligence pipeline that transforms 48,620 raw transactional order records into a PostgreSQL star-schema data warehouse using automated Talend ETL jobs, powering an interactive Power BI dashboard for strategic revenue analysis.

**Project Assets:** [Database Schemas](./database/) • [Talend ETL Jobs](./etl/) • [Power BI Report](./dashboards/)

---

## Overview

A pizza restaurant logged a full year (2015) of transactional data across disconnected CSV files without unified reporting. This repository contains the complete data engineering and analytics solution: source data modeling, ETL automation, star-schema data warehousing, and dynamic DAX reporting.

---

## Architecture

```mermaid
flowchart LR
    A[("PostgreSQL<br/>Source DB")] -->|Extract| B["Talend Open Studio<br/>ETL Jobs"]
    B -->|Transform & Load| C[("PostgreSQL<br/>Star-Schema DW")]
    C -->|Direct Connect| D["Power BI<br/>Dashboard"]

    style A fill:#4169E1,color:#fff
    style B fill:#FF6D70,color:#fff
    style C fill:#336791,color:#fff
    style D fill:#F2C811,color:#000
