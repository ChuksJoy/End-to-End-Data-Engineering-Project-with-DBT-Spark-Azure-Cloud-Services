# 🏗️ End-to-End Data Engineering Project (Azure + Databricks + Spark + dbt)

This repository contains my implementation of a full **end-to-end Data Engineering pipeline** using **Azure Databricks, Apache Spark, Azure Data Factory (ADF), dbt**, and **ADLS Gen2** following a YouTube tutorial.

 **YouTube Tutorial Reference:**  
*Project guided by*: [Youtube Tutorial by codewithyu](https://www.youtube.com/watch?v=divjURi-low&list=PL_Ct8Cox2p8UlTfHyJc3RDGuGktPNs9Q3)  

##  **Project Overview**

This project demonstrates how to build a modern Medallion-style data architecture in Azure.  
It covers:

- Data ingestion from Azure SQL DB to ADLS Gen2  
- Orchestration using Azure Data Factory  
- Transformation using Azure Databricks (Spark)  
- Modular SQL transformation using dbt  
- Secret management with Azure Key Vault  
- Medallion architecture (Bronze → Silver → Gold)

---

##  **System Architecture**

###  Tools & Technologies
- **Azure Cloud**
- **Azure Data Factory (ADF)**
- **Azure Databricks (Spark engine)**
- **Azure Key Vault**
- **Azure SQL Database**
- **Azure Data Lake Gen2 (ADLS)**
- **dbt (Data Build Tool)**

###  Medallion Architecture
- **Bronze** → Raw ingestion  
- **Silver** → Cleaned/transformed  
- **Gold** → Curated for analytics  

---

##  **Step-By-Step Implementation**

### 1️⃣ **Microsoft Azure Setup**
- Created a **free Azure account**
- Created a resource group named **medallion**

---

### 2️⃣ **Storage Account Setup for Medallion Layers**
Created an ADLS Gen2 storage account with three containers:

- `bronze`
- `silver`
- `gold`

---

### 3️⃣ **Azure Data Factory Setup**
Created an Azure Data Factory instance:

- `medallionazuredatafactory`

Inside ADF:

#### ✔️ Linked Services
- Azure SQL DB (source)
- ADLS Gen2 (destination → bronze layer)

#### ✔️ Pipeline Creation
- Created an ADF pipeline for ingestion
- Added a **Lookup** to fetch table names dynamically
- Created a new dataset (`tablesquery`) to read metadata from Azure SQL DB
- Used the lookup query:

```sql
select * from [ivory-medalliondb-dev].information_schema.tables
where table_schema = 'SalesLT'
and table_type = 'BASE TABLE'
