# 스파크 Data I/O 예제 #spark #textFile #wholeTextFiles #saveAsTextFile



## **textFile() 텍스트 파일 불러오기** [Transformation 함수]

```scala
val input = sc.textFile("/usr/local/lib/spark/README.md")

input.take(3)

input: org.apache.spark.rdd.RDD[String] = /usr/local/lib/spark/README.md MapPartitionsRDD[156] at textFile at <console>:37

res146: Array[String] = Array(# Apache Spark, "", Spark is a fast and general cluster computing system for Big Data. It provides)
```



## **wholeTextFiles() 폴더 내 텍스트 파일 불러오기**

```scala
와일드카드*를 지원하여 여러 텍스트 파일 중 선택적으로 불러오기 가능

val inputAll = sc.wholeTextFiles("/usr/local/lib/spark")

inputAll.take(3)

inputAll: org.apache.spark.rdd.RDD[(String, String)] = /usr/local/lib/spark MapPartitionsRDD[158] at wholeTextFiles at <console>:37

res147: Array[(String, String)] = 

Array((file:/usr/local/lib/spark/CHANGES.txt,"Spark Change Log

\----------------

Release 1.6.1

  [SPARK-13474][PROJECT INFRA] Update packaging scripts to push artifacts to home.apache.org

  Josh Rosen <joshrosen@databricks.com>



  2016-02-26 18:40:00 -0800
```



## **saveAsTextFile() 텍스트 파일 저장**

```scala
input.saveAsTextFile("/home/test.md")
```