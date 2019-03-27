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
- ZooKeeper (계층 구조를 구성하여 데이터 공유를 자유롭게 할 수 있다)
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

View the list of existing clusters

```
azure hdinsight cluster list
```

Show the details of a specific cluster

```
azure hdinsight cluster show <Cluster Name>
```

Delete the existing cluster

```
azure hdinsight cluster delete <Cluster Name>
```

Delete all of the clusters in a resource group

```
azure group delete <Resource Group Name>
```

Scale or resize the cluster

```
azure hdinsight cluster resize [option] <clusterName> <Target Instance Count>
```

```
azure logout -u <username>
```

 

### Deleting clusters

- Deleting the cluster
- Different methods for deletion

1. Azure Portal을 통하여 삭제가 가능
2. PowerShell

```powershell
Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME
```

3. Command-line interface

```commond-line
Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME
```



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

- Exercise 1: Create an HDInsight cluster that uses <u>Data Lake Store storage</u>

> Task 1 : Create a Data Lake Store account

![image](https://user-images.githubusercontent.com/46669509/54793611-8445b400-4c86-11e9-9016-194d1f86166a.png)

1. in the Azure Portal, click + New, click Storage, and then click Data Lake Storage

2. On the New Data Lake Store blade, type the following information

   ![image](https://user-images.githubusercontent.com/46669509/54793656-c5d65f00-4c86-11e9-9e05-00a0a6047c7d.png)

   - Name: wcbmagic3585 (예시) [반드시 유니크해야함]
   - Subscription: Azure Portal의 활성화 된 구독을 선택
   - Resource Group : datalake [한개로 통일할것이니 맞추는게 좋다]
   - Location : 자신이 위치한 곳으로 설정 그러나 대한민국은 없다. [미국 동부2 로 진행함]
   - Pricing: Pay-as-you-go (종량제를 선택)

3. Click Encryption Settings(암호설정으로 쓰여짐), and then in the Encryption Type list, click Do not enable encryption, and then click OK

   ![image](https://user-images.githubusercontent.com/46669509/54794178-75143580-4c89-11e9-8446-90c555b5250b.png)

4. On the New Data Lake Store blade, click Create

5. 배포가 진행되는 동안 in Azure Portal, Data Lake Storage [wcbdatalake]를 클릭

6. Data Lake Storage Overview blade, click Data Explorer

   ![image](https://user-images.githubusercontent.com/46669509/54794505-2cf61280-4c8b-11e9-8ba1-7af41f2c3369.png)

7. On the Data Explorer blade, and then click New Folder, 이름을 clusters로 type, and then click OK

   ![image](https://user-images.githubusercontent.com/46669509/54794524-4a2ae100-4c8b-11e9-8fce-21b67a50b4ba.png)

> Task 2 : Define basic configuration for a Linux-based Spark cluster

1. Azure Portal에서 새로만들기 하여 분석(Data+Analytics)에서 HDInsight를 click

   ![image](https://user-images.githubusercontent.com/46669509/54800826-4c029d80-4ca7-11e9-945c-62b480ea9ad9.png)

2. On the Basic blade 아래의 내용을 설정한다.
   - Cluster name: myhdinsight3585 (예시) [반드시 유니크해야함]
   - Subscription: Azure Portal의 활성화 된 구독을 선택

3. Under Cluster type, click Configure required settings.

4. On the Cluster configuration blade, 아래의 설정 내용을 적용한 후, click Select
   - Cluster type: Spark
   - Operation system: Linux (현재 Linux만 지원) 
   - Version: Spark (최신버젼) [베타버전은 피하자]
   - Cluster tier: STANDARD (설정 당시 없음)

5. On the Basic blade, 아래의 설정을 진행 및 click Next
   - Cluster login password: Pa55w.rdPa55w.rd (예시) [spark의 패스워드]
   - Resource group(Use existing): datalakerg [Data Lake Storage와 같은 Resource Group을 사용]
   - Lcation: 자신이 위치한 곳으로 설정 그러나 대한민국은 없다. [미국 동부2 로 진행함]

> Task 3 : Configure access between the Spark cluster and the Data Lake Storage account 

1. On the Storage blade, under Primary storage type, click Data Lake Storage

2. Click Select Data Lake Storage account

3. On the Select Data Lake Store blade, click wcbdatalake (Data Lake Storage)

   ![image](https://user-images.githubusercontent.com/46669509/54794266-e653e880-4c89-11e9-9106-51081be18193.png)

4. On the Storage blade, click Data Lake Store access

5. On the Data Lake Store access blade, click Create new, and the click Service principal.

6. On the Create service principal blade, type the following information, and then click Create:

   ![image](https://user-images.githubusercontent.com/46669509/54800838-591f8c80-4ca7-11e9-914e-31b9bcc5e2e7.png)

   -  Service principal name: wcbservice (예시) [반드시 유니크해야함]
   - Certificate password: Pa55w.rd
   - Confirm password: Pa55w.rd

7. On the Data Lake Storage access blade, click Access

   ![image](https://user-images.githubusercontent.com/46669509/54794559-79415280-4c8b-11e9-9963-308e2132a5dd.png)

8. On the Assign selected permissions blade, click Run(실행)

   ![image](https://user-images.githubusercontent.com/46669509/54794638-c45b6580-4c8b-11e9-8ce8-19b02eaa114f.png)

   ![image](https://user-images.githubusercontent.com/46669509/54794645-cc1b0a00-4c8b-11e9-8fda-c354636ae6dc.png)

9. Permission이 나는 것을 기다렸다가 아래의 Done(완료)를 click

   #### 에러메세지가 표시 될 경우

   아래와 같이 루트 경로를 clusters로 변경하자.

   ![image](https://user-images.githubusercontent.com/46669509/54794884-1ea8f600-4c8d-11e9-9b9c-6830b03f1e3b.png)

   에러이유는 clusters 아래의 사용자 폴더에 권한이 주어지지 않아 그런 것으로 추정된다..

   

10. On the Data Lake Storage access blade, click Select

11. On the Storage blade, click Next

> Task 4: Provision the cluster and verify cluster creation

1. On the Cluster summary blade, next to Cluster size, click Edit

2. On the Cluster size blade, in the Number of Worker nodes box (위에 위치), type 1

3. Click Worker node size.

4. On the Choose your node size blade, click View all (모두보기), click A3 General Purpose, and then Select

5. Click Head node size

6. On the Choose your node size blade, click View all, click A3 General Purpose, and the Select

7. On the Cluster size blade, click Next

8. On the Advanced setting , click Next (설정할 것 없음)

9. On the Cluster summary blade , 이상사항이 없으면, click Create

   ![image](https://user-images.githubusercontent.com/46669509/54800724-c121a300-4ca6-11e9-91e8-09e21ccd76d1.png)

10. 배포시간은 대략 20~30분이 소요 됨 (30분 걸림)

11. On the cluster blade, under PROPERTIES, click Storage accounts, Verify that the HDInsight cluster is using Data Lake Storage account

12. On the cluster blade, under PROPERTIES, click Data Lake Storage access. Verify that the service principal is correctly associated with the HDInsight cluster, and that the SERVICE PRINCIPAL status is Enabled.

     

- Exercise 2: Customize HDInsight by using script actions

> Task 1 : Create a storage account th use as additional storage

1. Click All resources and then click +Add, click Storage, and then click Storage account - blob, file, table, queue

2. On the Create Storage Account blade, 아래의 세팅을 한 후, click Create

   ![image](https://user-images.githubusercontent.com/46669509/54795327-10f47000-4c8f-11e9-88c4-434122664a95.png)

   - Name: mywcbstorage (예시) [반드시 유니크해야함]
   - Replication: Locally-redundant storage(LRS)
   - Subscription: 구독정보
   - Resource Group (Create New): datalake
   - Location: 자신의 위치 (storage는 대한민국 중부로 셋팅함)

3. 배포가 끝난 후 Resource Group의 mywcbstorage  를 click

4. On the Storage blade, click Access Key (설정 목록 중 중간 하부에 위치함)

   ![image](https://user-images.githubusercontent.com/46669509/54795395-6466be00-4c8f-11e9-8db6-bb086729f1bc.png)

5. On the key1 row, to the right of the key column, click Click to copy (문서모양의 아이콘이 있음)

6. 복사한 key를 메모장에 옮겨 놓는다. [필수적임]

> Task 2 : Connect additional storage to a running cluster by using a script action

1. Azure Portal의 All Resources 에 가서 myhdinsight3585 를 클릭한다. 

2. HDInsight cluster blade에서 Script actions 를 클릭한다. 

3. Script actions blade 에서 + Submit new를 클릭

   ![image](https://user-images.githubusercontent.com/46669509/54795606-43529d00-4c90-11e9-96ad-02d3e017a08e.png)

4. Submit script action blade에서 아래의 설정을 진행 한 후 click Create
   - Select premade script: Add an Azure Storage account
   - Head: selected 
   - Worker: selected
   - Parameters: myhdinsight3585 -- Space bar-- access key 의 값을 넣어준다.

5. SCRIPT ACTION HISTORY 리스트에 running인 상태로 나타나면 OK

6. In the Azure Portal, click All resources, and then click wcbmagic3585

   ![image](https://user-images.githubusercontent.com/46669509/54795658-76952c00-4c90-11e9-9243-66e633a9bfc9.png)

7. HDInsight cluster blade에서 Cluster dashboard를 클릭한다.

8. Cluster dashboard 에서 HDInsight cluster dashboard를 클릭한다. 

   ![image](https://user-images.githubusercontent.com/46669509/54795682-89a7fc00-4c90-11e9-9f8f-3d6caa654309.png)

9. On the Ambari home page, click HDFS, and then click Configs 

10. Login 창이 뜨면 ID: admin / Password: Pa55w.rdPa55w.rd 를 통해 Login한다. 

    ![image](https://user-images.githubusercontent.com/46669509/54795682-89a7fc00-4c90-11e9-9f8f-3d6caa654309.png)

    

    ![image](https://user-images.githubusercontent.com/46669509/54795717-b1975f80-4c90-11e9-88cb-434cf8dd0394.png)

11. In the Filter box, type the name of your additional storage account (mywcbstorage)

    ![image](https://user-images.githubusercontent.com/46669509/54795826-f58a6480-4c90-11e9-862e-f3613f6968f4.png)

    ![image](https://user-images.githubusercontent.com/46669509/54800738-d0a0ec00-4ca6-11e9-88ee-a5e0d7106cb7.png)

    

12. 화면에 storage account가 존재하는지 확인한다. 

- Exercise 3: Delete an HDInsight cluster

1. 지우면된다.

   

   ## Why desire these process on Spark?

> 왜 Spark는 Storage를 연결하여 사용하는 과정이 필요한가? 
>
> 스파크(Spark)는 인메모리 기반의 범용 데이터 처리 플랫폼이다. 배치 처리, 머신러닝, SQL 질의 처리, 스트리밍 데이터 처리, 그래프 라이브러리 처리와 같은 다양한 작업을 수용할 수 있도록 설계되어 있다. 따라서 추가적으로 Storage를 구성하여 연결을 진행해주어야 하는 과정이 필요하다. 