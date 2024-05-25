
This blog comprises of implementing Medallion architecture in Azure Databricks. Before delving into the implementation stuff . Lets dig into what medallion architecture is about?
# Problem Statement
This dataset from Kaggle provides information of orders made at Olist Store, a Brazilian Ecommerce, from 2016 to 2018.
In this project, we will build a data pipeline to ingest and process the data using Medallion Architecture, and eventually visualise the data in PowerBI. We will build a dashboard to find out the states with the most number of orders made and the daily volume of orders from these states.
# Dashboard
![olist ecommerce dashboard](https://github.com/geetanjalich/Medallion-architecture-Project-on-Sales-data/assets/79563879/0f33d658-827c-4a8e-8662-f711add40083)

# What is Medallion Architecture?

Medallion architecture serves different business use cases. Generally according to Databricks documentation the medallion architecture consists of three main layers: Bronze, Silver, Gold. As we progress from bronze to Gold layer, the quality of data improves. Bronze layer on the other hand consists of the raw data which can be further cleaned and curated data to silver layer. Gold layer consists of highly curated data ideal for analysis purpose. A medallion architecture is built using Databricks on Data Lake. The filename will be orchestrated using Azure Data Factory pipelines. The Medallion architecture consists of three main layers: Bronze, Silver, and Gold. It aims to incrementally and progressively improve the structure and quality of data as it flows through each layer of the architecture.


# 2. Environment Setup
If you are not familiar with the azure services . Let me give a brief overview of services we are going to use.
Azure Data Factory: ADF is the ETL tool used for data ingestion. 
Azure Data Lake Gen 2: This is where all the data are stored.
Azure Databricks: Azure Databricks transformed the data into the required format.
Azure Synapse Analytics: This tool will load the Azure SQL database data.
Power BI: To load data from Azure Synapse Analytics and Create reports for business use cases.

# 3. Data Architecture:
The end-to-end architecture for this project is below: We assume here the raw data is already present in landing directory in storage account in CSV format. Using Azure Data Factory we perform ingestion to bronze layer which will contain files in parquet format. Then Databricks will be used to perform data ingestion to ADLS Silver Layer, and transformation to the ADLS Gold layer. Later data will be analyzed and visualized through Synapse analytics and Power BI. Azure Data Factory will be an orchestration tool to monitor and schedule the pipeline. 

 ![image](https://github.com/geetanjalich/medallion-architecture-project-02/assets/79563879/182a98d3-b625-4901-82ef-777abea61f0f)
 ![image](https://github.com/geetanjalich/medallion-architecture-project-02/assets/79563879/3fcd61f2-f624-40bb-a26c-a72f5a66a962)

Landing: We consider the data to be already present in the landing directory.
Bronze (Raw) : Storing your source system’s data unchanged, contain duplicates, uncleaned
Silver (Cleansed) : Storing deduplicated, cleansed, missing values replaced, same grain as source bronze/raw data.
Gold (Curated) : Storing data modelled for analysis, e.g. Star Schema, Aggregate tables. This is the highest quality data ready for reporting. You can decide on the grain you want to aggregate the data to.

You can also have additional layers which totally depends on the requirements you have.
Here, I have taken 3 layers into consideration: Bronze, Silver, Gold
