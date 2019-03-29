# Module 8 : Analyzing Data with Spark SQL

## Module Overview

- Implementing iterative and interactive queries
- Performing exploratory data analysis

### Prerequisites

1. Create an HDInsight cluster by using the Azure Portal
2. Create a new storage account
3. Install putty.exe



## Lesson 1: Implementing iterative and interactive queries

- <u>Interactive analysis with the Spark shell</u>
- <u>Iterative and interactive operations in Spark RDDs</u>
- Dataset and DataFrame APIs, RDDs, and Spark SQL
- Unified APIs in Spark 2.0
- SparkSession context
- Working with the Dataset API
- Data formats, including Parquet
- Working with Parquet files in Spark
- Caching
- Persistence
- Choosing a storage level
- Shared variables: Broadcast variables and accumulators
- Managing Spark Thrift Server and resource allocation in Apache Hadoop YARN

### Interactive analysis with the Spark shell
- Scala:

```
$ SPARK_HOME/bin/spark-shell
```

- Python:

```
$ SPARK_HOME/bin/pyspark
```

### Iterative and interactive operations in Spark RDDs
- Iterative vs interactive operations on a Spark RDD

![image](https://user-images.githubusercontent.com/46669551/55056731-50b1c200-50aa-11e9-94e2-870caa4ff337.png)

### RDDs have three key features:

- Resilience: RDDs can be re-created if the data is removed from memory
- Distributed: RDDs are held in memory and can be partitioned across many data nodes in a cluster
- Dataset: RDDs can be created from files, created programmatically from data already in memory or created from another RDD

### Dataset and DataFrame APIs, RDDs, and Spark SQL

The Dataset API is a data abstraction framework that organizes your data into named columns:

- Creates a schema for the data
- Is conceptually like a table in a relational database, or a dataframe in Python or R
- Provides a relational view of the data for easy SQL-like data manipulations and aggregations
- Is essentially a row of RDDs

> The SparkSQL module in Spark provides such an interface, for processing structured data. There are two ways to interact with SparkSQL:
>
> - SQL, a subset of ANSI SQL
> - The Dataset API

### Unified APIs in Spark 2.0

![image](https://user-images.githubusercontent.com/46669551/55056770-7343db00-50aa-11e9-9b6a-a4b3c57711c2.png)

> The typed interface and untypedinterfaces are optimized for different tasks:
>
> - The **typed interface** is most suitable for **data engineering tasks**. Languages that have compilation-time type checking use the typed API. It returns a type of **Dataset[T]**.
> - The **untyped interface** is most suitable for **interactive analysis**. Languages that do not have compilation-time type checking, such as R and Python, use the untyped API. It returns a type of **Dataset[Row]**

### SparkSession context

SparkSession provides access to:

- View and set runtime properties for Spark
- View catalog metadata
- Execute SQL
- Work with Hive

### - Creating a SparkSession object

```
// Create a SparkSession. No need to create SparkContext
// as it is created as part of the Sparksession
val warehoseLocation = "file:${system:user.dir}/spark-warehouse"
val spark = SparkSession
	.builder()
	.appName("SparkSessionZipsExample")
	.config("spark.sql.warehouse.dir", warehouseLocation)
	.enableHiveSupporter()
	.getOrCreate()
```

### - Setting runtime properties

```
spark.conf.set("spark.sql.shuffle.partitions", 6)
spark.conf.set("spark.executor.memory", "2g")
```

### - Accessing catalog metadata

```
spark.catalog.listDatabases.show(false)
spark.catalog.listTables.show(false)
```

### - Using SparkSession sql

```
cityDF.createOrReplaceTempView("cities_table")
cityDF.cache()
val resultsDF = spark.sql("SELECT city, pop, state, zip FROM cities_table")
resultsDF.show(5)
```

### - Saving and reading from Hive

```
spark.table("cities_table").write.saveAsTable("cities_hive_table")
```



###  Working with the Dataset API

```json
case class ClimateData (c02_level: Long, device_id: Long, device_name: String, humidity_pct: Long, ip: String, iso_3166_short_code: String, latitude: Double, longitude: Double, scale:String, temperature: Long, timestamp: Long)
```

```json
val ds = spark.read.json(“/data/climate.json”).as[ClimateData]
```

### Data formats, including Parquet

- Apache Parquet is:
  - A compressed, columnar data format
  - An Apache Software Foundation project
  - Supported by the entire Hadoop ecosystem
  - Capable of up to three times greater performance than other compressed and uncompressed file types

### Working with Parquet files in Spark

1. Querying Parquet with SQL

```python
val sqlDF = spark.sql("SELECT * FROM parquet.`/data/sample.parquet`")
```

2. Saving Spark dataframes as Parquet files

- Writing Parquet

```python
import spark.implicits._
citiesDF.write.parquet("cities.parquet")
```

- Temporary view in parquet

```python
parquetFileDF.createOrReplaceTempView("parquetFile")
val namesDF = spark.sql("SELECT product FROM parquetFile WHERE number_of_regions BETWEEN 13 AND 19")
```

### Caching

Store an object in memory to improve performance

```python
words = doc.flatMap(lambda x: x.split()) \ 
	.map(lambda x: (x,1)) \ 
    .reduceByKey(lambda x, y: x + y) words \
    .cache () 
```

```python
sql("CACHE TABLE [climateData]")
spark.cacheTable(“climateData”)
```

```python
spark.uncacheTable("climateData")
```

### Persistence (연속성)

- Persist RDD objects to optimize performance or recovery time:
  - **In memory, on disk, or both**
  - Control serialization
  - Add replicas 
- Not all clients support all storage levels
- Unpersist to set storage level to none

Cache 는 메모리에 저장하며, 유효기간이 있다. 

Persistence 는 메모리 또는 하드에 저장하여 유효기간이 없다고 볼 수 있다.

> The following storage levels are supported
>
> - NONE
> - MEMORY_ONLY
> - MEMORY_AND_DISK
> - MEMORY_ONLY_SER
> - MEOMRY_AND_DISK_SER
> - DISK_ONLY
> - DFF_HEAP

### Checking storafe levels

: Use the **getStorageLevel** method to display the storage level of an RDD object.

```
res0: org.apache.spark.storage.StorageLevel = StorageLevel(disk=false, memory=false, iffheap-false, deserialized=false, replication=1)
```



### Choosing a storage level

- Use MEMORY_ONLY for best performance
- Use serialization to reduce memory usage at the cost of greater CPU
- Use disk storage only when it performs better than recomputation
- Use replication to improve recovery times after failures

### Shared variables: Broadcast variables and accumulators

전역변수를 사용하고 싶을 때

- Broadcast variables:
  - Shared read-only values
  - Value can be any object
  - Values shared by all workers


```
countries = sc.broadcast ({'012':'Algeria','356':'India','826':'United Kingdon'})
```

#### 예제

```python
test =sc.broadcast([1,2,3,4,5,6,7,8,9,10])
```

```
test.value
```

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```



- Accumulators:
  - Shared writable values
  - Write at worker, read at driver
  - Numeric types supported by default
  - Workers update value by using **Add** or **+=**

```
val accum = sc.longAccumulator("SumAccumulator")
sc.parallelize(Array(2,3,4,5)).foreach(x => accum.add(x))
```

> - 연관된 연산을 통해 "추가" 만을 할 수 있는 변수
> - 병렬을 통해 효율적으로, 개수 및 합계를 구현하는데 사용
> - Spark는 숫자 값 형식과 표준 변경가능 컬렉션의 Accumulator을 시본으로 지원하여, 프로그래머는 새 형식으로 확장 할 수 있음
> - Driver 프로그램만(Task가 아님) Accumulator의 값을 읽을 수 있음

### Managing Spark Thrift Server and resource allocation in Apache Hadoop YARN
- Spark Thrift Server:
  - Provides ODBC or JDBC interface to Spark SQL
  - Available resources are configured through Ambari
  - Service can be stopped if not needed

- YARN resources:
  - **Memory Used** and **Memory Total**
  - *VCores Used** and **VCores Total**
  - Possible to kill applications to free resources

## 예제

```python
from pyspark.sql import SparkSession
spark2 = SparkSession \
    .builder \
    .appName("MySpark") \
    .config("spark.executor.memory","2G") \
    .getOrCreate()
    
df = spark2.read.json("wasb:///user/sshuser/Cars.json", multiLine=True)
df2 = df.toPandas()

df.show()
df.select("name").show()
df.select(df['name'], df['speed']).show()
df.filter(df['speed'] > 100).show()
df.filter(df['speed'] > 100).count()
df.groupby("name").count().show()
df.createOrReplaceTempView("Cars")

df4 = spark2.sql("select * from Cars")

type(df4)
```

### 출력 값

```python
+------+-------------+-----+------+
|itemNo|         name|speed|weight|
+------+-------------+-----+------+
|     1|      ferrari|  259|   800|
|     2|       jaguar|  274|   998|
|     3|     mercedes|  340|  1800|
|     4|         audi|  345|   875|
|     5|  lamborghini|  355|  1490|
|     6|    chevrolet|  260|   900|
|     7|         ford|  250|  1061|
|     8|       porche|  320|  1490|
|     9|          bmw|  325|  1190|
|    10|mercedes-benz|  312|  1567|
+------+-------------+-----+------+

+-------------+
|         name|
+-------------+
|      ferrari|
|       jaguar|
|     mercedes|
|         audi|
|  lamborghini|
|    chevrolet|
|         ford|
|       porche|
|          bmw|
|mercedes-benz|
+-------------+

+-------------+-----+
|         name|speed|
+-------------+-----+
|      ferrari|  259|
|       jaguar|  274|
|     mercedes|  340|
|         audi|  345|
|  lamborghini|  355|
|    chevrolet|  260|
|         ford|  250|
|       porche|  320|
|          bmw|  325|
|mercedes-benz|  312|
+-------------+-----+

+------+-------------+-----+------+
|itemNo|         name|speed|weight|
+------+-------------+-----+------+
|     1|      ferrari|  259|   800|
|     2|       jaguar|  274|   998|
|     3|     mercedes|  340|  1800|
|     4|         audi|  345|   875|
|     5|  lamborghini|  355|  1490|
|     6|    chevrolet|  260|   900|
|     7|         ford|  250|  1061|
|     8|       porche|  320|  1490|
|     9|          bmw|  325|  1190|
|    10|mercedes-benz|  312|  1567|
+------+-------------+-----+------+

+-------------+-----+
|         name|count|
+-------------+-----+
|       jaguar|    1|
|     mercedes|    1|
|  lamborghini|    1|
|         audi|    1|
|          bmw|    1|
|         ford|    1|
|      ferrari|    1|
|mercedes-benz|    1|
|    chevrolet|    1|
|       porche|    1|
+-------------+-----+

<class 'pyspark.sql.dataframe.DataFrame'>
```



## Lesson 2: Performing exploratory data analysis
- Apache Zeppelin, Livy, and Jupyter Notebooks
- Using Zeppelin and Jupyter Notebooks for visualization
- Joining dataframes with Spark
- Demonstration: Joining dataframes
- Using Livy to access Spark
- Managing interactive Livy sessions
- Demonstration: Using Livy to submit a Spark application

### Apache Zeppelin, Livy, and Jupyter Notebooks
- Apache Zeppelin
  - Notebook for data visualization and exploration

- Livy
  - REST-based job server for Spark

- Jupyter Notebooks
  - Notebook for data visualization and exploration
  - Built on Livy

### Using Zeppelin and Jupyter Notebooks for visualization
- Browser-based tools for data exploration and visualization:
  - Work organized into notebooks
  - Many similarities

- Some key differences:
  - Style of user interface
  - Availability can vary on Spark version
  - Polyglot notebook support
  - Local execution
  - Storage location

### Joining dataframes with Spark

- Use **join** to join dataframes

```
RDD.join(second RDD, [join column(s)], [join type])
```

- Join columns control how the join is made
- Join type defines the join:
  - **Inner** (default)
  - **Left Outer**
  - **Right Outer**
  - Full Outer
  - **Left Semi**

- Might use dataframes as broadcast variables

## Demonstration: Joining dataframes

- In this demonstration, you will see the effects of various types of dataframe
  joins

1. Dataframe을 Jupyter Notebook에서 생성

```
from pyspark.sql import Row
data1 = [('Alpha', 1),('Bravo', 2), ('Charlie', 3)]
df1 = sqlContext.createDataFrame(sc.parallelize(data1), ['name','id'])
df1.collect()
---------------------------------------------------------------------------------------------------
[Row(name=u'Alpha', id=1), Row(name=u'Bravo', id=2), Row(name=u'Charlie', id=3)]
```

2. 두번째 Dataframe을 생성한다.

```
data2 = [('Alpha', 1), ('Charlie', 3), ('Delta',4)]
df2 = sqlContext.createDataFrame(sc.parallelize(data2), ['name', 'id'])
df2.collect()
---------------------------------------------------------------------------------------------------
[Row(name=u'Alpha', id=1), Row(name=u'Bravo', id=2), Row(name=u'Charlie', id=3)]
```

3. Inner Join을 생성

```
df1.join(df2,'id').collect()
---------------------------------------------------------------------------------------------------
[Row(id=1, name=u'Alpha', name=u'Alpha'), Row(id=3, name=u'Charlie', name=u'Charlie')]
```

4. Left outer Join을 생성

```
df1.join(df2,'id','left_outer').collect()
---------------------------------------------------------------------------------------------------
[Row(id=1, name=u'Alpha', name=u'Alpha'), Row(id=3, name=u'Charlie', name=u'Charlie'), Row(id=2, name=u'Bravo', name=None)]
```

5. Right outer Join을 생성

```
df1.join(df2,'id','right_outer').collect()
---------------------------------------------------------------------------------------------------
[Row(id=1, name=u'Alpha', name=u'Alpha'), Row(id=3, name=u'Charlie', name=u'Charlie'), Row(id=4, name=None, name=u'Delta')]
```

6. Full outer Join을 생성

```
df1.join(df2,'id','outer').collect()
---------------------------------------------------------------------------------------------------
[Row(id=1, name=u'Alpha', name=u'Alpha'), Row(id=3, name=u'Charlie', name=u'Charlie'), Row(id=2, name=u'Bravo', name=None), Row(id=4, name=None, name=u'Delta')]
```

7. Left semi Join을 생성

```
df1.join(df2, 'id','left_semi').collect()
---------------------------------------------------------------------------------------------------
[Row(id=1, name=u'Alpha'), Row(id=3, name=u'Charlie')]
```

8. explicit join columns를 사용한다.

```
df1.join(df2,df1.id == df2.id).collect()
---------------------------------------------------------------------------------------------------
[Row(name=u'Alpha', id=1, name=u'Alpha', id=1), Row(name=u'Charlie', id=3, name=u'Charlie', id=3)]
```



### Using Livy to access Spark

- REST interface for Spark
- Scala and Python jobs supported
- Automatic recycling of resources
- Fault-tolerance

> curl 이라는 명령어는 웹을 요청하는 명령어
>
> ```
> curl -k --user "admin:mypassword!" -v -X GET https://cluster.azurehdinsight.net/livy/sessions
> ```
>
> ```
> 
> ```
>
> 

### Managing interactive Livy sessions

- For a list of sessions, use **GET /sessions**
- For session detail, use **GET /sessions/{sessionid}**
- For a session log, use  **GET /sessions/{sessionid}/log**
- For session statements, use  **GET /sessions/{sessionid}/statements**
- To end a session, use  **DELETE /sessions/{sessionid}**
- To cancel a session statement, use  **POST** **/sessions/{sessionId}/statements/ {statementId}/cancel** 

## Demonstration: Using Livy to submit a Spark application
- In this demonstration you will see how to submit, monitor, and manage Spark batches from Livy

```
* Using Powershell
* curl-7.64.1-win64-mingw.zip -> windows curl

* 배치 확인
curl -k --user "admin:Pa$$w0rd2019" -v -X GET "https://myhdinsight1234411.azurehdinsight.net/livy/batches"

* Ambari Server 설정 변경 
sudo vi /etc/ambari-server/conf/ambari.properties
     livy.server.csrf_protection.enabled=false

* PowerShell Script 참고 
$cred=Get-Credential

Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches

$json = @{ file="wasbs:///example/jars/SimpleSparkApp.jar";
className = "com.microsoft.spark.example.WasbIOTest"} | ConvertTo-Json

* 기존의 코드에 추가 Header정보 추가 - 특정 Page를 요청할 때에 받는 쪽에서 정확히 하기 위함.
* 추가를 안해주면 CSRF protection 오류가 생기게 된다.
###################################################################################################
# Invoke-RestMethod :                                                                             #
# Error 400                                                                                       #
# HTTP ERROR: 400                                                                                 #
# Problem accessing /batches. Reason:                                                             #
#     Missing Required Header for CSRF protection.                                                #
# Powered by Jetty://                                                                             #
###################################################################################################
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("X-Requested-By","admin")
$headers


Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches -Method Post -body $json -ContentType 'application/json' -Headers $headers

Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches -Method POST -body $json -ContentType 'application/json' -Headers $headers

Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches

Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches/0

Invoke-RestMethod -Credential $cred -Uri https://kdongxie2019cn.azurehdinsight.net/livy/batches/0 -Method DELETE -Headers $headers
```



## Lab: Performing exploratory data analysis by using iterative and interactive queries

### Lab Scenario

> You work as a consultant for Contoso, a large IT consultancy that has consultants in different industrial sectors. You have been asked to perform some exploratory analysis on a dataset from one of Contoso’s clients, a multinational real estate management company. The client is hoping to use data collected from 20 large buildings around the world to analyze usage of heating, ventilation, and air-conditioning (HVAC) systems. 
>
> In this lab, you will use Jupyter Notebooks to build a machine learning model, and Zeppelin for interactive data exploration, modeling, and visualization. 
>
> Finally, you will use Livy to interact with your Spark cluster.

- Exercise 1: Build a machine learning application

You want to create an iterative machine learning application to predict whether a building will be hotter or colder than its target temperature, based on the system identity and system age. 

**Note:** A text file that contains the code used in this lab can be found at **E:\Labfiles\Lab08\HVAC MLlib.txt** on the virtual machine that accompanies this course.

- Exercise 2: Use Zeppelin for interactive data analysis

You decide to use Zeppelin to analyze and visualize the HVAC dataset.

**Note:** A text file that contains the code used in this lab can be found at **E:\Labfiles\Lab08\HVAC Zeppelin.txt** on the virtual machine that accompanies this course.

Instructor Note: At the time of writing, Zeppelin is not available on HDInsight on Azure clusters using Spark 2.0. New Spark 1.6 clusters include Zeppelin by default. 

- Exercise 3: View and manage Spark sessions by using Livy

While working on data analysis tasks, you decide to monitor the status of all Livy sessions that are connected to your HDInsight on Azure instance.

Instructor Note: This exercise relies on the students having made connections to their cluster by using Jupyter or Zeppelin in one of the two previous exercises.

If students are more comfortable with curl, and curl is available, encourage them to use it instead of the PowerShell **Invoke-RestMethod** command.

### Lab Review

- How might you use a machine learning application in your organization?
- How might you implement Zeppelin or Livy in your company’s environment?



# Map & Reduce [RDD 파일의 형식에서만 가능]

## Jupyter Notebook Start

```python
sc
```

```python
from pyspark import SparkContext
sc2 = SparkContext("local", "Test App")
```



```python
<SparkContext master=yarn appName=remotesparkmagics>
```

In [16]: 알리바바와 40인의 도적들 text파일을 호출한다.

```python
file = "wasbs:///Test_Data/ab40thv.txt"
data = sc.textFile(file)
type(data)
```



```
<class 'pyspark.rdd.RDD'>
```

In [23]: lambda를 통해 임시 함수를 선언하기

```python
#data.collect()
myfunc = lambda a : a + 1
#myfunc(1)
#def myfunc2(a):
#  a = a + 1
#myfunc2(2)
data.count()
```



```
172
```

In [24]:

```python
data2 = data.filter(lambda s: 'a' in s) # tranformations
data2.count()
#data2.collect()
```



```
166
```

In [ ]:

```python
# Transformation
# Action
```

In [26]:

```python
data4 = sc.parallelize(['java', 'python', 'scala', 'c', 'c++', 'javascript'])
```

In [36]: RDD를 통한 MapReduce는 Action method가 선언되기 전까지는 Transformation만을 진행한다.

```
# Action
data4.count()
data4.collect()
```



```
['java', 'python', 'scala', 'c', 'c++', 'javascript']
```

In [67]: RDD type으로 변환 하기

```python
data5 = sc.parallelize([1,2,3,4])
data5.collect()
type(data5)
```



```
[1, 2, 3, 4]
<class 'pyspark.rdd.RDD'>
```

In [104]:

```python
data6 = data.filter(lambda s: 'a' in s) # tranformations
#data6.collect() <- 알리바바 파일안의 a로 이루어진 문장만을 가져오게 된다 # Action
```

In [120]:

```python
data7 = data.flatMap(lambda line: line.split(" "))
data7.collect()
data8 = data7.map(lambda s: (s, 1))
data9 = data8.collect()
type(data9)
type(data9[1])
data.flatMap(lambda line: line.split(" ")).map(lambda a : (a, 1)).count()
```



```
2421
```

In [126]:

```python
# reduce
from operator import add
data10 = sc.parallelize([1,2,3,4,5])
data10.reduce(add)

data10.reduce(lambda a, b: a+b)
```



```
15
```

In [133]:

```python
data11 = sc.parallelize([('a',2),('a',1),('b',1),('a',2)])
data11.collect()
data11.count()

data11.reduceByKey(lambda x, y: x+y).collect()
```



```
[('b', 1), ('a', 5)]
```

In [135]: **Cache** [빠르게 호출하기 위한 방법 (데이터가 클경우 캐슁을 통해 빠르게 호출이 가능하다)]

```python
data12 = sc.parallelize([('a',2),('a',1),('b',1),('a',2)])
data12.cache()
```



```
ParallelCollectionRDD[153] at parallelize at PythonRDD.scala:194
```

In [137]:

```python
data12.reduceByKey(lambda a, b: a * b * 2).collect()
```

원소가 하나일 시에는 연산이 이루어 지지 않고 그대로 출력 된다.

```
[('b', 1), ('a', 16)]
```

In[138]

```python
data13 = sc.parallelize([('a',2),('a',1),('b',1),('a',2),('b',1)])
data13.reduceByKey(lambda a, b: a * b * 2).collect()
```

원소가 2개 이상일 때 연산이 이루어 진 후 출력 된다.

```
[('b', 2), ('a', 16)]
```




