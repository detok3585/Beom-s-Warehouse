* 1988.csv를 사용하여 도착 지연횟수가 가장 많은 상위 10개의 항공사를 구하고 그래프로 출력하시오.
  (.csv) -> SparkSql
* Text-Books폴더의 Top 10개 단어의 빈도수를 구하고 히스토그램을 출력하시오.(Plain-Text)
  -> Transformations & Actions

.toPandas() => DataFrame



* Answer01

data_csv = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("inferSchema", "true").load("wasb:///Test_Csv/1988_2.csv")

data_csv.createOrReplaceTempView("airline_data_1988")

top10 = sqlContext.sql("select distinct(UniqueCarrier), count(ArrDelay) as cnt from airline_data_1988 group by UniqueCarrier order by cnt desc LIMIT 10")

import pandas as pd
df = top10.toPandas()

import numpy as np 
from matplotlib import pyplot as plt
plt.plot(df['UniqueCarrier'], df['cnt']) 
plt.show()




* Answer02

Test_Data = spark.sparkContext.textFile('wasb:///Test_Data')
Temp_Data = Test_Data.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a,b:a+b)
Temp_Data.sortBy(lambda k: k[1], False).take(5)