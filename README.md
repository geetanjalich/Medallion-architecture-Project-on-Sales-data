# Azure- Sales Data Analytics

# Architecture
![image](https://github.com/geetanjalich/medallion-architecture-project-02/assets/79563879/834698ca-45bf-4bed-b270-4213ed918145)

We assume here the raw data is already present in landing directory in storage account in CSV format. Using Azure Data Factory we perform ingestion to bronze layer which will contain files in parquet format. Then Databricks will be used to perform data ingestion to ADLS Silver Layer, and transformation to the ADLS Gold layer. Later data will be analyzed and visualized through Synapse analytics and Power BI. Azure Data Factory will be an orchestration tool to monitor and schedule the pipeline.

