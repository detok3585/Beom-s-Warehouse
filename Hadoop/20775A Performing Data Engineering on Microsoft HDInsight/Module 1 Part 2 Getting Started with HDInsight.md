

#### Hadoop은 3가지 모드로 구성할 수 있다

- 독립실행 모드
- 가상분산 모드
- 완전분산 모드, HA



#### 단어의 수를 세는 예제를 실행해 보자

`Hello World By World Hello` 의 내용을 가진 words.txt를 생성한다.

```
$ bin/hdfs dfs -mkdir /user/hadoop/input3 - input3 폴더생성
$ bin/hdfs dfs -put words.txt /user/hadoop/input3/ -input3 폴더에 words.txt 복사
$ bin/hdfs dfs -ls /user/hadoop/input3 -input3 폴더내 목록 출력
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount input3 output3 - 단어의 수를 세는 예제 실행
```



#### 실행 과정은 다음과 같다.

* Source
  Hello World By World Hello....
* Map Task
  Hello 1
  World 1
  By    1
  World 1
  Hello 1
* Reduce Task
  Hello 2
  World 2
  By 1

#### 결과는 아래 명령어로 확인한다

```
$ bin/hdfs dfs -cat /user/hadoop/output3/*
```

![image](https://user-images.githubusercontent.com/46669509/54730060-0e7c1280-4bca-11e9-98ca-5b05748487eb.png)





![image](https://user-images.githubusercontent.com/46669551/54655336-f1c9d700-4b04-11e9-9b2a-0e0456346001.png)

[그림]하둡 에코시스템 2.0



#### 사용환경을 필요한 요소만 모아 구성할 수 있다.

Sqoop, Flume -> HDFS + Apache Spark(Scala, Python, R)

Sqoop, Flume -> HDFS + MapReduce(Java)

Sqoop, Flume -> HDFS + Machine Learning Server(Python, R)

Storm + Kafka -> HDInsight + (Machine Learing) + Visualiztion on Azure(Cloud Environment)



## Lab :  Working With HDInsight 

### Exercise 1  : Provision an HDInsight cluster and run MapReduce jobs

- Task 1 : Create an HDInsight cluster and run a word count MapReduce job

  E:\Labfiles\Lab01에 있는 Create and Run WC.ps1를 실행하여 클러스터를 만들고 실행해보자.

  

### Exercise 2 : Run a Yarn job



멀티캠퍼스에서는 포트가 막혀있어 이번랩 생략...