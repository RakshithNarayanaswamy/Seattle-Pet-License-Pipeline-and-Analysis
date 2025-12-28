# 🐾 Seattle Pet License — Data Engineering & Analytics Project

## 📌 Overview

This project builds an **end-to-end data engineering & analytics pipeline** using the **Seattle Pet License dataset** from the City of Seattle Open Data Portal.

The goal is to design a **star-schema data warehouse model** and enable analytics such as:

✔ License trends over time  
✔ Pet ownership distribution by species & breed  
✔ Geographic license density by city & ZIP code  
✔ Seasonal registration patterns

The solution is implemented using **Azure Data Factory, Azure Blob Storage, SQL Server, Snowflake, and Tableau / Power BI** — following modern cloud data-engineering best practices.

---

## 🎯 Business Questions

**License & Ownership**

- How many licenses are issued per **month, quarter, and year**?
- What % of licenses belong to **dogs vs cats vs other species**?
- Which **breeds or breed combinations** are most common?

**Geography**

- Which **cities and ZIP codes** show the highest license registrations?
- Are there regions with **lower compliance or under-registration?**

**Seasonality**

- Do **license renewals peak at certain times?**

---

## ⭐ Data Model — Star Schema

### 🟡 Fact Table — `FACT_PET_LICENSE`

| Field          | Description                |
| -------------- | -------------------------- |
| fact_key       | Surrogate key              |
| license_number | Natural license identifier |
| date_key       | Foreign key — license date |
| species_key    | Foreign key — species      |
| breed_key      | Foreign key — breed        |
| location_key   | Foreign key — city / ZIP   |
| pet_name       | Pet name                   |
| license_count  | Always = 1                 |

> **Lowest grain = one row per license issued**

---

### 🔵 Dimension Tables

#### 📅 `DIM_DATE`

- Date
- Year
- Month
- Quarter

#### 📍 `DIM_LOCATION`

- ZIP Code
- City
- County
- State

#### 🐕 `DIM_BREED`

- Primary Breed
- Secondary Breed
- Species Association

#### 🐾 `DIM_SPECIES`

- Dog
- Cat
- Other

---

# ☁ Cloud Data Engineering Implementation (Azure + Snowflake)

This project was implemented fully in **Azure Data Factory (ADF), Azure Storage, SQL Server & Snowflake**.

---

## 🔹 Step 1 — Source System: SQL Server ➜ Parquet

I first **connected to an on-prem / SQL Server dataset using Azure Data Factory**.

From SQL Server:

- Data was extracted using **ADF Linked Service & Datasets**
- Then processed through an **ADF Mapping Data Flow**
- Finally written as **Parquet files into Azure Blob Storage containers**

This allowed me to store optimized, compressed analytics-friendly data.

Benefits of Parquet:

✔ Column-store  
✔ Highly compressed  
✔ Great for Snowflake & BI tools  
✔ Faster reads & processing

---

## 🔹 Step 2 — CSV ➜ Parquet Conversion (ADF Data Flows)

In addition to SQL Server ingestion, I also processed **CSV datasets** provided in the project.

Using **ADF Mapping Data Flows**, CSV files were:

- Cleaned
- Validated
- Transformed
- Converted into **Parquet files**
- Stored in **Azure Blob Storage Containers**

This ensured **consistent format across all data sources**.

---

## 🔹 Step 3 — Secure Credentials with Azure Key Vault

All sensitive credentials — such as:

🔐 SQL Server credentials  
🔐 Snowflake credentials  
🔐 Storage account keys

were stored securely in **Azure Key Vault**, and retrieved dynamically in ADF.

This ensures:

✔ No secrets hard-coded  
✔ Role-based security  
✔ Auditability

---

## 🔹 Step 4 — Parameterized & Reusable Pipelines

ADF pipelines were built using **parameters**, meaning:

- File paths
- Container names
- Environment values

…can be **changed without modifying logic**.

This makes the pipeline reusable and environment-agnostic.

---

## 🔹 Step 5 — Pipeline Integration with Snowflake

ADF pipelines then **loaded Parquet data from Blob Storage into Snowflake staging tables**, followed by merging into:

⭐ `FACT_PET_LICENSE`  
⭐ `DIM_DATE`  
⭐ `DIM_LOCATION`  
⭐ `DIM_BREED`  
⭐ `DIM_SPECIES`

Snowflake loads were performed using:

✔ COPY INTO  
✔ MERGE logic  
✔ Referential integrity controls

---

## 🔹 Step 6 — Master Orchestration Pipeline

All tasks were chained with **On-Success execution**, ensuring full automation:

1️⃣ SQL Server ➜ Parquet Export  
2️⃣ CSV ➜ Parquet Conversion  
3️⃣ Blob Storage ➜ Snowflake Stage Load  
4️⃣ Stage ➜ Fact & Dimensions Load

The result = **a fully automated ETL / ELT workflow**.

---

## 📊 Analytics & Dashboards

Dashboards were built in **Tableau / Power BI** enabling insights such as:

📍 Licenses by City & ZIP (Heat Map)  
![image](https://github.com/RakshithNarayanaswamy/Seattle-Pet-License-Pipeline-and-Analysis/blob/main/images/Screenshot%202025-12-28%20at%204.21.24%E2%80%AFAM.png)
📈 Licenses Over Time  
![image](https://github.com/RakshithNarayanaswamy/Seattle-Pet-License-Pipeline-and-Analysis/blob/main/images/Screenshot%202025-12-28%20at%204.20.37%E2%80%AFAM.png)
🟢 Species Distribution  
![image](https://github.com/RakshithNarayanaswamy/Seattle-Pet-License-Pipeline-and-Analysis/blob/main/images/Screenshot%202025-12-28%20at%204.21.08%E2%80%AFAM.png)
🐕 Top Breeds & Breed Combinations  
📆 Seasonal Trends

These support:

✔ Community planning  
✔ Licensing awareness  
✔ Ownership pattern analysis

---

## 🔍 Data Quality & Validation

✔ ZIP stored as **string** to preserve leading zeros  
✔ Handled nulls / incomplete records  
✔ Referential integrity enforced  
✔ Deduplicated license keys  
✔ Surrogate keys applied

---

## 🧰 Tech Stack

- Azure Data Factory
- Azure Blob Storage
- Azure Key Vault
- SQL Server
- Snowflake
- Parquet
- SQL MERGE / Dynamic Tables
- Tableau / Power BI

---

## 🚀 Business Impact

This project enables:

✔ Data-driven insight into pet ownership  
✔ Geographic mapping of pet license activity  
✔ Improved license compliance visibility  
✔ Support for city animal services planning

---

## 🙌 Acknowledgments

Dataset — City of Seattle Open Data Portal  
Built as part of — Data Architecture & Business Intelligence coursework

---

## 📌 Summary

This project demonstrates how **Azure Data Factory + Snowflake + Parquet + BI tools** can transform raw public datasets into a **secure, scalable analytics platform** — enabling deep insight into **Seattle pet licensing trends** and community behavior.
