# 스파크 key/value RDD 예제 #spark #reduceByKey #groupByKey #combineByKey # mapValues #keys #values #sortByKey



## **map() pair RDD 생성** [Transformation 함수]

```scala
스칼라에서 README를 spark context 객체의 textFile 메서드를 이용해 읽어오면 RDD 객체가 생성됨

이후 map 함수를 이용해 첫 번째 단어를 키로 사용한 pair RDD 생성

scala> val lines = sc.parallelize(List("holden likes coffee","panda likes long strings and coffee"))

scala> pairs = lines.map(x=>(x.split(" ")(0),x))

scala> pairs.first()

lines: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[112] at parallelize at <console>:32

pairs: org.apache.spark.rdd.RDD[(String, String)] = MapPartitionsRDD[113] at map at <console>:34

res98: (String, String) = (holden,holden likes coffee)
```



## **filter() 단순 필터 적용** [Transformation 함수]

```scala
scala> pairs.filter{case(key,value)=>value.length>20}.first()

res106: (String, String) = (panda,panda likes long strings and coffee)
```



## **flatMap() 라인별 단어별 잘라서 단어들의 집합으로 변환** [Transformation 함수]

```scala
scala> val pairs1 = lines.flatMap(x=>x.split(" ")).map(word=>(word,1)).take(10)

pairs1: Array[(String, Int)] = Array((holden,1), (likes,1), (coffee,1), (panda,1), (likes,1), (long,1), (strings,1), (and,1), (coffee,1))
```



## **mapValues() 각 value에 count를 위한 1을 붙이고** [Transformation 함수]

## **reduceByKey() key별 (value의 총합, 값 갯수)** [Transformation 함수]

```scala
scala> pairs1.mapValues(x=>(x,1)).reduceByKey((x,y)=>(x._1+y._1,x._2+y._2)).take(6)

res126: Array[(String, (Int, Int))] = Array((long,(1,1)), (coffee,(2,2)), (holden,(1,1)), (likes,(2,2)), (panda,(1,1)), (strings,(1,1)))
```



## **combineByKey() key별 집합 연산 일반적으로 사용 -> map-side 집합연산**

```scala
한 파티션 내의 데이터들을 하나씩 처리. 각 데이터는 이전에 나온 적이 없는 키를 갖고 있거나 이전에 나온 적이 있는 키

새로운 데이터 경우 createCombiner()함수로 해당 키에 대한 accumulator 생성.

이전에 나온 키의 경우 mergeValue()함수로 합함.

파티션별 계산이 끝나고 RDD전체에서 최종적으로 결과를 합칠 때 동일 키에 대한 accummulator를 가지면 mergeCombiner()함수로 합함.

//예제 -> input이 없음. 함수 인자들만 참고

scala> val result = input.combineByKey((v)=>(v,1),(acc:(Int,Int),v)=>(acc._1+v,acc._2+1),(acc1:(Int,Int),acc2:(Int,Int))=>(acc1._1+acc2._1,acc1._2+acc2._2)).map{case(key,value)=>(key,vlaue._1/value._2.toFloat)}

scala> result.collectAsMap().map(println(_))



병렬화 수준지정. 스파크에 특정 개수의 파티션을 사용하라고 요청

val data = Seq(("a",3),("b",4),("a",1))

sc.parallelize(data).reduceByKey((x,y)=>x+y).take(2)



sc.parallelize(data).reduceByKey((x,y)=>x+y,10).take(2)
```



## **join()**

```scala
데이터 조인, left right outer join, cross join, inner join 가능

//storeAdd ={ (Store("R"),"1026"),(Store("P"),"748"),(Store("P"),"3101"),(Store("S"),"Seattle")}

//storeRate = {(Store("R"),4.9),(Store("P"),4.8)}



//storeAdd.join(storeRate) = {(Store("R"),("1026",4.9)), (Store("P"),("748",4.8)), (Store("P"),("3101",4.8))}
```

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



## sort() 데이터 정렬

```scala
문자열 비교 함수

//val input:RDD[(Int,Venue)] =

implicit val sortIntegerByString = new Ordering[Int]{

​    override def compare(a:Int, b:Int) = a.toString.compare(b.toString)

}


//rdd.sortByKey(sortIntegerByString)
```



