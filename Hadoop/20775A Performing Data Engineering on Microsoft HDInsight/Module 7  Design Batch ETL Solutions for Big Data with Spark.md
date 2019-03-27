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

- **Spark Core** [Scala & Python]
  - Spark SQL
  - Spark Streaming (건건당 들어온 데이터를 처리가능)
  - GraphX
  - MLlib

- **Anaconda**
- **Livy**
- **Jupyter Notebook**

### Common execution model

- Driver

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

### Extract: read data with SparkContext
- Use SparkContext for “extract” phase

- Any Hadoop data source supported

- Many data formats supported:

  - Text

  - Sequences

  - Serialized Java objects

  - Hadoop InputFormat

### Transform: the RDD API

- An immutable collection of objects 
- Partitioned and distributed across multiple physical nodes of a YARN cluster 
- Can be operated on in parallel
  - Transformations
  - Actions

### RDD transformations

- Apply a transformation to generate an output RDD from an input RDD
- Transformations are lazily evaluated

### RDD actions

- Return values to a calling application
- Triggers evaluation of transformations on which the action depends

### RDD example: word count ETL

- Python word count:

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
  - HBase
  - Azure DocumentDB

- Azure Data Lake—adl:// URI

- Azure SQL Data Warehouse—ODBC/JDBC

## Demonstration: Spark ETL

In this demonstration, you will see:

- How to run a simple Spark ETL process from Jupyter Notebook

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

- YARN Capacity Scheduler manages resources and queues
- A queue is allocated a percentage of total cluster resources
- Manage YARN queues through Ambari

### Debugging Spark jobs

- Yarn UI

  - Details of Yarn resource allocation

  - Opens Spark UI from the **Tracking URL** link

- Spark UI
  - View details of running jobs

- Spark History Server
  - Details of completed jobs

### Spark driver and executor settings

- Executor settings:
  - Number of executors
  - Number of cores
  - Executor memory

- Cluster-level defaults

- Job-level override

- Driver settings

  - Minimum executors

  - Maximum executors 

  - Executor memory

### Partitioning

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











## Spark Setting

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

* Pyspark (WordCount)
test = sc.textFile("README.md") 
counts = test.flatMap(lambda line: line.split(" ")).map(lambda word: (word,1)).reduceByKey(lambda a, b: a+b)

wasb://myhdinsight12345111@myhdinsghtacc1.blob.core.windows.net/user/livy/ab40thv.txt


wasb://myhdinsight12345111(컨테이너 명)@myhdinsghtacc1.blob.core.windows.net/HdiNotebooks/mytest/ab40thv.txt

test = sc.textFile("wasb://myhdinsight12345111@myhdinsghtacc1.blob.core.windows.net/HdiNotebooks/mytest/ab40thv.txt") 

counts = test.flatMap(lambda line: line.split(" ")).map(lambda word: (word,1)).reduceByKey(lambda a, b: a+b)
```

