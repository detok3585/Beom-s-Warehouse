# Module 4 : Loading Data into HDInsight

### Module Overview

- Storing data for HDInsight processing
- Using data loading tools
- Maximizing value from stored data

![image](https://user-images.githubusercontent.com/46669551/54888165-05e34f00-4ede-11e9-8942-daa87b07a8ea.png)

![image](https://user-images.githubusercontent.com/46669551/54888316-a128f400-4edf-11e9-96ea-7834c4d94e81.png)

### Lesson 1: Storing data for HDInsight processing

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

- Use Azure Storage Explorer to upload a data file to the Blob storage account

- Use Azure Storage Explorer to download data to your local machine

- Check that file has downloaded correctly

### Demonstration: Create Data Lake storage and add it to an HDInsight cluster
In this demonstration, you will see how to:

- Create a Data Lake storage account
- Create an HDInsight cluster with access to the Data Lake storage

### Lesson 2: Using data loading tools

- Sqoop AzCopy
- Azure Data Lake Copy
- Azure Command Line Interface (Azure CLI)
- Demonstration: Managing storage using the Azure CLI

### Sqoop (Sqoop 을 통한 접속)

```
$ Sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databasename>' --username <adminLogin> --password <adminPassword> --table <tablename> --target-dir 'wasbs:///<pathtodatastorage> ' 

 sqoop import --connect 'jdbc:sqlserver://dongxie.database.windows.net:1433;database=dongxie' --username admin2019 --password Pa$$w0rd --table 'VS_CUSTOMER' --target-dir 'wasbs:///https://dongxie2019sa.blob.core.windows.net/data '
```

#### SSH KEY를 통하여 Putty 접속(예시임)

```
SSH 셋팅을 통하여 접속할 때 사용
ssh sshuser@myhdinsight-ssh.azurehdinsight.net 
Putty를 통해 패스워드를 사용할 때 사용
sshuser@myhdinsight-ssh.azurehdinsight.net
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

```
azure login
```

```
azure account list
```

```
azure account set "<Subscription Name>"
```

```
azure provider register --namespace Microsoft.DataLakeStore
```

```
Azure group create -l eastus2 -n <RGname>
```

```
azure storage account create -l eastus2 -g <RGName> --sku-name GRS --kind BlobStorage --access-tier Hot <storageaccountname>
```

```
azure storage account connectionstring show <sotrageaccountname> -g <RGName>
```

```
azure storage container create clidata --connection-string <ConnectionString>
```

파일이 위치한 곳에서 Cmd를 실행한다.

```
azure storage blob upload -f <file.csv> --container clidata -c <ConnectionString>
```

```
azure storage blob list --container clidata -c <ConnectionString>
```

또 다른 파일이 위치한 곳으로 루트를 이동시킨다. 

```
azure storage blob upload -f <dile.csv> --container clidata -c <ConnectionString>
```

```
azure storage blob list --container clidata -c <ConnectionString>
```

아래는 DataLake를 생성하여 파일을 저장하는 Cmd

```
azure datalake store account create -n <ADLSName> -l eastus2 -g <RGName>
```

업로드할 파일이 위치한 루트로 이동한다.

```
azure datalake store filesystem import -n <ADLSName> -p <File.csv> -d Clidata/<folder>/<file.csv>
```

```
Azure datalake store filesystem list -n <ADLSName> -p clidata/<folder>
```

RG 지우기

```
azure group delet -n <RGName>
```



### Lesson 3: Maximizing value from stored data

- Compressing and serializing data
- Storing data in a data lake

### Compressing and serializing data

- Serialization (연속적)
- Compression (압축)

![image](https://user-images.githubusercontent.com/46669551/54805456-609c6100-4cba-11e9-95b2-e316767acfc8.png)

### Storing data in a data lake (전체적인 Process의 개요도)

![image](https://user-images.githubusercontent.com/46669551/54805480-790c7b80-4cba-11e9-88a9-2fb897909703.png)

## 	Lab: Loading data into your Azure account

- Lab Scenario

This lab has two parts. First, you’ll identify an e-commerce store’s top-selling
item with HDInsight. To find this item, you’ll move data from a SQL database
into HDInsight, find the top-selling product, and export the results to SQL
Database. In the second scenario, you will upload a large file to Azure Blob
storage, transfer that file to ADLS, and compress it using Hadoop Streaming.

- Exercise 1: Load data for use with HDInsight

```
* 항공데이터
$ wget http://stat-computing.org/dataexpo/2009/{1987..2008}.csv.bz2

$ bzip2 -d *.bz2
$ sed -e '1d' 1991.csv > 1991_new.csv
$ du -sh 
http://stat-computing.org/dataexpo/2009/the-data.html

1. HDInsight cluster Hadoop 설정 및 데이터 베이스 설정을 한 후 항공데이터를 다운받는다. 
2. 구글에서 7-zip 프로그램을 받은 다음 설치 후 
3. zip파일을 풀어야 한다. 
4. du -sh 의 명령어를 통해 전체 데이터가 12G임을 확인한다. 

```

