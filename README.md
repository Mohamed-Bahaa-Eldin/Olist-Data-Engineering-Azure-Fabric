
## 📋 Project Overview
This project demonstrates a robust, enterprise-grade Data Engineering pipeline utilizing the **Medallion Architecture** (Bronze, Silver, Gold). It integrates diverse data sources into a unified ecosystem using Azure-native tools for heavy lifting and Microsoft Fabric for modeling, orchestration, and business intelligence. 

All infrastructure and resources (ADLS Gen2, Data Factory, Databricks).


---

## 🏗️ Architecture Diagram
 
<img width="1536" height="836" alt="2" src="https://github.com/user-attachments/assets/b28033c0-322c-4793-926b-dbd5410e12a5" />

---

## 🛠️ Tech Stack
* **Data Sources:** GitHub (REST API), MySQL (Relational), MongoDB (NoSQL)
* **Ingestion:** Azure Data Factory (ADF)
* **Storage:** Azure Data Lake Storage Gen2 (ADLS Gen2)
* **Processing:** Azure Databricks (Apache Spark)
* **Data Modeling & Orchestration:** Microsoft Fabric (Pipelines, SQL Endpoints)
* **Business Intelligence:** Power BI (DirectLake Mode)

---

## 🎯 Business Logic & Problem Solving
Traditional flat-table exports often fail to provide accurate metrics due to data "fan-out." This project solves this by implementing a **Multi-Fact Star Schema** to address specific analytical needs:

### 1️⃣ Handling Granularity (The 3-Fact Strategy)
I designed three separate Fact tables to handle different business grains, which is essential for accurate reporting:
*   **Fact_Orders:** For high-level lifecycle and logistics tracking.
*   **Fact_Order_Items:** For granular SKU-level revenue and inventory analysis.
*   **Fact_Payments:** For monitoring installment success and payment method performance.

### 2️⃣ Advanced Feature Engineering (KPIs)
I created custom logic to extract insights that were missing from the raw data:
*   **Operational Health:** `Approved_Performance` (approval speed) and `Delivery_Performance` (Actual vs. Estimated).
*   **Efficiency:** `Total_Process_Day` (Total lead time from purchase to delivery).
*   **Data Trust:** `Red_Flag` logic to identify illogical data (e.g., delivery date occurring before purchase date).
*   **Time Analysis:** Splitting all timestamps into **Date** and **Time** dimensions to enable peak-hour sales analysis.

### 3️⃣ Dimension History (SCD)
To handle evolving business catalogs, I implemented **Dimension History (SCD Type 1)** for the Product table. If a product's classification or category changes in the source system, the pipeline automatically updates the Gold layer to reflect the latest hierarchy, ensuring Power BI reports are always up to date.

## 🚀 Pipeline Phases & Key Highlights

### 1️⃣ Ingestion: The Bronze Layer
* **The Process:** Extracting raw data from GitHub APIs, MySQL, and MongoDB, and loading it into the Bronze container in ADLS Gen2.
* **✨ What makes this special:** Handled multi-format data ingestion (handling JSON from NoSQL/APIs alongside structured relational data) using ADF's Copy Activities.
* **Screenshot:**
<img width="1341" height="383" alt="image" src="https://github.com/user-attachments/assets/6ca5b6a6-ee03-4b06-bd91-1d8fe063bb9b" />

<img width="557" height="633" alt="image" src="https://github.com/user-attachments/assets/c928a9de-e52f-4555-a2f6-91efed8b1058" />


### 2️⃣ Transformation: The Silver Layer
* **The Process:** Mounting ADLS Gen2 to Azure Databricks, reading the raw data, and performing extensive cleaning, schema enforcement, and deduplication using PySpark.
* **✨ What makes this special:** Heavy-duty Spark processing is used to unify disparate data sources. The cleaned data is written back to the Silver layer strictly in **Delta format**, unlocking ACID transactions, time-travel capabilities, and optimizing downstream read performance.
* **Screenshot:**
* <img width="587" height="660" alt="image" src="https://github.com/user-attachments/assets/5a951791-10f9-4ddb-b527-7f77866325d9" />


### 3️⃣ Dimensional Modeling: The Gold Layer
* **The Process:** Transitioning the Silver Delta tables into Microsoft Fabric. Writing  SQL DDL and DML scripts to transform the cleaned data into a business-ready Star Schema (Fact and Dimension tables).
* **✨ What makes this special:** Seamlessly bridged the Azure and Fabric ecosystems. By utilizing Fabric's SQL endpoints, the complex dimensional modeling is executed purely in SQL over Delta tables, making the data perfectly structured for analytical queries.
* **Screenshot:**
<img width="1201" height="891" alt="erd drawio" src="https://github.com/user-attachments/assets/039444a2-5b83-464c-8bb7-13983879337f" />


### 4️⃣ Master Orchestration
* **The Process:** Building a master pipeline in Microsoft Fabric to control the entire workflow sequentially.
* **✨ What makes this special:** Created a true "single pane of glass" orchestration. The Fabric Pipeline remotely triggers the ADF ingestion pipeline, executes the Databricks Silver notebooks, and finally runs the Fabric SQL scripts for the Gold layer. Built-in dependency management ensures jobs only proceed upon upstream success.
* **Screenshot:**
<img width="1303" height="290" alt="image" src="https://github.com/user-attachments/assets/dbae40a2-8cde-4561-95c3-5004c1fdff55" />


### 5️⃣ Serving & Business Intelligence
* **The Process:** Connecting Power BI to the Gold Layer in Fabric to build interactive dashboards.
* **✨ What makes this special:** Leveraged Power BI's **DirectLake mode**. This allows the dashboard to query the Delta/Parquet files directly from the lakehouse with the blazing-fast performance of Import mode, but without actually duplicating or importing any data into the BI semantic model.
* **Screenshot:**
<img width="1476" height="829" alt="image" src="https://github.com/user-attachments/assets/ec716c67-25db-4fa8-9ef7-e18caed4246d" />
<img width="1474" height="827" alt="image" src="https://github.com/user-attachments/assets/7e1385ff-86c0-443a-8c08-1721846ff7de" />
<img width="1477" height="830" alt="image" src="https://github.com/user-attachments/assets/0e39f21f-831d-4137-8dc6-886aac2987d0" />
<img width="1474" height="831" alt="image" src="https://github.com/user-attachments/assets/c6c96e57-98d0-4eab-abff-18e363baa6fe" />
<img width="1474" height="828" alt="image" src="https://github.com/user-attachments/assets/f30467fa-0bfd-4e98-923c-2cd67c93558a" />





---
*Developed by Mohamed Bahaa*
