# Data Layers: Bronze, Silver, and Gold

This document summarizes how each layer of the Medallion Architecture is defined in this project, how each one is built, and which questions to ask when interviewing a source system.

> Based on learning material from [Data With Baraa](https://www.datawithbaraa.com).

---

## 1. Layer Comparison

| Property | 🥉 Bronze Layer | 🥈 Silver Layer | 🥇 Gold Layer |
|:--|:--|:--|:--|
| **Definition** | Raw, unprocessed data as-is from sources | Clean and standardized data | Business-ready data |
| **Objective** | Traceability and debugging | Intermediate layer: prepare data for analysis | Provide data to be consumed for reporting and analytics |
| **Object Type** | Tables | Tables | Views |
| **Load Method** | Full load (truncate and insert) | Full load (truncate and insert) | None |
| **Data Transformation** | None (as-is) | Data cleaning, standardization, normalization, derived columns, data enrichment | Data integration, data aggregation, business logic and rules |
| **Data Modeling** | None (as-is) | None (as-is) | Star schema, aggregated objects, flat tables |
| **Target Audience** | Data engineers | Data analysts, data engineers | Data analysts, business users |

---

## 2. Workflow for Building Each Layer

Every layer goes through the same four stages: **Analysing → Coding → Validating → Docs and Version**.

### 🥉 Bronze Layer

| Stage | Activity |
|:--|:--|
| **Analysing** | Interview source system experts |
| **Coding** | Data ingestion |
| **Validating** | Data completeness and schema checks |
| **Docs and Version** | Document and version in Git |

### 🥈 Silver Layer

| Stage | Activity |
|:--|:--|
| **Analysing** | Explore and understand the data |
| **Coding** | Data cleansing |
| **Validating** | Data correctness checks |
| **Docs and Version** | Document and version in Git. Documents: **data flow**, **data integration** |

### 🥇 Gold Layer

| Stage | Activity |
|:--|:--|
| **Analysing** | Explore and understand the business objects |
| **Coding** | Data integration |
| **Validating** | Data integration checks |
| **Docs and Version** | Document and version in Git. Documents: **data model**, **data catalog**, **data flow** |

---

## 3. Source System Interview

Before building the Bronze layer, I need to understand the source systems. These are the questions to ask the source system experts.

### Business Context and Ownership
- Who owns the data?
- What business process does it support?
- Is there system and data documentation?
- Is there a data model and data catalog?

### Architecture and Technology Stack
- How is the data stored? (SQL Server, Oracle, AWS, Azure, ...)
- What are the integration capabilities? (API, Kafka, file extract, direct DB, ...)

### Extract and Load
- Incremental or full loads?
- What are the data scope and historical needs?
- What is the expected size of the extracts?
- Are there any data volume limitations?
- How can I avoid impacting the source system's performance?
- What are the authentication and authorization requirements? (tokens, SSH keys, VPN, IP whitelisting, ...)

---

## 4. How This Applies to My Project

| Layer | In this project |
|:--|:--|
| **Bronze** | Six CSV files from the CRM and ERP are loaded as-is into SQL Server tables using full load |
| **Silver** | Data is cleaned, standardized, and enriched, with quality checks in `tests/` |
| **Gold** | CRM and ERP data is integrated into a star schema (`dim_customers`, `dim_products`, `fact_sales`) exposed as views. See [`data_catalog.md`](data_catalog.md) |
