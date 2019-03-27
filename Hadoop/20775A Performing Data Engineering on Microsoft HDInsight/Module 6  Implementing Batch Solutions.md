# Module 6 : Implementing Batch Solutions

<https://cwiki.apache.org/confluence/display/Hive/LanguageManual>

### Module Overview

- Apache Hive storage
- HDInsight data queries using Hive and Pig
- Operationalize HDInsight

![image](https://user-images.githubusercontent.com/46669551/54962833-b8ccaf00-4fa9-11e9-83af-1c3174e16500.png)

> Hive 내부의 처리 순서
>
> 1. Parser : 구문을 분석
>
> 2. Planner : Parser 진행 후 작업의 수행계획 
>
> 3. Execution : 수행
>
> 4. Optimizer : 수행 시 Path를 지정해주어 수행작업을 최적화한다.
>
> 특징 : 
>
> 1. Hive는 Table형식의 파일로 만들어 준다음(정형화) 작업을 진행해야 한다. 
> 2. Batch 를 사용하기 위해서는 Map Reduce를 벗어날 수 없다.
> 3. Map Reduce를 하지 못하는 부분을 데이터를 정형화 하여 진행 할 수 있도록 해준다.

### Lesson 1: Apache Hive storage

- What is Hive?
- Internal and external Hive tables
- Load data into a Hive table
- Improve Hive performance by using partitioning and bucketing
- Load data from semi-structured files, such as JSON, into Hive
- Storage formats (text, sequence files, Apache Parquet, ORC, and others)
- Demonstration: Using templates in Azure

> 데이터가 100개가 있다고 가정하자 

> Hive를 통해 데이터를 Partition 을 나누어 구간데이터의 형태로 지정하여 내가 원하는 데이터를 빠르게 처리하거나 호출하는 것이 가능하다.
>
> 나눈 데이터를 Bucket이라고 생각하면되겠다.
>
> 비정형과 정형의 중간인 JSON과 같은 semi-structure의 형태를 띈다.
>
> 내부적으로 사용되는 File의 Format이 존재한다. (전송 및 접근의 효용성이 높아진다) [Parquet, Avro - 전송format]

> *RDBMS 의 종류

> OLTP(트렌젝션 위주의 DB) - ‘Online transaction processing’의 줄임말로 온라인상에서 처리 및 거래되는 데이터베이스 시스템을 말합니다

> OLAP(넌 트렌젝션 위주의 DB) -  ‘Online analytical processing’의 줄임말로 온라인상에서 분석이 가능한 시스템입니다 (계속 누적이 되는 Data) [Hive]

![image](https://user-images.githubusercontent.com/46669551/54963345-d1d65f80-4fab-11e9-81ba-30e753fcf529.png)

### What is Hive?

- Apache Hive project:
  - Data warehouse system for Hadoop
  - Impose a structure to data in HDFS
  - Summarize, aggregate, query data using HiveQL (SQL-based language)
  - Extend query language using **user-defined functions (UDFs)** written in multiple languages

### Internal and external Hive tables

- Hive tables: internal(임시파일의 용도) vs. external(사용자 정의)
- Internal tables:
  - Only for temporary use
  - When a table is deleted, **both data and metadata is deleted**
  - **Fully managed by the Hive**
- Example of Hive

```hive
CREATE TABLE tweet_msg
(
	name STRING,
	language STRING,
	text STRING,
	created_at STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '|' 
```



- External tables:
  - Data is at a custom data location (within HDFS)
  - Data is used, created or processed by outside processes
  - When a table is deleted, only metadata is removed from Hive (**underlying data remains unchanged)**

```hive
CREATE EXTERNAL TABLE tweet_rawdata
(
	json STRING
)
ROW FORMAT DELINITED
FILED TERMINATED BY '|'
STORED AS TEXTFILE
LOCATION '/twitter/log'
```



### Load data into a Hive table

- Loading data into a Hive table:

  - LOAD DATA operation
    - Import data into a table by copying or moving data into a table directory of the Hive data warehouse
    - Simple copy/move filesystem operation
    - SQL query will access raw data during query execution

  - INSERT statement:
    - Populates data table based on an execution of a Hive query

  - CREATE TABLE … AS SELECT construct:
    - Creates and populates a data table based on an execution of a Hive query

### Load text file data into a Hive table

```
LOAD DATA LOCAL INPATH '/input/tweetmsg_sample.txt' INTO TABLE tweet_msg
```

### Inserting data into Hive tables from queries

```
INSERT OVERWRITE TABLE results
SELECT column1, column2, column3
FROM source;
```

### Creating

```
CREATE TABLE results
AS SELECT column1,column2,column3
FROM source;
```

### Azure Portal 을 이용하여 

### Hadoop cluster을 만들고 Ambri 를 접속

![image](https://user-images.githubusercontent.com/46669551/54965205-e0744500-4fb2-11e9-9a16-f54ef3e7e03c.png)

### Hive를 사용하여  Table을 생성  

![image](https://user-images.githubusercontent.com/46669551/54965239-13b6d400-4fb3-11e9-8f8a-d67f6627a433.png)

![image](https://user-images.githubusercontent.com/46669551/54965222-fd107d00-4fb2-11e9-85d6-2471de9bc2cc.png)

### DDL을 가져온 내용 

```
CREATE TABLE `employee`(
  `name` string, 
  `salary` string, 
  `id` tinyint, 
  `sex` string, 
  `department` string)
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.ql.io.orc.OrcSerde' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.orc.OrcInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.orc.OrcOutputFormat'
LOCATION
  'wasb://kdongxiecn-ctr@kdongxiesa.blob.core.windows.net/hive/warehouse/employee'
TBLPROPERTIES (
  'COLUMN_STATS_ACCURATE'='{\"BASIC_STATS\":\"true\"}', 
  'numFiles'='0', 
  'numRows'='0', 
  'rawDataSize'='0', 
  'totalSize'='0', 
  'transient_lastDdlTime'='1553563409')
```



### Improve Hive performance by using partitioning and bucketing

- Partitioning and bucketing (성능 향상을 위한 방법)

  - Both are used to improve query performance

  - Both subdivide data tables

- Partitioning (File을 나누어 주는 것)

  - PARTITION BY clause

  - Specify partition columns

  - At filesystem level, data results in subdirectories

- Bucketing 

  - CLUSTERED BY clause

  - Advantage for map-side joins

  - Support for efficient dataset sample

### Load data from semi-structured files, such as JSON, into Hive
- Load data from semi-structured files (such as JSON):

  - Hive supports SerDe to deal with custom file formats

    - Serializer-Deserializer (SerDe) interface

    - A number of built-in ones, including JSON SerDe
  - Specify SerDe when table is created

    - CREATE TABLE clause

    - ROW FORMAT SERDE statement to specify a fully qualified name of the Java class implementing SerDe
  - Hive will load data from file, then write back to HDFS using SerDe

### Load data from a JSON file containing log data into a Hive table

```
CREATE TABLE log_data(
	ts BIGINT,
	line STRING
) ROW FROMAT SERDE 'apache.hive.hcatalog.data.JsonSerDe' 
STORED AS TEXTFILE;
```



### Storage formats (text, sequence files, Apache Parquet, ORC, and others)
- Storage formats in Hive:

  - Dictate how row data is stored in a Hive table

  - Delimited text (default)

  - Binary storage formats:

    - Row-oriented:
      - Avro [하나의 format - type]

    - Column-oriented:
      - Parquet, ORC

## Demonstration: Using templates in Azure

In this demonstration, you will see how to:

- Create a Linux-based HDInsight cluster and a SQL Database using a JSON template 

> Delete the HDInsight cluster, database, and resource group after the module is complete.
>
> **Question**
>
> Describe the main differences between internal and external tables in Hive. In which circumstances would you choose one over another?
>
> **Answer**
>
> Internal tables that store data within the Hive data warehouse should be considered for temporary use only. External tables separate the storage of metadata and table data. Metadata is managed by Hive while table data is stored at an external location. External tables are often used when an external process manages an underlying table’s data, or when table data needs to be stored at a custom location.\



### Join tables with Hive

- Table joins in Hive:
  - Converted to map/reduce jobs
  - Only equality conditions are supported

- Main types of joins:
  - Common or shuffle join
  - Map or map-side or <u>broadcast join</u> (Full Outer JOIN)
  - Bucket map join

- Hive optimizer tries to improve efficiency of joins

### Hive UDFs with Java and Python

<https://docs.microsoft.com/ko-kr/azure/hdinsight/hadoop/python-udf-hdinsight>

- User-defined functions (UDFs) in Hive
  - Implement custom code in your language of choice

- Use an ADD statement to add a script or jar package to the distributed cache

- Use a CREATE TEMPORARY FUNCTION for Java-based UDFs

- Use your UDF in the Hive queries

SELECT statement with Python UDF

```
add file wasbs://sampleudf.py
SELECT TRANSFORM (id, devicetype, devicemodel)
USING 'python sampleudf.py' AS (id, deviceTypeName, deviceModelName)
FROM sampletable
ORDER BY id LIMIT 100;
```



### Design scripts with Pig

<https://docs.microsoft.com/ko-kr/azure/hdinsight/hadoop/apache-hadoop-use-pig-ssh>

- Apache Pig

  - Abstraction on the top of a MapReduce execution engine

  - Support for ETL processes:

    - Load data to be manipulated from the file system

    - Transform the data (filter, group, and so on)

    - Output data or store it for processing

  - Development using Pig **Latin programming language** (함수형)

  - Support for UDFs

  - Pig programs are easier to develop compared to MapReduce jobs

### An example of pig Latin script

```pig
LOGS = LOAD '/example/data/sample.log';
LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
RESULT = order FREQUENCIES by COUNT desc;
DUMP RESULT;
```



### Run Pig programs in HDInsight

- Use Pig with Hadoop on HDInsight:

  - Interactive console (Grunt)

  - Pig script to support batch processing

- Grunt

  - Run Pig commands interactively

  - Run Pig programs as a script

- Azure PowerShell
  - Orchestrate Pig job execution

## Lesson 3: Operationalize HDInsight

<https://docs.microsoft.com/ko-kr/azure/data-factory/introduction>

- What is Data Factory?
- Using Data Factory with HDInsight
- Using Apache Oozie with HDInsight
- Apache Oozie compared to Data Factory

![image](https://user-images.githubusercontent.com/46669551/54966329-4367db00-4fb7-11e9-93db-4ed913358e41.png)

### What is Data Factory?

- **Data Factory** (데이터처리의 구성 및 과정을 쉽게 만들어 주는 프로그램) (Work Flow) -- 이거 개편함

  - Cloud service to orchestrate and automate data movement and transformation

  - Can connect to on-premises and cloud data sources

- Support three main capabilities:

  - Collect data:
    - From on-premises, or cloud-based sources

  - Process and transform data:

    - Requires provisioning of compute environment

    - Using HDInsight, Machine Learning, Azure Data Lake Analytics, Azure SQL DB, Azure SQL DW

  - Publish data for further consumption
    - Back to on-premises or cloud-based destinations

## 작동 원리

### Azure Data Factory의 파이프라인(데이터 기반 워크플로)는 일반적으로 다음 네 단계를 수행합니다.

![image](https://user-images.githubusercontent.com/46669551/54966370-6b573e80-4fb7-11e9-8b6d-f9df4f6695a3.png)

![image](https://user-images.githubusercontent.com/46669551/54973021-8767d980-4fd1-11e9-9eb3-37e0ebf36d7f.png)

![image](https://user-images.githubusercontent.com/46669551/54973137-f3e2d880-4fd1-11e9-8e77-adfb36ddba9a.png)

### Using Data Factory with HDInsight

- Data Factory data transformation activity:

  - Requires a compute environment—Azure HDInsight cluster or Azure Batch

  - HDInsight cluster can be:

    - Existing HDInsight cluster

    - On-demand HDInsight cluster created specifically for this activity (and deleted processing is finished)

  - Main HDInsight-related activities:

    - Hive activity

    - Pig activity

    - MapReduce activity

    - Spark activity

### Using Apache Oozie (work flow) with HDInsight

- Apache Oozie:

  - A scalable, distributed **workflow** to run dependent jobs

  - Integrates with **Hadoop ecosystem**, such as MapReduce, Hive, Pig, Sqoop, and so on

- Three main components:

  - Workflow engine

  - Coordinator engine

  - Workflow definition

### Apache Oozie compared to Data Factory [Apache인것이냐?(Hadoop의 종속적 기능)  Azure냐?(Cloud Service)]
- **Apache Oozie vs. Data Factory**

- Similarities:

  - Orchestrate data movement and data processing jobs for big data workloads

  - Integrated with Hadoop ecosystem

  - Pipelines can be triggered based on a schedule or data availability

- Differences:

  - Data Factory is a cloud-based managed service

  - Data Factory can only use an HDInsight cluster as a compute environment 

  - Oozie is shipped with most of Hadoop distributions, while Data Factory requires an HDInsight cluster

## Lab: Implement batch solutions

- Exercise 1: Deploy HDInsight cluster and data storage
- Exercise 2: Use data transfers with HDInsight clusters
- Exercise 3: Query HDInsight cluster data

> Lab Scenario
>
> The Contoso IT department has a large on-premises IT infrastructure, and are planning to move much of this infrastructure into the cloud. The Contoso IT department is keen to understand the big data management capabilities in Azure.
> Contoso have approached your consulting organization to demonstrate, through a Proof of Concept (PoC), how to implement batch solutions using Azure, Apache, Hive, Pig, and SQL Database. 

> Lab Review
>
> - Which control interface would you use to control Pigs within your own organization?
> - What typical data queries in HDInsight would you implement within your organization?

### PIG 사용

### 1. PUTTY 를 이용하여 Cluster에 접속 

### 2. Password를 입력한 후 `pig`를 입력 

### 3. 이후 아래의 Command를 입력하여 sample data를 가공처리 한다. 

```
LOGS = LOAD 'wasbs:///example/data/sample.log';
LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
RESULT = order FREQUENCIES by COUNT desc;
DUMP RESULT;
```

### 4. 출력 값 확인

```
(TRACE,816)
(DEBUG,434)
(INFO,96)
(WARN,11)
(ERROR,6)
(FATAL,2)
```

### Query data using HiveQL from the Ambari dashboard

### 1. Ambari에 접속하여 Hive View를 클릭한 후 Query Editer에 아래의 Command를 입력한다.

```
set hive.execution.engine=tez;
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
```



### 2. 몇분 후 t4에 가진 [ERROR]의 값이 3개라는 문구가 출력 되게 된다.

```
sev	count
[ERROR]	3
```

![1553580459009](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553580459009.png)



## 전체의 Lab이 끝났으니 모든 Resource Group을 삭제 한다. 









Load text file data into a Hive table 

```
LOAD DATA LOCAL INPATH
'/input/tweetmsg_sample.txt'INTO TABLE tweet_msg
```

``` 
데이터를 만들어 Hive로 조작하기 
mysqlserver12344321.database.windows.net

id : admin2019
pwd : Pa$$w0rd2019

Hive & Pig
/usr/bin/hive
> show tables
> show databases;
> use database명;
> drop database database명;
> create table employee(eid int,
name String, salary String,
dest String);

eid	int
name String
salary String
dest String

1	Tom	2000	home
2	John	3000	home
3	Amy	4000	compnay
/home/sshuser/employee.txt
/usr/bin/hive

create table employee3(eid int, name String, salary String, dest String) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';

LOAD DATA LOCAL INPATH '/home/sshuser/employee.txt' OVERWRITE INTO TABLE employee3;
```

