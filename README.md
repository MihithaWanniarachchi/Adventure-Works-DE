# Azure Data Pipeline for AdventureWorks Data

This project implements an end-to-end Azure data pipeline using **Azure Data Factory**, **Azure Databricks**, **Azure Synapse Analytics**, and **Power BI** to process and analyze AdventureWorks data.

### Architecture of the project
![Architecture](https://github.com/user-attachments/assets/da1b48fa-0133-468c-b90f-2e3696186ece)

### Layers:

- **Bronze Layer**: Raw data ingested from GitHub using Azure Data Factory
- **Silver Layer**: Cleaned and processed data stored using Azure Databricks
- **Gold Layer**: Curated, analytics-ready data served via Azure Synapse Analytics

![image](https://github.com/user-attachments/assets/8c26711d-ecf8-4084-b9ac-fd8679ad95e0)


All layers are stored in **Azure Data Lake Storage Gen2**.

## 🛠️ Technologies Used

| Tool/Service             | Purpose                                    |
|--------------------------|--------------------------------------------|
| Azure Data Factory       | Data ingestion (GitHub → ADLS Bronze)      |
| Azure Databricks         | Data transformation (Bronze → Silver)      |
| Azure Synapse Analytics  | Data modeling & querying (Silver → Gold)   |
| Azure Data Lake Storage  | Central data storage (all layers)          |
| Microsoft Entra ID       | Authentication and role assignment         |
| Power BI                 | Final visualization via Synapse SQL Pool   |

## ⚙️ Pipeline Flow

### 1️⃣ Ingest GitHub Data using Azure Data Factory (Bronze Layer)

- Create linked services in **Azure Data Factory**:
  - HTTP (GitHub)
  - Azure Data Lake (for source & sink)
- Build a **dynamic pipeline** using:
  - Parameters (`p_rel_url`, `p_sink_folder`, `d_sink_file`)
  - `Lookup` activity to read parameter list from a JSON file in ADLS
  - `ForEach` activity to iterate over the files
  - `Copy Data` activity inside `ForEach` to move files to ADLS Bronze
 
  ![image](https://github.com/user-attachments/assets/090cc209-7e71-4476-b42c-00fffad83750)

### 2️⃣ Process Data in Azure Databricks (Silver Layer)

- Created Databricks Workspace & Cluster
- Registered an app in Microsoft Entra ID
- Granted necessary storage permissions
- Mounted ADLS Gen2 to access raw data
- Processed data using PySpark:
  - Removed nulls, corrected data types and formats
- Stored cleaned data as Parquet files to Silver Layer

![image](https://github.com/user-attachments/assets/5984ab14-3c86-469d-afdd-f16c2384f676)


### 3️⃣ Curate & Query in Azure Synapse (Gold Layer)

- Created Synapse Workspace and SQL Pools
- Gave Synapse’s Managed Identity access to ADLS Gen2
- Queried Parquet files using `OPENROWSET`
- Transformed and stored outputs to Gold Layer

![image](https://github.com/user-attachments/assets/64637705-cc24-4edd-a1ac-4a2c2bef0e16)

### 4️⃣ Visualize in Power BI

- Connected Power BI Desktop to Synapse SQL endpoint
- Built dynamic dashboard.

## 📌 Key Highlights

- Fully Azure-native data pipeline (ADF, Databricks, Synapse, ADLS)
- Scalable Medallion architecture: Bronze → Silver → Gold
- Automated ingestion + transformation + SQL-based analytics


