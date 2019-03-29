# Module 7 : Design Batch ETL Solutions for Big Data with Spark
### 들어가기 전에...

There are demonstrations and labs in this course that require access to Microsoft® Azure®. You need to allow sufficient time for the setup and configuration of a Microsoft Azure pass that will provide access for you and your students.

For details of how to acquire Microsoft Azure passes for your class, see:

Access to Microsoft Learning Azure Passes for Students of Authorized Microsoft Learning Partners

<https://aka.ms/x0n953>

Before starting the module, please check the following links:

- How to get an Azure Free trial for testing Hadoop in HDInsight®:

  <https://azure.microsoft.com/en-us/resources/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/>.

- How to create Apache Spark clusters in Microsoft Azure HDInsight:

  [https](https://aka.ms/AA3480f)[://aka.ms/AA3480f](https://aka.ms/AA3480f)

Review the latest updates and documentation:

- HDInsight Documentation:

  <https://docs.microsoft.com/en-us/azure/hdinsight/>.

- Apache Spark Documentation:

  <http://spark.apache.org/>.

- Hortonworks Documentation (HDP):

  <http://docs.hortonworks.com/>.

Several demonstrations in this module require access to a Spark cluster in HDInsight. You should create the Spark cluster in HDInsight before you start the module, as described by the article *How to create Apache Spark clusters in Azure HDInsight*. 

Before starting this module, you should perform the following steps. This will take 30-45 minutes to complete:

- Start the machine learning notebook: **Predictive analysis on food inspection data using MLLib**. For instructions on how to run this notebook, see: [https](https://github.com/Microsoft/azure-docs/blob/master/articles/hdinsight/hdinsight-apache-spark-machine-learning-mllib-ipython.md)[://](https://github.com/Microsoft/azure-docs/blob/master/articles/hdinsight/hdinsight-apache-spark-machine-learning-mllib-ipython.md)aka.ms/AA3480l 

## ETL

![image](https://user-images.githubusercontent.com/46669551/55041580-02340180-5071-11e9-96b7-6386a08d0ac6.png)

# [HDP 구성요소](https://ko.hortonworks.com/products/data-platforms/hdp/)

![image](https://user-images.githubusercontent.com/46669551/55041720-b2096f00-5071-11e9-90f1-45e0f46bd758.png)

![image](https://user-images.githubusercontent.com/46669551/55041728-c51c3f00-5071-11e9-8e78-25b75a047197.png)

## Module Overview

- What is Spark?
- Extract, Transform, Load (ELT) with Spark
- Spark performance

### Prerequisites

- HDInsight cluster on Azure
- PuTTY, or other SSH client

## Lesson 1: What is Spark?

- Understanding Apache Spark on HDInsight
- Spark components and supporting applications
- Common execution model
- Spark SQL
- IntelliJ IDEA
- Jupyter Notebooks
- Power BI

### Understanding Apache Spark on HDInsight
![image](https://user-images.githubusercontent.com/46669551/54976590-314d6300-4fde-11e9-802e-266c33414daa.png)

## Spark components and supporting applications 

![image](https://user-images.githubusercontent.com/46669551/55042736-4544a380-5076-11e9-97b4-33612bdee97e.png)

- **Spark Core** [Scala & Python]
  - Spark SQL
  - Spark Streaming (건건당 들어온 데이터를 처리가능)
  - GraphX
  - MLlib

- **Anaconda**
- **Livy**
- **Jupyter Notebook**

### Common execution model

- Driver : 클러스터 관리자에 연결하여 리소스를 응용 프로그램에 걸쳐 리소스 할당. 클러스터 실행자를 얻고 계산 작업을 처리 데이터를 캐시. 실행자에 앱코드를 전송. 실행자를 위한 작업을 전송하여 실행.

- Executor

- Task

- Cluster manager:

  - Spark (Standalone. A cluster manager included with Spark)

  - Apache Mesos (A general-purpose cluster manager for Hadoop)

  - Hadoop YARN (The Hadoop 2 resource manager)

    

### Spark SQL

- Mix SQL statements with Spark programs
- Connect to and query any data source
- Execute Hive queries on existing data
- Connect through JDBC or ODBC

### IntelliJ IDEA

- Commonly used for Scala/Spark application development
- Open-source and paid-for editions available

### Jupyter Notebooks

- General-purpose data querying and analysis tool
- Installed on HDInsight clusters with support for Spark queries written in: 
  - Scala
  - Python version 2
  - Python version 3

- Access through Azure Portal, direct URL

### Power BI

- BI tools

  - Interactive visualization

  - Self-service BI reporting

- Support for HDInsight on Azure Spark clusters as a data source

### Lesson 2: Extract, Transform, Load (ELT) with Spark
- Advantages of Spark for ETL
- Extract: read data with SparkContext
- Transform: the RDD API
- RDD transformations
- RDD actions
- RDD example: word count ETL
- Load: Spark SQL
- Submitting Spark applications
- Connecting Spark to external data sources
- Demonstration: Spark ETL

### Advantages of Spark for ETL

- Supports multiple file formats such as Parquet, Avro, Text, JSON, XML, ORC, and so on

- Supports data stored in Azure Storage, HDFS, Apache HBase, Cassandra, S3, RDBMSs, NoSQL

- Enables ETL coding using Java, Scala, or Python

- Provides in-memory computing for fast data processing

- Includes APIs to facilitate the creation of schemas for your data and perform SQL computations

- Supports distributed computing and fault tolerance is built in to the framework

### Extract: read data with SparkContext (기능[sc.textFile()의 형태])
- Use SparkContext for “extract” phase

- Any Hadoop data source supported

- Many data formats supported:

  - Text

  - Sequences

  - Serialized Java objects

  - Hadoop InputFormat

## [RDD](https://12bme.tistory.com/306)  (Resilient Distributed Dataset)

Spark = RDD + Interface

> Action code가 입력되기 전까지 실행되지 않는 특징을 가지고 있다. 

![image](https://user-images.githubusercontent.com/46669551/55044362-5e9d1e00-507d-11e9-9738-4c0d8b4e8efb.png)

> **RDD의 내부 작동 원리**
>
> ------
>
> RDD는 병렬도 동작하고, 이는 스파크의 가장 큰 장점입니다. 각 트랜스포메이션은 속도를 비약적으로 향상시키기 위해 실행됩니다.
>
>  데이터셋에 대한 트랜스포케이션은 게으릅니다. 이 말은 모든 트랜스포메이션은 데이터셋에 대한 액션이 호출됐을 때 실행된다는 뜻입니다. 이로 인해 스파크의 실행이 최적화됩니다. 예를 들어, 데이터 분석가가 데이터셋에 익숙해지기 위해 수행하는 다음 일반적인 과정을 보겠습니다.
>
>   \1. 특정 칼럼의 고유한 값의 개수를 세기
>
>   \2. A로 시작하는 것들 찾기
>
>   \3. 스크린에 결과 출력하기
>
> 위의 언급된 과정과 같이 간단하게, A로 시작하는 단어들이 궁금하다면, 다른 아이템의 고유 값 개수를 세는 것은 의미없는 작업입니다. 그러므로 스파크는 위의 실행 과정을 순서대로 따르지 않으며, A로 시작하는 단어를 세고 결과를 출력하는 작업만 수행합니다.
>
> 이 과정을 코드로 나눠보겠습니다. 
>
> 첫번째로는 .map(lambda v: (v, 1))를 이용해 스파크가 A를 포함하는 단어를 모으게 하고, 
>
> 두번째로 .filter(lambda val: val.startswith('A')) method) 를 사용해 A로 시작하는 단어를 필터링합니다. 
>
> 세번째로 .reduceByKey(operator.add)를 호출하면, 각 키마다 출현한 횟수를 모두 더해 데이터셋이 줄어들게 됩니다. 이 모든 단계들이 데이터셋을 트랜스폼(transform)합니다.
>
> 네번째로는 .collect() 함수를 호출해 단계를 수행합니다. 이 단계가 우리의 데이터셋에 대한 액션입니다. (이 과정으로 데이터셋에서의 고유 데이터 갯수를 세게 됩니다.)
>
>  결과적으로 액션은 트랜스포케이션의 순서를 역순으로 해서, 매핑을 먼저하고 데이터를 필터링합니다. 이로 인해 더 적은 데이터가 리듀서(Reducer)에 전달 됩니다.

![image](https://user-images.githubusercontent.com/46669551/55044498-e7b45500-507d-11e9-89b5-1d6d6149e07d.png)

### Transform: the RDD API

- An immutable collection of objects 
- Partitioned and distributed across multiple physical nodes of a YARN cluster 
- Can be operated on in parallel (RDD는 두가지의 기능을 갖는다)
  - Transformations (변환)
  - Actions (수행)

### RDD transformations (Data를 처리하지 않은 상태)

- Apply a transformation to generate an output RDD from an input RDD
- Transformations are lazily evaluated

### RDD actions (Data처리를 수행)

- Return values to a calling application
- **Triggers evaluation** of transformations on which the action depends

### RDD example: word count ETL

- Python word count: [함수가 함수를 받는다는 특징을 갖는다]

  ![image](https://user-images.githubusercontent.com/46669551/54982907-19caa600-4fef-11e9-8541-7bb6e6ae4a12.png)

- Scala word count:

  ![image](https://user-images.githubusercontent.com/46669551/54982924-23eca480-4fef-11e9-915d-48466e68ddbe.png)

### Load: Spark SQL

- Use Spark SQL to persist RDDs to Hive
  - RDD > DataFrame > Hive

- Most RDDs cannot convert directly to a DataFrame
  - Use an interim RDD of type:
    - Case class
    - JavaBean
    - Row object

- Persist DataFrame to Hive as a table or view

### Submitting Spark applications

Many ways to trigger Spark applications:

- Use spark-submit 
- Execute a compiled jar file or Python script
  - Application file must be available to all nodes in the Spark cluster

### Connecting Spark to external data sources
Data sources external to HDInsight on Spark clusters:

- NoSQL—connectors
  - **HBase**
  - **Azure DocumentDB**
  - **MongoDB**
- Azure Data Lake—adl:// URI
- Azure SQL Data Warehouse—ODBC/JDBC



### 1. Jupyter NoteBook을 통하여 Storage account에 접속하여 데이터를 호출하기

```
1. Storageaccount의 container의 속성에서 root를 확인한다

https://kdongxie2019sa.blob.core.windows.net/kdongxie2019cn-2019-03-27t03-14-45-795z

2. 호출한는 systex에 맞추어 준다

baby_names = sqlContext.read.format("com.databricks.spark.csv").options(header='true', inferschema='true').load('wasb://컨테이너이름@스토리지이름.blob.core.windows.net/user/sshuser/baby_names.csv')

3. Jupyter notebook에 명령어를 실행시켜 Spark application을 실행시킨다.

baby_names = sqlContext.read.format("com.databricks.spark.csv").options(header='true', inferschema='true').load('wasb://kdongxie2019cn-2019-03-27t03-14-45-795z@kdongxie2019sa.blob.core.windows.net/user/sshuser/baby_names.csv')
```

### 출력 값

```
Starting Spark application
ID	YARN Application ID	Kind	State	Spark UI	Driver log	Current session?
0	application_1553657040065_0005	pyspark	idle	Link	Link	✔
SparkSession available as 'spark'.
```

### 2. 호출한 데이터를 변수에 저장한 후 출력하기

```
baby_names.registerTempTable("baby_names")
result = sqlContext.sql("select * from baby_names")
result.show()
```

### 출력 값

```
+----+-----------+-----------+---+-----+
|Year| First Name|     County|Sex|Count|
+----+-----------+-----------+---+-----+
|2013|      GAVIN|ST LAWRENCE|  M|    9|
|2013|       LEVI|ST LAWRENCE|  M|    9|
|2013|      LOGAN|   NEW YORK|  M|   44|
|2013|     HUDSON|   NEW YORK|  M|   49|
|2013|    GABRIEL|   NEW YORK|  M|   50|
|2013|   THEODORE|   NEW YORK|  M|   51|
|2013|      ELIZA|      KINGS|  F|   16|
|2013|  MADELEINE|      KINGS|  F|   16|
|2013|       ZARA|      KINGS|  F|   16|
|2013|      DAISY|      KINGS|  F|   16|
|2013|   JONATHAN|   NEW YORK|  M|   51|
|2013|CHRISTOPHER|   NEW YORK|  M|   52|
|2013|       LUKE|    SUFFOLK|  M|   49|
|2013|    JACKSON|   NEW YORK|  M|   53|
|2013|    JACKSON|    SUFFOLK|  M|   49|
|2013|     JOSHUA|   NEW YORK|  M|   53|
|2013|      AIDEN|   NEW YORK|  M|   53|
|2013|    BRANDON|    SUFFOLK|  M|   50|
|2013|       JUDY|      KINGS|  F|   16|
|2013|      MASON|ST LAWRENCE|  M|    8|
+----+-----------+-----------+---+-----+
only showing top 20 rows
```

### 3. 출력값의 갯 수를 출력해준다.

```
sqlContext.sql("select count(*) from baby_names").show()
```

### 출력 값

```
+--------+
|count(1)|
+--------+
|  145570|
+--------+
```

### 4. Groupby를 통하여 Distict한다

```
sqlSmt = "select County, count(*) from baby_names group by County"
```

### Groupby한 내용을 출력한다.

```
sqlContext.sql(sqlSmt).show()
```

### 출력 값

```
+-----------+--------+
|     County|count(1)|
+-----------+--------+
|     Cayuga|     537|
|     Ulster|    1002|
|     FULTON|      58|
|      Kings|   12888|
|     Monroe|    3819|
|     Queens|   10365|
|   Franklin|     368|
|     Oneida|    1532|
|    Chemung|     702|
|CATTARAUGUS|     170|
|     Orange|    2160|
|    STEUBEN|     231|
|      YATES|       3|
|    Steuben|     759|
|     Otsego|     402|
| Montgomery|     505|
|   Tompkins|     607|
|      KINGS|    4957|
|   Schuyler|     159|
|     OSWEGO|     371|
+-----------+--------+
only showing top 20 rows
```



## Demonstration: Spark ETL

In this demonstration, you will see:

- How to run a simple Spark ETL process from Jupyter Notebook

### Jupyter Notebook을 활용 PySpark를 이용한 데이터 처리

1. Text file을 호출하여 RDD의 타입으로 변환하여준다.

```
from pyspark.sql import Row
text_file = sc.textFile("wasbs:///HdiSamples/HdiSamples/TwitterTrendsSampleData/tweets.txt")
```

```
counts = text_file.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a+b)
```

2. 변환한 파일의 앞의 5개만을 호출하여 출력한다.

```
counts.take(5) #for visualization of the interim step only
```

`출력 값`

```
[(u'', 59), (u'Texas","url":"http:\\/\\/www.ibm.com\\/socialbusiness","description":"Marketing', 1), (u'Lead', 3), (u'#Gardening', 2), (u'sleep', 1)]
```

3. 변환된 파일의 전체 갯수를 count한다.

```
counts.count() #for visualization of the interim step only

`3817`
```

4. RDD를 DataFrame의 형식으로 바꾸어 주고 DataFrame Type의 Table로 저장해준다.

```
counts_row = counts.map(lambda p: Row(word=p[0],count=int(p[1])))
schemaCounts = sqlContext.createDataFrame(counts_row)
schemaCounts.registerTempTable("counts")
```

5. 가장많이 나온 word와 갯수를 위에서 5개만을 출력해준다

```
output = sqlContext.sql("select * from counts order by count desc limit 5")
for eachrow in output.collect():
    print(eachrow)
```

`출력 값`

```
Row(count=506, word=u'+0000')
Row(count=346, word=u'Nov')
Row(count=267, word=u'25')
Row(count=200, word=u'{"created_at":"Tue')
Row(count=131, word=u'for')
```

### Putty를 통한 개발

1. spark cluster에 접속하여 .py파일을 생성

```
touch wordcount.py
```

2. 생성된 .py 파일안에 스크립트를 넣어준다.

```
import sys

from pyspark import SparkContext, SparkConf

if __name__ == "__main__":

    conf = SparkConf().setAppName("Word Count - Python").set("spark.hadoop.yarn.resourcemanager.address", "https://myhdinsight123411.azurehdinsight.net:8032")
    sc = SparkContext(conf=conf)
    words = sc.textFile("wasb:///user/sshuser/ab40thv.txt").flatMap(lambda line: line.split(" "))
    wordCounts = words.map(lambda word: (word, 1)).reduceByKey(lambda a,b:a +b)
    wordCounts.saveAsTextFile("wasb:///user/sshuser/output/")
```

3. spark를 사용하여 쿼리문을 실행시킨다.

```
spark-submit wordcount.py
```

4. 파일 생성을 확인한다.

![image](https://user-images.githubusercontent.com/46669551/55051059-f824f980-5096-11e9-8b1a-a5589d482073.png)

## Lesson 3: Spark performance

- YARN queues
- Debugging Spark jobs
- Spark driver and executor settings
- Partitioning
- Spark SQL query graphs
- Sharing metastore and storage accounts between Hive and
- Spark
- Demonstration: Tracking and debugging jobs running on Spark in HDInsight

### YARN queues

- **YARN Capacity Scheduler** <u>manages resources and queues</u>
- A queue is allocated a percentage of total cluster resources
- Manage YARN queues <u>through **Ambari**</u>

### Debugging Spark jobs

- Yarn UI

  - Details of Yarn resource allocation
  - Opens Spark UI from the **Tracking URL** link


```
https://CLUSTERNAME.azurehdinsight.net/yarnui
```

- Spark UI
  - View details of running jobs

- Spark History Server
  - Details of completed jobs

### Spark driver and executor settings

- Executor settings:
  - Number of executors: **spark.executor.instances**
  - Number of cores: **spark.executor.cores**
  - Executor memory: **spark.executor.memory**

- Cluster-level defaults

- Job-level override

- Driver settings

  - Minimum executors

  - Maximum executors 

  - Executor memory

## Partitioning

- Divides RDDs into blocks for more efficient parallelization

- Automatically managed by Spark

- Control partitioning manually with
  - PartitionBy action
  - Repartition action

### Spark SQL query graphs

- Spark SQL query graphs (or query plans) generated by the Catalyst Optimizer
  - Convert SQL statements into a series of logical steps

- View plans for running applications on the **SQL** pane of Spark UI

  - The **Duration** property indicates the performance impact of the query overall 

  - The **data size total** property of individual steps indicates the likely performance impact

### Sharing metastore and storage accounts between Hive and Spark
- Metastore

  - Metadata in Azure SQL Database

  - Share between cluster types to improve performance

- Storage Account

  - To maximize performance, do not share

  - Assigning each cluster one (or more) storage accounts maximizes I/O performance

## Demonstration: Tracking and debugging jobs running on Spark in HDInsight
In this demonstration, you will learn how to track and debug Spark jobs using: 

- YARN UI
- Spark UI
- Spark History Server

### Lab: Working with Spark ETL

- Exercise 1: Design a Spark ETL application

### Lab Scenario

> You work as a consultant for Contoso, a large IT consultancy organization with staff in different industrial sectors. You are asked to create a Spark ETL application to load data from one of Contoso’s clients, a movie streaming service provider. The data contains movie rating information from users of the streaming service; in future, Contoso will use the data as one of several inputs to a machine learning model for making movie recommendations.

### Lab Scenario (Continued)

> In this lab, you will use Jupyter Notebook and a sample data file to prototype the ETL process (using Python 2). You will then create a Python script containing your application and upload the script to the shared storage of your HDInsight cluster. Finally, you will trigger the application from spark-submit.

### Lab Review

> In this lab, you learned a method for designing Spark ETL processes.



## On-promise Spark Setting 

```
INSERT INTO TABLE employee VALUES(1, 'Tome', 2000);
SELECT * FROM employee;

$ tar xvfz spark-2.3.3-bin-hadoop2.7.tgz
$ cd spark-2.3.3-bin-hadoop2.7
$ cd conf
$ cp spark-env.sh.template spark-env.sh
$ cd ~/spark-2.3.3-bin-hadoop2.7/bin
$ cp slaves.template slaves
server11
server12
$ cd ~
$ scp -r spark-2.3.3-bin-hadoop2.7 server11:/home/hadoop/
$ scp -r spark-2.3.3-bin-hadoop2.7 server12:/home/hadoop/
$ cd ~/spark-2.3.3-bin-hadoop2.7/sbin

http://server10:8080
http://server10:4040
bin/spark-shell --master spark://server10:7077

http://server01:8080/ -> Spark Master UI
http://server01:4040/ -> Spark Application UI

$ spark-shell

scala> val inputfile = sc.textFile("input.txt")
scala> val counts = inputfile.flatMap(line => line.split(",")).map(word => (word, 1)).reduceByKey(_+_);
scala> counts.toDebugString
scala> counts.cache()
scala> counts.saveAsTextFile("output2")
scala> :q


SparkWordCount.scala
import org.apache.spark.SparkContext 
import org.apache.spark.SparkContext._ 
import org.apache.spark._  

object SparkWordCount { 
   def main(args: Array[String]) { 

      val sc = new SparkContext( "local", "Word Count", "/usr/local/spark", Nil, Map(), Map()) 
		
      /* local = master URL; Word Count = application name; */  
      /* /usr/local/spark = Spark Home; Nil = jars; Map = environment */ 
      /* Map = variables to work nodes */ 
      /*creating an inputRDD to read text file (in.txt) through Spark context*/ 
      val input = sc.textFile("in.txt") 
      /* Transform the inputRDD into countRDD */ 
		
      valcount = input.flatMap(line ⇒ line.split(" ")) 
      .map(word ⇒ (word, 1)) 
      .reduceByKey(_ + _) 
       
      /* saveAsTextFile method is an action that effects on the RDD */  
      count.saveAsTextFile("outfile") 
      System.out.println("OK"); 
   } 
} 

$ scalac -classpath "spark-core_2.xxxx.jar:/usr/local/spark/lib/spark-assembly-x.x.x-hadoop2.7.3.jar" SparkPi.scala
jar -cvf wordcount.jar SparkWordCount*.class spark-core_2.x.jar/usr/local/spark/lib/spark-assembly-x.x.x-hadoop2.7.3.jar
spark-submit --class SparkWordCount --master local wordcount.jar

###########파이썬########
* Pyspark (WordCount)
test = sc.textFile("README.md") 
counts = test.flatMap(lambda line: line.split(" ")).map(lambda word: (word,1)).reduceByKey(lambda a, b: a+b)

wasb://myhdinsight12345111@myhdinsghtacc1.blob.core.windows.net/user/livy/ab40thv.txt


wasb://myhdinsight12345111(컨테이너 명)@myhdinsghtacc1.blob.core.windows.net/HdiNotebooks/mytest/ab40thv.txt

test = sc.textFile("wasb://myhdinsight12345111@myhdinsghtacc1.blob.core.windows.net/HdiNotebooks/mytest/ab40thv.txt") 

counts = test.flatMap(lambda line: line.split(" ")).map(lambda word: (word,1)).reduceByKey(lambda a, b: a+b)
```

