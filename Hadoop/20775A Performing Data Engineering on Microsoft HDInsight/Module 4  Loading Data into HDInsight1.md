# Module 4 : Loading Data into HDInsight

### Module Overview

- Storing data for HDInsight processing
- Using data loading tools
- Maximizing value from stored data



## Lesson 1: Storing data for HDInsight processing
- Azure Storage
- Azure Data Lake storage
- Demonstration: Azure Storage Explorer
- Demonstration: Create Data Lake storage and add it to an HDInsight cluster



### Azure Storage

``` 
wabs://<storageaccount>.blob.core.windows.net/<containerName>/<datafolder>/<yourfiles>
```

![image](https://user-images.githubusercontent.com/46669551/54805164-72313900-4cb9-11e9-9e9b-c560d03652cc.png)

### Azure Data Lake storage

- Azure Data Lake
  - ADLA
  - ADLS

```
adl://mydatalakestore.azuredatalakestore.net/myfoldername/allmydata.txt
```



### Demonstration: Azure Storage Explorer

In this demonstration, you will see how to:

- Create a Blob storage account using Azure Storage Explorer

![image](https://user-images.githubusercontent.com/46669509/54806693-25039600-4cbe-11e9-8824-31d6516d69e0.png)

- Use Azure Storage Explorer to upload a data file to the Blob storage account

![image](https://user-images.githubusercontent.com/46669509/54806780-6ac05e80-4cbe-11e9-84d3-3c5a73f054d8.png)

![image](https://user-images.githubusercontent.com/46669509/54806841-9a6f6680-4cbe-11e9-9a49-3dba2c26b4a7.png)

![image](https://user-images.githubusercontent.com/46669509/54806885-bd017f80-4cbe-11e9-8303-0778b5ed7e4a.png)

![image](https://user-images.githubusercontent.com/46669509/54806934-ede1b480-4cbe-11e9-85ed-f10ae4a29728.png)



![image](https://user-images.githubusercontent.com/46669509/54807007-31d4b980-4cbf-11e9-84ef-10906842f770.png)



![image](https://user-images.githubusercontent.com/46669509/54807023-3f8a3f00-4cbf-11e9-8920-f71121afb051.png)

![image](https://user-images.githubusercontent.com/46669509/54807139-9d1e8b80-4cbf-11e9-8547-9489687ed438.png)

![image](https://user-images.githubusercontent.com/46669509/54807126-942dba00-4cbf-11e9-94b9-d13edc31092e.png)

- Use Azure Storage Explorer to download data to your local machine

  ![image](https://user-images.githubusercontent.com/46669509/54807165-b7586980-4cbf-11e9-82bf-cb7e03d1937c.png)

- Check that file has downloaded correctly



### Demonstration: Create Data Lake storage and add it to an HDInsight cluster
In this demonstration, you will see how to:

- Create a Data Lake storage account
- Create an HDInsight cluster with access to the Data Lake storage



## Lesson 2: Using data loading tools

- Sqoop AzCopy
- Azure Data Lake Copy
- Azure Command Line Interface (Azure CLI)
- Demonstration: Managing storage using the Azure CLI



### Sqoop

```
$ Sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databasename>' --username <adminLogin> --password <adminPassword> --table <tablename> --target-dir 'wasbs:///<pathtodatastorage> ' 
```



### AzCopy

```
AzCopy /Source:<source> /Dest:<destination> [Options]
```



### Azure Data Lake Copy

- AdlCopy
- AdlCopy /source 

```
https://mystorage.blob.core.windows.net/mycluster/<data> /dest 
```

```
swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey 
```



### Azure Command Line Interface (Azure CLI)

- Azure CLI

```
az storage <target> <targetaction> <args>
```



### Demonstration: Managing storage using the Azure CLI

In this demonstration, you will see how to:

- Use the Azure CLI to manage storage accounts
  - Create a resource group
  - Add an Azure Blob storage account and ADLA account
  - Upload data
  - Download data



## Lesson 3: Maximizing value from stored data

- Compressing and serializing data
- Storing data in a data lake

### Compressing and serializing data

- Serialization
- Compression

![image](https://user-images.githubusercontent.com/46669551/54805456-609c6100-4cba-11e9-95b2-e316767acfc8.png)

### Storing data in a data lake

![image](https://user-images.githubusercontent.com/46669551/54805480-790c7b80-4cba-11e9-88a9-2fb897909703.png)



## Lab: Loading data into your Azure account

- Lab Scenario

This lab has two parts. First, you’ll identify an e-commerce store’s top-selling
item with HDInsight. To find this item, you’ll move data from a SQL database
into HDInsight, find the top-selling product, and export the results to SQL
Database. In the second scenario, you will upload a large file to Azure Blob
storage, transfer that file to ADLS, and compress it using Hadoop Streaming.

- Exercise 1: Load data for use with HDInsight