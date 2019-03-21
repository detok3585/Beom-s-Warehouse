# Module 2 : Deploying HDInsight Cluster

### Module Overview

- Identifying HDInsight cluster type
- Managing HDInsight clusters by using the Azure Portal
- Managing HDInsight clusters by using Azure PowerShell

## Lesson 1 : Identifying HDInsight cluster types

- Introducing Apache Hadoop
- Describing Hadoop components
- Introducing Apache Spark
- Introducing Apache Storm 
- Introducing Apache HBase
- Introducing Interactive Hive in HDInsight
- Introducing Microsoft R Server
- Introducing Apache Kafka (데이터 수집 [중간에 끊겨도 이어서 수집가능])

### Introducing Apache Hadoop

- Apache Hadoop is a framework for distributed storage and processing of big datasets
- Hadoop in HDInsight cluster provides:
  - hadoop Distrubuted File System storage
- <u>Uses Azure Storage</u>
- Hadoop MapReduce parallel processes data in <u>chunks</u> across nodes in a cluster

### Describing Hadoop components

HDIndight Cluster Components:

- Ambari (UI를 지원하며 Hadoop의 전체 모니터링이 가능)
- Apache Pig
- Apache Sqoop
- Apache Hive 
- Apache Oozie
- Avro 
- Apache Phoenix
- YARN
- Apache Tez
- ZooKeeper
- Mahout

### Introducing Apache Spark 

> 특징 : Storage가 존재하지 않음, Memory = Spark의 성능 , 처리엔진만 존재

An Apache Spark Cluster on HDInsight includes the following modules and components:

- Spark Core
- Anaconda (Python 을 사용하기 위한 tool)
- Livy
- Jupyter Notebook

### Introducing Apache Storm

> 장점: 데이터를 처리 시 분산 node를 통해 많은 데이터 처리가 가능

- Advantage of Storm on HDInsight
- Topology: Spouts and bolts
- Common use cases:
  - Internet of Things (IoT)
  - Fraud detection
  - Social analytics
  - ETL processes
  - Network monitoring
  - Search

### Introducing Apache HBase

> 분산 프레임 이며, Column Based Data를 하나로 합칠 수 있다.

- Data management on Hbase
- Querying data
- The HBase Master user interface

### Introducing Interactive Hive in HDInsight

> Tez 를 통해 Memory에 캐슁을 통해 빠르게 처리할 수있다고 함.

- In-memory caching results in very fast performance
- Only contains the Hive service
- Accessible from <u>Ambari Hive view</u>, <u>Beeline</u>, and <u>Hive ODBC</u>

### Introducing Microsoft R Server

- HDInsight R Server:

  - Enterprise-class server

  - Manages <u>parallel</u> and <u>distributed workloads</u> of R processes and clusters

  - Included in HDInsight as a cluster type

  - More than 8,000 R packages, plus ScaleR included

- Completed R Models can be:

  - Scored within HDInsight

  - Scored in Azure Machine Learning

  - Scored on-premises

- Use Rstudio or SSH to access the R console in a cluster

### Introducing Apache Kafka

> 장점 : Node를 Scaling 을 통해 여러 node를 통해 데이터를 수집가능케 한다.

- Use Kafka for:

  - Message broking (한번에 많은 양의 Massage가 들어올때에 순차적으로 처리할 수 있도록 자동으로 delay time을 생성)

  - Publish-subscribe queuing (구독형태로 등록된 것에 한정하여 데이터 입력이 가능)

  - Real-time streaming pipeline

  - Messaging

  - Activity tracking

  - Aggregation

  - Transformation

- Benefits:

  - Horizontal scaling

  - Fault tolerance

## Lesson 2: Managing HDInsight clusters by using the Azure Portal
- Creating clusters
- Customizing clusters
- Configuring clusters by using Ambari
- Managing Azure services by using the Azure CLI
- Deleting clusters
- Demonstration: Creating and deleting HDInsight clusters by using the Azure
  Portal

### Creating clusters

- Cluster creation methods

- Cluster type (Hadoop, Spark, Storm, HBase, R Server가 있으며, 추가로 Kafka와 Interactive Hive가 있다.)

- Operating system

- HDInsight version

- Cluster tier

- Resource group

- Login user

- Storage

- Cluster location

### Customizing clusters

- Customize HDInsight clusters

  - Cluster scaling

  - Script action

  - Bootstrap

  - Cluster access control

### Configuring clusters by using Ambari

- Ambari web user interface

  - Dashboard view

  - Services view

  - Host view

  - Alerts view

  - Admin view

- Ambari views

  - YARN Queue Manager

  - Hive view

  - Tez view

### Managing Azure services by using the Azure CLI
- Azure CLI
- Log in to the Azure account
- Execution modes
- Creating the cluster
- Displaying the list of clusters
- Deleting the cluster
- Customizing the cluster

Log in with username and password

```azure
'azure login -u myUserName@contoso.onmicrosoft.com'
```

Log in with service principal 

```azure
'azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID'
```

Resource Manager mode

```azure
'azure config mode arm'
```

Service Management mode

```azure
'azure config mode asm'
```

Create the cluster

```
azure HDInsight Cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey Storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername
```



### Deleting clusters

- Deleting the cluster
- Different methods for deletion

### Demonstration: Creating and deleting HDInsight clusters by using the Azure Portal
In this demonstration, you will see how to:

- Create an HDInsight cluster by using the Azure Portal
- Delete the HDInsight cluster using the Azure Portal and storage account

## Lesson 3: Managing HDInsight clusters by using Azure PowerShell
- Creating clusters by using Azure PowerShell
- Customizing clusters by using Azure PowerShell
- Deleting clusters by using Azure PowerShell
- Demonstration: Using PowerShell to create and delete HDInsight clusters

### Creating clusters by using Azure PowerShell
- Azure PowerShell is an extension of Windows PowerShell

- Use Azure PowerShell to:

  - Log in to an Azure account and subscription

  - Create an Azure resource group

  - Create an Azure Storage account

  - Create an Azure Blob container

  - Create an HDInsight cluster

### Customizing clusters by using Azure PowerShell
- Scaling the cluster
- Cluster access control (grant and revoke)
- Display cluster information
- Upload data to cluster storage

### Deleting clusters by using Azure PowerShell
- Two ways to delete a cluster:
  - Delete a cluster in the resource group (or delete the resource group)

- Use Azure PowerShell

```powershell
Remove-AzureRmHDInsightCluster –ClusterName <Cluster Name>
```

### Demonstration: Using PowerShell to create and delete HDInsight clusters

In this demonstration, you will see how to:

- Create an HDInsight cluster by using Azure PowerShell
- Delete a cluster by using Azure PowerShell

## Lab: Managing HDInsight clusters by using the Azure Portal

### Lab Scenario

You are piloting HDInsight clusters on Machine Learning and you want to create HDInsight clusters that can access data that is held in Data Lake Store. You then want to customize these clusters by using script actions, implemented through the Azure Portal.

At the end of the lab, you will delete the cluster so that your account does not get charged.

- Exercise 1: Create an HDInsight cluster that uses Data Lake Store storage
- Exercise 2: Customize HDInsight by using script actions
- Exercise 3: Delete an HDInsight cluster