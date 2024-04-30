
This blog comprises of implementing Medallion architecture in Azure Databricks. Before delving into the implementation stuff . Lets dig into what medallion architecture is about?

# What is Medallion Architecture?

Medallion architecture serves different business use cases. Generally according to Databricks documentation the medallion architecture consists of three main layers: Bronze, Silver, Gold. As we progress from bronze to Gold layer, the quality of data improves. Bronze layer on the other hand consists of the raw data which can be further cleaned and curated data to silver layer. Gold layer consists of highly curated data ideal for analysis purpose. A medallion architecture is built using Databricks on Data Lake. The filename will be orchestrated using Azure Data Factory pipelines. The Medallion architecture consists of three main layers: Bronze, Silver, and Gold. It aims to incrementally and progressively improve the structure and quality of data as it flows through each layer of the architecture.

# 1. Dataset:
In this project,  we will use the Sales dataset. You can find the dataset  from kaggle(https://www.kaggle.com/code/barirahzainal/my-project-brazilian-e-commerce-public). The data source is available in CSV files. We assume here that the data is already in azure data lake storage (ADLS Gen2). We will orchestrate the pipeline using Azure Data Factory to take the data from the raw layer and process it further into the next layers. You can check out the hierarchy of the medallion layers in the storage account be

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

- Landing: We consider the data to be already present in the landing directory.
- Bronze (Raw) : Storing your source system’s data unchanged, contain duplicates, uncleaned
- Silver (Cleansed) : Storing deduplicated, cleansed, missing values replaced, same grain as source bronze/raw data.
- Gold (Curated) : Storing data modelled for analysis, e.g. Star Schema, Aggregate tables. This is the highest quality data ready for reporting. You can decide on the grain you want to aggregate the data to.

You can also have additional layers which totally depends on the requirements you have.
Here, I have taken 3 layers into consideration: Bronze, Silver, Gold

# Data Ingestion and orchestration using Azure Data Factory

## Creating a connection between Azure Data Factory and Azure Data lake storage account

We can connect from Azure data factory to Azure data lake storage account using different ways. You can use access key(not recommended), Managed Identity (good practice), SAS, Service principal. For ease purpose I have used account key as an access to storage account. 
![image](https://github.com/geetanjalich/medallion-architecture-project-02/assets/79563879/65267170-de48-430b-9383-8bdb405e54e4)


## Data Ingestion to bronze layer

Ingestion of data to bronze layer is done using a lookup activity in Azure data factory. The lookup activity reads the file names in the lookup file . Using the file name it reads from Landing directory and translates the file to parquet format in bronze layer. We can also eliminate any file that is not useful or not relevant in this layer. A For Each activity iterates from the Lookup activity to copy all the CSV files. The For Each item would loop through all the files and copy the items listed from the lookup filename.

![image](https://github.com/geetanjalich/medallion-architecture-project-02/assets/79563879/be118359-19ba-422b-b65f-c71faf8f98db)


After completing the configuration of pipeline you can test the pipeline using debug option or trigger option. The successful completion of pipeline will result in parquet files in the bronze layer of the medallion architecture . Now the next step will be to further transform the data to ingest in the next layers for different use cases.




