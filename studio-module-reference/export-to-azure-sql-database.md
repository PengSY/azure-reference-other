---
title: "Export to Azure SQL Database | Microsoft Docs"
ms.custom: ""
ms.date: 03/02/2017
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: adc130f5-3b07-462d-8f61-17e0457c41eb
caps.latest.revision: 19
author: "jeannt"
ms.author: "jeannt"
manager: "cgronlun"
---
# Export to Azure SQL Database
Use this option in the [Export Data](export-data.md) module to export data from your machine learning experiment to an Azure SQL Database or Azure SQL Data Warehouse. This is useful in many scenarios: for example, you might want to store intermediate results, save scores, or persist tables of engineered features.  
  
 In general, storing data in an Azure SQL Database or Azure SQL Data Warehouse is more expensive than using tables or blobs in Azure. There may also be limits on the amount of data that you can store in a database, depending on your subscription type. However, there are no transaction fees against SQL databases. Moreover, database storage is ideal for quickly writing smaller amounts of frequently used information, for sharing data between experiments, or for reporting results, predictions, and metrics.  
  
 You will need to know the instance name and database name where the data is stored, and provide an account that has write permissions. You will also need to provide the table name and map the columns from your experiment to columns in the table.  
  
## How to Export Data to an Azure SQL Database  
  
1.  Add the [Export Data](export-data.md) module to your experiment. You can find this module in the [Data Input and Output](data-input-and-output.md) group in the **experiment items** list in Azure Machine Learning Studio. 

    Connect it to the module that produces the data that you want to export to Azure SQL DB.  
  
2.  For **Data destination**, select **Azure SQL Database**. This option supports Azure SQL Data Warehouse as well.  
  
3.  Set the following options specific to Azure SQL Database or Azure SQL Data Warehouse.  
  
     **Database server name**  
     Type the server name that is generated by Azure. Typically it has the form `<generated_identifier>.database.windows.net`.  
  
     **Database name**  
     Type the name of a database on the server you just specified.  
  
     The database must already exist; the [Export Data](export-data.md) cannot create it.  
  
     **Server user account name**  
     Type the user name of an account that has access permissions for the database.  
  
     **Server user account password**  
     Provide the password for the specified user account.  
  
     **Comma-separated list of columns to be saved**  
     Type the names of the columns in the experiment that you want to write to the database.  
  
     **Data table name**  
     Type the name of the table where data will be stored.  
  
     For Azure SQL Database, if the table does not exist, it will be created. For Azure SQL Data Warehouse, the table must already exist and have the correct schema, so be sure to create it in advance.  
  
     **Comma-separated list of datatable columns**  
     Type the names of the columns as you wish them to appear in the destination table. The columns should correspond in order with the column names that you list in **Comma-separated list of columns to be saved**.  
  
     if you are writing to Azure SQL Data Warehouse, the columns names must match those already in the destination table schema.  
  
     **Number of rows written per SQL Azure operation**  
     Indicate how many rows should be written to the destination table in each batch. By default, the value is set to 50, which is the default batch size for Azure SQL Database. However, you should increase this value if you have a large number of rows to write.  
  
    > [!TIP]
    >  For Azure SQL Data Warehouse, we recommend that you set this value to 1. If you use a larger batch size, the size of the command string that is sent to Azure SQL Data Warehouse can exceed the allowed string length, causing an error.  
  
4.  If you don't want to write new results each time you run the experiment, select the **Use cached results** option. If there are no other changes to module parameters, the experiment will write the data the first time the module is run, and thereafter not perform writes.  
  
     However, a write will always be performed if any parameters have been changed in [Export Data](export-data.md) that would change the results.  
  
5.  Run the experiment.  
  
##  <a name="Examples"></a> Examples  
 For examples of how to use the [Export Data](export-data.md) module, see these experiments and templates in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com):  
  
-   The Retail Forecasting template illustrates a machine learning task based on data stored in Azure SQLDB. It demonstrates useful techniques, such as using Azure SQL database to pass datasets between experiments in different accounts, saving and combining forecasts, and how to create an Azure SQL database just for machine learning.  
  
     [http://gallery.cortanaintelligence.com/Experiment/Retail-Forecasting-Step-1-of-6-data-preprocessing-5](http://gallery.cortanaintelligence.com/Experiment/Retail-Forecasting-Step-1-of-6-data-preprocessing-5)  
  
-   This article walks you through using a SQL Server database hosted in an Azure VM as a source for storing training data and predictions. It also illustrates how relational database can be used for feature engineering and feature selection.  
  
     [Build and deploy a machine learning model using SQL Server on an Azure VM](/azure/machine-learning/team-data-science-process/sql-walkthrough)  
  
-   This article demonstrates how you can use data in Azure SQL Data Warehouse to build a clustering model:  
  
     [How to use Azure ML with Azure SQL Data Warehouse](https://blogs.technet.microsoft.com/machinelearning/2016/03/08/how-to-use-azure-ml-with-azure-sql-data-warehouse/)  
  
-   This article demonstrates how to create a regression model to predict pricing, using data in Azure SQL Data Warehouse:  
  
     [Use Azure Machine Learning with SQL Data Warehouse](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-integrate-azure-machine-learning/)  
  
##  <a name="TechnicalNotes"></a> Technical Notes  
 This section contains some advanced configuration options and answers to commonly asked questions.  
  
-   **What happens if the database is in a  different geographical region?**?  
  
     If the Azure SQL Database or SQL Data Warehouse is in a different region from the machine learning account, writes will be slower. Further, there will be charges for data ingress and egress on the subscription if the compute node is in a different region than the storage account.  
  
-   **Why are some characters in the output data not displayed correctly?**  
  
     Azure Machine Learning supports the UTF-8 encoding. If string columns in your database use a different encoding, the characters might not be saved correctly.  
  
     Also, Azure Machine Learning cannot output data types such as `money`.  For more information, see [Module Data Types](machine-learning-module-data-types.md).  
  
##  <a name="parameters"></a> Module Parameters  
  
|Name|Range|Type|Default|Description|  
|----------|-----------|----------|-------------|-----------------|  
|Data source|List|Data Source Or Sink|Azure Blob Storage|Data source can be HTTP, FTP, anonymous HTTPS or FTPS, a file in Azure BLOB storage, an Azure table, an Azure SQL Database or Azure SQL Data Warehouse, a Hive table or an OData endpoint.|  
|Database server name|any|String|none||  
|Database name|any|String|none||  
|Server user account name|any|String|none||  
|Server user account password|||none||  
|Comma separated list of columns to be saved|||none||  
|Data table name|any|String|none||  
|Comma separated list of datatable columns|String|String|none|String|  
|Number of rows written per SQL Azure operation|String|Integer|50|String|  
|Use cached results|TRUE/FALSE|Boolean|FALSE|Module only executes if valid cache does not exist; otherwise use cached data from prior execution.|  
  
##  <a name="exceptions"></a> Exceptions  
  
|Exception|Description|  
|---------------|-----------------|  
|[Error 0027](errors/error-0027.md)|An exception occurs when two objects have to be the same size, but they are not.|  
|[Error 0003](errors/error-0003.md)|An exception occurs if one or more of inputs are null or empty.|  
|[Error 0029](errors/error-0029.md)|An exception occurs when an invalid URI is passed.|  
|[Error 0030](errors/error-0030.md)|an exception occurs in when it is not possible to download a file.|  
|[Error 0002](errors/error-0002.md)|An exception occurs if one or more parameters could not be parsed or converted from the specified type to the type required by the target method.|  
|[Error 0009](errors/error-0009.md)|An exception occurs if the Azure storage account name or the container name is specified incorrectly.|  
|[Error 0048](errors/error-0048.md)|An exception occurs when it is not possible to open a file.|  
|[Error 0015](errors/error-0015.md)|An exception occurs if the database connection has failed.|  
|[Error 0046](errors/error-0046.md)|An exception occurs when it is not possible to create a directory on specified path.|  
|[Error 0049](errors/error-0049.md)|An exception occurs when it is not possible to parse a file.|  
  
## See Also  
 [Import Data](import-data.md)   
 [Export Data](export-data.md)   
 [Export to Azure Blob Storage](export-to-azure-blob-storage.md)   
 [Export to Hive Query](export-to-hive-query.md)   
 [Export to Azure Table](export-to-azure-table.md)