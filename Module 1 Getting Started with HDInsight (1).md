# Module 1: Getting Started with HDInsight

### Module Overview

- What is big data?
- Introduction to Hadoop
- Working with MapReduce functions on HDInsight
- Introducing HDInsight

### 기술 정리

- HDInsight
  - 대량 데이터를 쉽고 빠르며 비용 효율적으로 처리할 수 있도록 하는 클라우드 서비스. HDInsight는 또한 추출, 변환 및 로드(ETL), 데이터 웨어하우징, Machine Learning, IoT와 같은 광범위한 시나리오를 지원.
- Apache Hadoop(High-Availability Distributed Object-Oriented Platform)
  - 대량의 자료를 처리할 수 있는 큰 컴퓨터 클러스터에서 동작하는 분산 응용 프로그램을 지원하는 [프리웨어](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A6%AC%EC%9B%A8%EC%96%B4) [자바](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)) [소프트웨어 프레임워크](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)이다. 분산처리 시스템인 [구글 파일 시스템](https://ko.wikipedia.org/wiki/%EA%B5%AC%EA%B8%80_%ED%8C%8C%EC%9D%BC_%EC%8B%9C%EC%8A%A4%ED%85%9C)을 대체할 수 있는 하둡 분산 파일 시스템(HDFS: Hadoop Distributed File System)과 [맵리듀스](https://ko.wikipedia.org/wiki/%EB%A7%B5%EB%A6%AC%EB%93%80%EC%8A%A4)를 구현한 것이다.
- Apache Hive
  - [하둡](https://ko.wikipedia.org/wiki/%ED%95%98%EB%91%A1)에서 동작하는 [데이터 웨어하우스](https://ko.wikipedia.org/wiki/%EB%8D%B0%EC%9D%B4%ED%84%B0_%EC%9B%A8%EC%96%B4%ED%95%98%EC%9A%B0%EC%8A%A4)(Data Warehouse) 인프라 구조로서 데이터 요약, 질의 및 분석 기능을 제공한다. 아파치 [HDFS](https://ko.wikipedia.org/wiki/HDFS)이나 [아파치 HBase](https://ko.wikipedia.org/wiki/%EC%95%84%ED%8C%8C%EC%B9%98_HBase)와 같은 데이터 저장 시스템에 저장되어 있는 대용량 데이터 집합들을 분석.
- Apache Spark
  - 메모리 내 처리를 지원하여 빅 데이터 분석 애플리케이션의 성능을 향상하는 병렬 처리 프레임워크.
- Apache Phoenix
  - NoSQL 데이터베이스인 HBase 를 관계 데이터베이스 처럼 사용할 수 있게 하는 오픈소스.
- Apache Kafka
  - 오픈 소스 분산형 스트리밍 플랫폼입니다. 게시-구독 메시지 큐와 유사한 기능을 제공하므로 메시지 브로커로 자주 사용.
- Apache Storm
  - 데이터 스트림 처리용 확장 가능한 분산형 실시간 계산 시스템.
- Spark Streaming 
  - 다양한 소스로 부터 실시간 스트리밍 데이터 처리.

![image](https://www.dropbox.com/s/iyw3faoyy8dp7ha/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202018-06-12%2022.24.17.png?raw=1)

- Apache HBase

  - 분산되고 확장 가능한 대용량 데이터 저장소 인 [Hadoop](http://hadoop.apache.org/) 데이터베이스.

- Azure Stream Analytics

  - 디바이스에서 대용량의 데이터 스트리밍을 검사할 수 있도록 하는 이벤트 처리 엔진. 들어오는 데이터는 장치, 센서, 웹 사이트, 소셜 미디어 피드, 애플리케이션 등에서 가져옴. 또한 데이터 스트림의 정보 압축, 패턴 및 관계 식별을 지원

- Yarn(Yet Another Resource Nagotiator)

  - 하둡 2.0부터 제공되는 리소스 관리 플랫폼.

    각 어플리케이션(HBase, Accumulo, Storm, MapReduce...)에 필요한 리소스(CPU, 메모리, 디스크 등)를 할당하고 모니터링하는 업무 수행.

    **MapReduce의 단점**을 극복하기 위해서 시작된 프로젝트

### Lesson 1: What is big data?

- Defining the three Vs of big data
- Listing processing techniques
- Identifying big data use cases

### Defining the three Vs of big data

The three Vs are Volume, Variety, and Velocity

- **Volume** describes datasets that are too large to fit into a <u>traditional relational database</u> [크기가 크다]

- **Variety** results from analyzing many different types of data [종류가 많다]

- **Velocity** covers data that is constantly arriving

###### MongoDB: Schema를 설정하지 않아도 DataBase에 입력이 가능

### Listing processing techniques

- Batch query (축척된 데이터를 처리하는 형식)
  - Answers the question: “How many gallons of water are in this still body of water?”

- Streaming query (IOT Device에서 발생되는 Data들 : Delay time은 존재하나 실시간 처리가 가능하다)
  - Answers the question: “How many gallons of water have passed through this **flowing** river in the last hour?

- Hybrid approach (Batch와 Streaming을 합쳐 놓은 형태)

![image](https://user-images.githubusercontent.com/46669551/54651157-e66eaf80-4af4-11e9-83a0-e10980394800.png)

### Identifying big data use cases

- Manufacturing

- Finance

- Extreme sports

###### **Question**: What are some examples of big data use cases in your industry?

### Lesson 2: Introduction to Hadoop

- Overview of the Hadoop programming framework
- Working with Hadoop Distributed File System
- Identifying the MapReduce process
- Managing resources with YARN

![image](https://user-images.githubusercontent.com/46669551/54652015-9691e780-4af8-11e9-972f-15bca31bd89f.png)

###### Apache 오픈 소스 프로젝트

> Highly Scalable distributed file system (HDFS)

###### 데이터 노드에서 분산 프로세싱
![image](https://user-images.githubusercontent.com/46669551/54651429-313cf700-4af6-11e9-911e-f9e6afa74b19.png)

### Data Volume
##### Hadoop은 파일을 분산 파일 시스템에 저장

- 저장소와 컴퓨팅은 여러 노드에 걸쳐 분산된다

- 파일은 여러 대의 노드로 펼쳐져서 저장될 수 있다.

##### Hadoop은 매우 큰 볼륨의 데이터를 저장할 수 있음

- 결합된 저장소 리소스로 몇 개의 노드에서 수 천개의 노드로 확장할 수 있음

- 선형 스케일 아웃

- 단일 노드[computer]의 용량보다 큰 파일을 포함하는 대용량 파일도 지원

![image](https://user-images.githubusercontent.com/46669551/54652278-aeb63680-4af9-11e9-8d95-90a630a07ffa.png)

### Data Variety
##### Hadoop은 데이터를 저장 (비 관계형 저장소)

- 파일은 다양한 반구조적 또는 비구조적 데이터를 포함할 수 있다

- 이전에는, 이러한 파일들은 가치가 인사이트를 제공하지 못했다

- 오늘날, 새로운 비즈니스 질문과 인사이트가 데이터 과학을 통해 발견되고 있음

![image](https://user-images.githubusercontent.com/46669551/54652333-e0c79880-4af9-11e9-9bb5-623c1db8fe0a.png)

### Data Velocity
##### Hadoop은 데이터를 라이브 스트리밍 하고 실시간으로 프로세싱 할 수 있음

- Hadoop은 규모있는 이벤트 스트림 데이터를 수집할 수 있음

![image](https://user-images.githubusercontent.com/46669551/54652382-0ce31980-4afa-11e9-8458-96cb2d2e1c90.png)

### Hadoop은 여러 프로젝트로 구성된 플랫폼

##### Apache Software Foundation (ASF)에 의해 통제됨

##### MapReduce, HDFS, YARN으로 구성된 핵심 서비스

##### 코어 서비스에 전체적인 기능들을 포함: 

- 데이터를 조작하고 이동할 수 있도록 하는 데이터 서비스(Hive, Hbase, Pig, Flume, Sqoop)

- 클러스터를 관리할 수 있는 운영 서비스 (Ambari, Falcon, Oozie)

![image](https://user-images.githubusercontent.com/46669551/54652446-3f8d1200-4afa-11e9-980a-b1c8add12460.png)

### Hadoop 배포본은 프로젝트의 패키지
##### 전체 패키지의 일관성에 대한 테스트

![image](https://user-images.githubusercontent.com/46669551/54652515-75ca9180-4afa-11e9-81bc-001c675ddfa3.png)



### HDFS

![image](https://user-images.githubusercontent.com/46669551/54652631-d4900b00-4afa-11e9-91fe-1387d4ff0ce0.png)

##### HDFS는 분산 파일 시스템

- 몇 개의 노드에서 수 천개의 노드로 확장

- 파일은 여러 대의 노드로 펼쳐져서 저장될 수 있음

##### HDFS는 대용량 데이터를 저장할 수 있음

- 단일 노드의 용량보다 큰 파일을 포함하는, 대용량 파일도 지원

##### HDFS는 비관계형 데이터를 포함

![image](https://user-images.githubusercontent.com/46669551/54652691-fee1c880-4afa-11e9-988a-5417b5b5a2c7.png)

### 파일 분할
![image](https://user-images.githubusercontent.com/46669551/54652760-6ac43100-4afb-11e9-883e-c026253950d6.png)

### 블록 배치(Replication Option)

![image](https://user-images.githubusercontent.com/46669551/54652795-90513a80-4afb-11e9-8644-0ecbc1d58477.png)

- **첫 번째 카피**는 파일을 생성한 노드에 기록됨 **(쓰기 선호)**

- **두 번째 카피**는 **같은** **랙** **(rack)**에 있는 데이터 노드에 기록됨 **(랙을 가로지르는 과정에서 발생할 수 있는 네트워크 부하 최소화)**

- **세 번째 카피**는 **다른** **랙** **(rack)**에 있는 데이터 노드에 기록됨 **(스위치의 오류 허용)**

> 오류가 발생할 시에 가장 가까운 Node에서 Data를 찾게 된다. [성능 이슈]

### HDFS Node Type
**Name node – 클러스터 당 단일 인스턴스** 

- 클러스터에서 파일 시스템 메타데이터 운영, 복제, 각 파일 블록의 위치등을 담당

**Data Node – 클러스터의 각 노드에 하나의 인스턴스**

- 파일 블록 저장소, 해당 파일 데이터를 클라이언트로 서비스

###### Backup Node / Checkpoint Node 

**Name Node의 백업 담당**

### HDFS Architecture
![image](https://user-images.githubusercontent.com/46669551/54653024-72d0a080-4afc-11e9-8f5f-d2171d1280ce.png)

- heartbeat: 데이터노드는 네임노드에게 하트비트를 3초마다 보낸다. 하드비트에 디스크 가용 공간정보, 데이터 이동, 적재량 등의 정보가 들어 있다. 

- balancing: 분산기능

- replication: 복제기능



### HDFS에 파일 밀어넣기

![image](https://user-images.githubusercontent.com/46669551/54653049-9267c900-4afc-11e9-9bf8-233482573411.png)

> 쪼개진 데이터를 균일하게 분산시켜 준다.

### HDFS에서 파일 가져오기

![image](https://user-images.githubusercontent.com/46669551/54653098-bdeab380-4afc-11e9-9faa-07fd6677191b.png)



### Failures, Failures, Failures

HDFS는 **소프트웨어나 하드웨어 모두**에서 실패가 자주 발생하는 상황을 고려하여 설계됨

**실패 유형:**

- 디스크 오류와 실패

- 데이터 노드 오류와 실패

- 스위치/랙 실패

- 네임노드 실패

- 데이터센터 실패

![image](https://user-images.githubusercontent.com/46669551/54653164-0904c680-4afd-11e9-91ea-4d037feaf0ff.png)



### 내 결함성 (데이터 노드 실패)
![image](https://user-images.githubusercontent.com/46669551/54653199-246fd180-4afd-11e9-9bb5-91f808159c88.png)

> heartbeat에 의하여 존재여부를 확인하고, 장애가 생겼을 시 re-balancing을 진행한다.

### 내 결함성 (NameNode 실패)

![image](https://user-images.githubusercontent.com/46669551/54653223-3d788280-4afd-11e9-9bcb-66ecc1e2026f.png)



### 라이브 수평 확장 및 재 조정(rebalancing)
![image](https://user-images.githubusercontent.com/46669551/54653247-584af700-4afd-11e9-899a-e0d43185e228.png)

> 자동적으로 진행이 되는 과정이 아님. 수동으로 명령어를 통하여 진행된다. 



### MapReduce

##### 데이터가 있는 곳에서 프로세스를 처리

- 분산 프로세싱: 단일 파이프를 통한 직렬화된 프로세싱 대신 로컬 컴퓨팅을 데이터가 있는 곳으로 분산

- 오직 결과 데이터만 가져옴

- 노드를 추가하여 선형 확장

##### 세 단계의 실행

- Map: 개발자는 데이터에 Map 함수를 작성

- Shuffle / Distributes: 프레임워크가 자동으로 정리(네트워크, 동기화, 복구, 스케줄링)

- Reduce: 개발자가 Reduce 함수를 작성하여 결과 데이터를 반환

![image](https://user-images.githubusercontent.com/46669551/54653854-79144c00-4aff-11e9-97a6-7880728bc6c5.png)

![image](https://user-images.githubusercontent.com/46669551/54654701-63ecec80-4b02-11e9-97f4-320183150620.png)

#### Map Reduce function in JavaScript

```javascript
var map = function (key, value, context) {
var words = value.split(/[^a-zA-Z]/);
for (var i = 0; i < words.length; i++) {
if (words[i] !== "")
context.write(words[i].toLowerCase(),
1);}
}};
var reduce = function (key, values, context) {
var sum = 0;
while (values.hasNext()) {
sum += parseInt(values.next());
}
context.write(key, sum);
};
```



### Hadoop MapReduce

- **HDFS에 저장되어 있는 데이터 집합을 분석하기 위한 프로그래밍 프레임워크 (라이브러리와 런타임)**

- **사용자가 제공하는 Map과 Reduce 기능** 

  - Map() – subdivide and conquer (분할과 정복): <u>조각을 내는 작업</u>

  - Reduce() – 결합하고 카디널리티를 감소시킴

![image](https://user-images.githubusercontent.com/46669551/54653914-c42e5f00-4aff-11e9-8947-ea3220c44443.png)

### MapReduce 프로그래밍 패러다임
**MapReduce API:** 원하는 MapReduce 응용 프로그램을 프로그래밍하는데 사용

**MapReduce framework:** map 단계, 정렬/셔플/병합 등과 같은 MapReduce의 다양한 단계의 구현

**MapReduce system:** 사용자의 MapReduce 응용 프로그램, 클러스터 리소스 관리, 수천개의 현재 작업 스케줄링등을 수행하는 백엔드 인프라

![image](https://user-images.githubusercontent.com/46669551/54653963-ec1dc280-4aff-11e9-9135-321115230703.png)



### Map

![image](https://user-images.githubusercontent.com/46669551/54654028-271ff600-4b00-11e9-8c52-6e6866e09e01.png)



### Reduce

![image](https://user-images.githubusercontent.com/46669551/54654064-49b20f00-4b00-11e9-9c3e-2b00c317fffe.png)



### Hadoop Architecture
![image](https://user-images.githubusercontent.com/46669551/54654096-62bac000-4b00-11e9-932d-21bb628e235d.png)

### Pre-execution: 태스크 제출
•클라이언트 앱이 작업을 생성

•작업은 작업 관리자에서 스케줄링 됨

•작업은 예약된 시간에 전달 됨

![image](https://user-images.githubusercontent.com/46669551/54654259-e96f9d00-4b00-11e9-8195-c391c7eacf7c.png)



### Execution: Map
•작업은 모든 멤버 노드로 분산됨

•각각의 멤버 노드는 Mapper가 됨

![image](https://user-images.githubusercontent.com/46669551/54654341-3784a080-4b01-11e9-8fdc-60d806ec378b.png)

### Execution: Shuffle and reduce
•Mapper 함수는 파티션의 모든 행을 통해 실행

•Mapper는 결과를 Reducer로 Push

•Reducer는 Mapper로 출력 처리를 시작

![1553048259913](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1553048259913.png)

### Post-processing: Data Summary
•Reducer는 병렬로 각자의 동작을 수행

•각 Reducer의 출력은 단일 임시 테이블로 합계됨

•출력 결과는 출력 파일로 게시됨

![image](https://user-images.githubusercontent.com/46669551/54654461-977b4700-4b01-11e9-9689-a02aed5a182f.png)



### Wordcount가 끝난 다음에는?

- 역 색인

- 분산 데이터 정리

- 데이터 변환

- 머신 러닝 알고리즘

- 전통적인 분석

- 예측 분석

### YARN

**클러스터** 리소스 관리

- **모든 프로젝트에 접근할 수 있는** MapReduce에서 배타적으로 사용할 수 있는 모든 클러스터 리소스 관리를 분리

- Hadoop을 **넓은 범위에서 모든 엔진을 위한 일반적인 플랫폼으로 만듦**

- Hadoop에서 **동일한 일반적인 리소스 관리로 여러 응용프로그램을 실행**

![image](https://user-images.githubusercontent.com/46669551/54652631-d4900b00-4afa-11e9-91fe-1387d4ff0ce0.png)

### Hadoop 1.x vs. Hadoop 2.0
- Hadoop 1.x : (Single Use System: 일괄처리 앱)

- Hadoop 2.0 : (Multi Purpose Platform: 일괄처리, 상호작용,  온라인, 스트리밍)

![image](https://user-images.githubusercontent.com/46669551/54655165-3143f380-4b04-11e9-9781-09f211a2c6dc.png)



### YARN이 만들어진 동기

Map/Reduce가 사용할 수 있는 유일한 계층인가?

실시간 분석을 원한다면?

각 작업으로 메시지를 전달할 수 있는지?

유휴 자원을 활용할 수 있는지?

![image](https://user-images.githubusercontent.com/46669551/54655185-47ea4a80-4b04-11e9-9cf4-c096a8187388.png)

> YARN 에서 Resource Manager 가 추가되어 있음을 확인 가능하다. (Node Manager를 관리해주는 역할)
>
> YARN 에서 Resource Manager가 없으면 Map Task를 진행 할 수 없다.

### YARN - Concept(컨셉)
##### Resource Manager – 전역 리소스 스케줄러

- 시스템 내의 경쟁중인 응용 프로그램들에서 리소스를 중재하는데 한정됨

##### Node Manager – 노드당 하나

- 응용 프로그램의 컨테이너를 실행하고 관리하는 책임

- 리소스 사용(CPU, 메모리, 디스크, 네트워크)을 모니터링

- 사용량 관련 정보를 Resource Manager에게 알림

#####  Application Master – 응용 프로그램당 하나

- Resource Manager로 부터 자원을 협상하는 작업

- Node Manager와 함께 동작

- Container의 자원 소모와 상태를 모니터링

##### Container – 추상화 개념

- Container는 **할당된 자원**을 의미(메모리, CPU, 디스크, 네트워크 등)

- Resource Manager가 승인한 특정 자원 요청의 성공적인 결과

### 유연한 데이터 프로세싱 옵션

![image](https://user-images.githubusercontent.com/46669551/54655336-f1c9d700-4b04-11e9-9b2a-0e0456346001.png)

[그림]하둡 에코시스템 2.0

- Java MapReduce

- Streaming MapReduce

- Pig Latin

- Hive

- Oozie

- Sqoop

`<http://hadoopecosystemable.github.io/> `



**코디네이터**

\- Zookeeper([http://zookeeper.apache.org](http://zookeeper.apache.org/))
분산 환경에서 서버 간의 상호 조정이 필요한 다양한 서비스를 제공하는 시스템으로, 크게 다음과 같은 네 가지 역할을 수행합니다. 첫째, 하나의 서버에만 서비스가 집중되지 않게 서비스를 알맞게 분산해 동시에 처리하게 해줍니다. 둘째, 하나의 서버에서 처리한 결과를 다른 서버와도 동기화해서 데이터의 안정성을 보장합니다. 셋째, 운영(active) 서버에 문제가 발생해서 서비스를 제공할 수 없을 경우, 다른 대기 중인 서버를 운영 서버로 바꿔서 서비스가 중지 없이 제공되게 합니다. 넷째, 분산 환경을 구성하는 서버의 환경설정을 통합적으로 관리합니다.



**리소스 관리**

- YARN([http://hadoop.apache.org](http://hadoop.apache.org/))

  얀(YARN)은 데이터 처리 작업을 실행하기 위한 클러스터 자원(CPU, 메모리, 디스크등)과 스케쥴링을 위한 프레임워크입니다. 기존 하둡의 데이터 처리 프레임워크인 맵리듀스의 단점을 극복하기 위해서 시작된 프로젝트이며, 하둡2.0부터 이용이 가능합니다. 맵리듀스, 하이브, 임팔라, 타조, 스파크 등 다양한 애플리케이션들은 얀에서 리소스를 할당받아서, 작업을 실행하게 됩니다. 얀에 대한 자세한 설명은 <http://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/index.html>을 참고하시기 바랍니다.



- Mesos([http://mesos.apache.org](http://mesos.apache.org/))

  메소스(Mesos)는 클라우드 인프라스트럭처 및 컴퓨팅 엔진의 다양한 자원(CPU, 메모리, 디스크)을 통합적으로 관리할 수 있도록 만든 자원 관리 프로젝트입니다. 메소스는 2009년 버클리 대학에서 Nexus 라는 이름으로 시작된 프로젝트이며, 2011년 메소스라는 이름으로 변경됐으며, 현재는 아파치 탑레벨 프로젝트로 진행중이며, 페이스북, 에어비엔비, 트위터, 이베이 등 다양한 글로벌 기업들이 메소스로 클러스터 자원을 관리하고 있습니다. 메소스는 클러스터링 환경에서 동적으로 자원을 할당하고 격리해주는 매커니즘을 제공하며, 이를 통해 분산 환경에서 작업 실행을 최적화시킬 수 있습니다. 1만대 이상의 노드에도 대응이 가능하며, 웹 기반의 UI, 자바, C++, 파이썬 API를 제공합니다. 하둡, 스파크(Spark), 스톰(Storm), 엘라스틱 서치(Elastic Search), 카산드라(Cassandra), 젠킨스(Jenkins) 등 다양한 애플리케이션을 메소스에서 실행할 수 있습니다.



**데이터 저장**

- HBase([http://hbase.apache.org](http://hbase.apache.org/))

  H베이스(HBase)는 HDFS 기반의 칼럼 기반 데이터베이스입니다. 구글의 빅테이블(BigTable) 논문을 기반으로 개발됐습니다. 실시간 랜덤 조회 및 업데이트가 가능하며, 각 프로세스는 개인의 데이터를 비동기적으로 업데이트할 수 있습니다. 단, 맵리듀스는 일괄 처리 방식으로 수행됩니다. 트위터, 야후!, 어도비 같은 해외 업체에서 사용하고 있으며, 국내에서는 2012년 네이버가 모바일 메신저인 라인에 HBase를 적용한 시스템 아키텍처를 발표했습니다.



- Kudu([http://getkudu.io](http://getkudu.io/))

  쿠두(Kudu)는 컬럼 기반의 스토리지로서, 특정 컬럼에 대한 데이터 읽기를 고속화할 수 있습니다. 물론 기존에도 HDFS에서도 파케이(Parquet), RC, ORC와 같은 파일 포맷을 사용하면 컬럼 기반으로 데이터를 저장할 수 있지만, HDFS 자체가 온라인 데이터 처리에 적합하지 않다는 약점이 있었습니다. 그리고 HDFS 기반으로 온라인 처리가 가능한 H베이스의 경우, 데이터 분석 처리가 느리다는 단점이 있었습니다. 쿠두는 이러한 문제점들을 보완하여 개발한 컬럼 기반 스토리지이며, 데이터의 발생부터 분석까지의 시간을 단축시킬 수 있습니다. 클라우데라에서 시작된 프로젝트이며, 2015년말 아파치 재단의 인큐베이션 프로젝트로 선정됐습니다.



**데이터 수집**

- Chukwa([http://chukwa.apache.org](http://chukwa.apache.org/))

  척와(Chuckwa)는 분산 환경에서 생성되는 데이터를 HDFS에 안정적으로 저장하는 플랫폼입니다. 분산된 각 서버에서 에이전트(agent)를 실행하고, 콜렉터(collector)가 에이전트로부터 데이터를 받아 HDFS에 저장합니다. 콜렉터는 100개의 에이전트당 하나씩 구동되며, 데이터 중복 제거 등의 작업은 맵리듀스로 처리합니다. 야후!에서 개발했으며, 아파치 오픈소스 프로젝트로 공개돼 있습니다.



- Flume([http://flume.apache.org](http://flume.apache.org/))

  플럼(Flume)은 척와처럼 분산된 서버에 에이전트가 설치되고, 에이전트로부터 데이터를 전달받는 콜랙터로 구성됩니다. 차이점은 전체 데이터의 흐름을 관리하는 마스터 서버가 있어서 데이터를 어디서 수집하고, 어떤 방식으로 전송하고, 어디에 저장할지를 동적으로 변경할 수 있습니다. 클라우데라에서 개발했으며, 아파치 오픈소스 프로젝트로 공개돼 있습니다.



- Scribe(<https://github.com/facebook/scribe>)

  페이스북에서 개발한 데이터 수집 플랫폼이며, Chukwa와는 다르게 데이터를 중앙 집중 서버로 전송하는 방식입니다. 최종 데이터는 HDFS 외에 다양한 저장소를 활용할 수 있으며, 설치와 구성이 쉽게 다양한 프로그램 언어를 지원합니다. HDFS에 저장하려면 JNI(Java Native Interface)를 이용해야 합니다.



- Sqoop([http://sqoop.apache.org](http://sqoop.apache.org/))

  스쿱(Sqoop)은 대용량 데이터 전송 솔루션이며, 2012년 4월에 아파치의 최상위 프로젝트로 승격됐습니다. Sqoop은 HDFS, RDBMS, DW, NoSQL 등 다양한 저장소에 대용량 데이터를 신속하게 전송하는 방법을 제공합니다. 오라클, MS-SQL, DB2 등과 같은 상용 RDBMS와 MySQL, 포스트그레스큐엘(PostgreSQL)과 같은 오픈소스 RDBMS 등을 지원합니다.



- Hiho(<https://github.com/sonalgoyal/hiho>)

  스쿱과 같은 대용량 데이터 전송 솔루션이며, 현재 깃헙(GitHub)에 공개돼 있습니다. 하둡에서 데이터를 가져오기 위한 SQL을 지정할 수 있으며, JDBC 인터페이스를 지원합니다. 현재는 오라클과 MySQL의 데이터 전송만 지원합니다.



- Kafka([http://kafka.apache.org](http://kafka.apache.org/))

  카프카(Kafka)는 데이터 스트림을 실시간으로 관리하기 위한 분산 메세징 시스템입니다. 2011년 링크드인에서 자사의 대용량 이벤트처리를 위해 개발됐으며, 2012년 아파치 탑레벨 프로젝트가 됐습니다. 발행(publish)-구독(subscribe) 모델로 구성되어 있으며, 데이터 손실을 막기 위하여 디스크에 데이터를 저장합니다. 파티셔닝을 지원하기 때문에 다수의 카프카 서버에서 메세지를 분산 처리할 수 있으며, 시스템 안정성을 위하여 로드밸런싱과 내고장성(Fault Tolerant)를 보장합니다. 다수의 글로벌 기업들이 카프카를 사용하고 있으며, 그중 링크드인은 하루에 1조1천억건 이상의 메세지를 카프카에서 처리하고 있습니다.



**데이터 처리**

- Pig([http://pig.apache.org](http://pig.apache.org/))

  피그(Pig)는 야후에서 개발됐으나 현재는 아파치 프로젝트에 속한 프로젝트로서, 복잡한 맵리듀스 프로그래밍을 대체할 피그 라틴(Pig Latin)이라는 자체 언어를 제공합니다. 맵리듀스 API를 매우 단순화한 형태이고 SQL과 유사한 형태로 설계됐습니다. SQL과 유사하기만 할 뿐, 기존 SQL 지식을 활용하기가 어려운 편입니다.



- Mahout([http://mahout.apache.org](http://mahout.apache.org/))

  머하웃(Mahout)은 하둡 기반으로 데이터 마이닝 알고리즘을 구현한 오픈소스 프로젝트입니다. 현재 분류(classification), 클러스터링(clustering), 추천 및 협업 필터링(Recommenders/collaborative filtering), 패턴 마이닝(Pattern Mining), 회귀 분석(Regression), 차원 리덕션(Dimension reduction), 진화 알고리즘(Evolutionary Algorithms) 등 중요 알고리즘을 지원합니다. Mahout을 그대로 사용할 수도 있지만 각 비즈니스 환경에 맞게 최적화해서 사용하는 경우가 많습니다.



- Spark([http://spark.apache.org](http://spark.apache.org/))

  스파크(Spark)는 인메모리 기반의 범용 데이터 처리 플랫폼입니다. 배치 처리, 머신러닝, SQL 질의 처리, 스트리밍 데이터 처리, 그래프 라이브러리 처리와 같은 다양한 작업을 수용할 수 있도록 설계되어 있습니다. 2009년 버클리 대학의 AMPLab에서 시작됐으며,   2013년 아파치 재단의 인큐베이션 프로젝트로 채택된 후, 2014년에 탑레벨 프로젝트로 승격됐습니다. 현재 가장 빠르게 성장하고 있는 오픈소스 프로젝트 중의 하나이며, 사용자와 공헌자가 급격하게 증가하고 있습니다.



- Impala([http://impala.io](http://impala.io/))

  임팔라(Impala)는 클라우데라에서 개발한 하둡 기반의 분산 쿼리 엔진입니다. 맵리듀스를 사용하지 않고, C++로 개발한 인메모리 엔진을 사용해 빠른 성능을 보여줍니다. 임팔라는 데이터 조회를 위한 인터페이스로 HiveQL을 사용하며, 수초 내에 SQL 질의 결과를 확인할 수 있습니다. 2015년말 아파치 재단의 인큐베이션 프로젝트로 채택됐습니다.



- Presto([https://prestodb.io](https://prestodb.io/))

  프레스토(Presto)는 페이스북이 개발한 대화형 질의를 처리하기 위한 분산 쿼리 엔진입니다. 메모리 기반으로 데이터를 처리하며, 다양한 데이터 저장소에 저장된 데이터를 SQL로 처리할 수 있습니다. 특정 질의 경우 하이브 대비 10배 정도 빠른 성능을 보여주며, 현재 오픈소스로 개발이 진행되고 있습니다.



- Hive([http://hive.apache.org](http://hive.apache.org/))

  하이브(Hive)는 하둡 기반의 데이터웨어하우징용 솔루션입니다. 페이스북에서 개발했으며, 오픈소스로 공개되며 주목받은 기술입니다. SQL과 매우 유사한 HiveQL이라는 쿼리 언어를 제공합니다. 그래서 자바를 모르는 데이터 분석가들도 쉽게 하둡 데이터를 분석할 수 있게 도와줍니다. HiveQL은 내부적으로 맵리듀스 잡으로 변환되어 실행됩니다.



- Tajo([http://tajo.apache.org](http://tajo.apache.org/))

  타조(Tajo)는 고려대학교 박사 과정 학생들이 주도해서 개발한 하둡 기반의 데이터 웨어하우스 시스템입니다. 2013년 아파치 재단의 인큐베이션 프로젝트로 선정됐으며, 2014년 4월 최상위 프로젝트로 승격됐습니다. 맵리듀스 엔진이 아닌 자체 분산 처리 엔진을 사용하며, HiveQL을 사용하는 다른 시스템들과는 다르게 표준 SQL을 지원하는 것이 특징입니다. HDFS, AWS S3, H베이스, DBMS 등에 저장된 데이터 표준 SQL로 조회할 수 있고, 이기종 저장소간의 데이터 조인 처리도 가능합니다. 질의 유형에 따라서 하이브나 스파크보다 1.5 ~ 10배 빠른 성능을 보여줍니다.



**워크플로우 관리**

- Oozie([http://oozie.apache.org](http://oozie.apache.org/))

  우지(Oozie)는 하둡 작업을 관리하는 워크플로우 및 코디네이터 시스템입니다. 자바 서블릿 컨테이너에서 실행되는 자바 웹 애플리케이션 서버이며, 맵리듀스 작업이나 피그 작업 같은 특화된 액션으로 구성된 워크플로우를 제어합니다. 



- Airflow(<http://nerds.airbnb.com/airflow>)

  에어플로우(Airflow)는 에어비앤비가 개발한 워크플로우 플랫폼입니다. 데이터 흐름의 시각화, 스케쥴링, 모니터링이 가능하며, 하이브, 프레스토, DBMS 엔진과 결합하여 사용할 수 있습니다.



- Azkaban([https://azkaban.github.io](https://azkaban.github.io/))

  아즈카반(Azkaban)은 링크드인에서 개발한 워크플로우 관리도구입니다. 링크드인은 자사의 복잡한 데이터 파이프라인을 관리하기 위해서 아즈카반을 개발했으며, 이를 오픈소스로 공개했습니다. 아즈카반은 워크플로우 스케쥴러, 시각화된 절차, 인증 및 권한 관리, 작업 모니터링 및 알람 등 다양한 기능은 웹UI로 제공합니다. 



- Nifi([https://nifi.apache.org](https://nifi.apache.org/))

  나이파이(Niagarafiles, Nifi)는 데이터 흐름을 모니터링하기 위한 프레임워크입니다. 여러 네트워크를 통과하는 데이터 흐름을 웹UI에서 그래프로 표현하며, 프로토콜과 데이터 형식이 다르더라도 분석이 가능합니다. 또한 데이터를 흘러 보낼때 우선 순위를 제어할 수 있습니다. 나이파이는 원래는 미국 국가안보국(NSA)에서 개발된 기술로, NSA의 기술 이전 프로그램인 TTP를 통해서 처음 외부에 공개된 오픈소스 기술입니다.



**데이터 시각화**

- Zeppelin ([https://zeppelin.incubator.apache.org](https://zeppelin.incubator.apache.org/))

  제플린(Zeppelin)은 빅데이터 분석가를 위한 웹 기반의 분석 도구이며, 분석결과를 즉시 표, 그래프로 제공하는 시각화까지 지원합니다. 아이파이썬(iPython)의 노트북(Notebook)과 유사한 노트북 기능을 제공하며, 분석가는 이를 통해 손쉽게 데이터를 추출, 정제, 분석, 공유를 할 수 있습니다. 또한 스파크, 하이브, 타조, 플링크(Flink), 엘라스틱 서치, 카산드라, DBMS 등 다양한 분석 플랫폼과 연동이 가능합니다.  2013년 엔에프랩의 내부 프로젝트로 시작됐으며, 2014년 아파치 재단의 인큐베이션 프로젝트로 선정됐습니다. 



**데이터 직렬화**

- Avro([http://avro.apache.org](http://avro.apache.org/))

  RPC(Remote Procedure Call)와 데이터 직렬화를 지원하는 프레임워크입니다. JSON을 이용해 데이터 형식과 프로토콜을 정의하며, 작고 빠른 바이너리 포맷으로 데이터를 직렬화합니다. 경쟁 솔루션으로는 아파치 쓰리프트(Thrift), 구글 프로토콜 버퍼(Protocol Buffer) 등이 있습니다.



- Thrift([http://thrift.apache.org](http://thrift.apache.org/))

  쓰리프트(Thrift)는 서로 다른 언어로 개발된 모듈들의 통합을 지원하는 RPC 프레임워크입니다. 예를 들어 서비스 모듈은 자바로 개발하고, 서버 모듈은 C++로 개발되었을때, 쓰리프트로 쉽게 두 모듈의 통신 코드를 생성할 수 있습니다.  쓰리프트는 개발자가 데이터 타입과 서비스 인터페이스를 선언하면, RPC 형태의 클라이언트와 서버 코드를 자동으로 생성합니다. 자바, C++, C#, Perl, PHP, 파이썬, 델파이, Erlang, Go, Node.js 등과 같이 다양한 언어를 지원합니다.  



### Overview of the Hadoop programming framework

- Hadoop Distributed File System (HDFS)

- MapReduce

- Yet Another Resource Negotiator (YARN) [2.0 이상의 버젼에서 도입]



### Working with Hadoop Distributed File System

저장소가 여러 대가 존재하고 소프트웨어적으로 하나의 파일시스템으로 묶어 사용

- The storage system of Hadoop
- Unit of storage is a block
- Files are split into multiple blocks and stored on worker nodes across the cluster
- HDFS hides these complexities from the user



### Identifying the MapReduce process

처리를 따로하여 성능향상하는 프로세스.

- The data processing engine of Hadoop clusters



### Managing resources with YARN

하둡 2.0부터 제공되는 리소스 관리 플랫폼.

각 어플리케이션(HBase, Accumulo, Storm, MapReduce...)에 필요한 리소스(CPU, 메모리, 디스크 등)를 할당하고 모니터링하는 업무 수행.

**MapReduce의 단점**을 극복하기 위해서 시작된 프로젝트

- Resource management system with two main components

  - Application master
  - Resource manager

  



## Lesson 3: Working with MapReduce functions on HDInsight



### Defining Map and Reduce functions

- The Data Processing Framework



Team, Name Event Score

Bellows College, Tracy Friday, High Jump, 6’

Fabrikam United, Alberto Tibbetts, 110 Hurdles, 13.91s

Bellows College, Wilfredo Boatwright, 100 dash, 12.03s

Bellows College, Leo Pitman, 100 Hurdles, 14.21 s

![image](https://user-images.githubusercontent.com/46669509/54651268-70b71380-4af5-11e9-85c3-fe7dcb310a46.png)



### Working with higher level abstractions

- Hive
- Pig



## Lesson 4: Introducing HDInsight



### Introducing Azure HDInsight

- HDInsight
- WABS
- ADLS
- Decoupling Storage from Compute



### Identifying HDInsight variations

- HDInsight
  - Hbase
  - Interactive Hive
  - Spark
  - Storm
  - R Server
  - Apache Kafka



## Lab: Working with HDInsight

### Exercise 1: Provision an HDInsight cluster and run MapReduce jobs