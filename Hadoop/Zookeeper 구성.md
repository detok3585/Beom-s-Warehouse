# Zookeeper 구성

HA -> Zookeeper

node 1, node 2, node 3, node 4
zk 1, zk 2, zk 3
jn 1, jn 2, jn 3
name 1, name 2
                data 3, data 4

myhd1234432111-ssh.azurehdinsight.net:50070
apache zookeepr (분산 코디네이션 서비스)

server 2

a = 1(server1), a = 1(server11), a = 1(server111)

server 3



## Zookeeper  설치

```
$ wget http://apache.tt.co.kr/zookeeper/stable/zookeeper-3.4.13.tar.gz
$ tar xvfz zookeeper-3.4.13.tar.gz
```

wget으로 Zookeeper를 다운로드 받아와 압축해제한다.



## Zookeeper 설정(server10)

```
$ cd zookeeper-3.4.13
$ cd conf
$ cp zoo_sample.cfg zoo.cfg
$ vi zoo.cfg
```

Zookeeper - conf 폴더에 들어가 설정 파일을 샘플을 통해 생성하고 수정한다. 

dataDir=/home/hadoop/zookeeper-3.4.13/data - 변경

하단에 아래 문구를 추가해준다.

server.1=server10:2888:3888
server.2=server11:2888:3888
server.3=server12:2888:3888



```
$ mkdir ~/zookeeper-3.4.13/data
```

이후 data폴더를 생성하여 준다.





## Server 11,12 복사

```
$ scp -r ~/zookeeper-3.4.13/* server11:/home/hadoop/zookeeper-3.4.13/
$ scp -r ~/zookeeper-3.4.13/* server12:/home/hadoop/zookeeper-3.4.13/
```

scp 명령어를 이용해 설정한 Zookeeper를 server11,server12에 복사해준다.



## myid 설정

해당 서버를 식별하기 위해 각 서버마다 각자의 myid를 가져야한다.  myid의 경로와 값은 다음과 같다.

* Server10
  /home/hadoop/zookeeper-3.4.13/myid
  1
* Server11
  /home/hadoop/zookeeper-3.4.13/myid
  2
* Server12
  /home/hadoop/zookeeper-3.4.13/myid
  3



아래의 명령어를 입력하여 myid를 설정해준다.

```
$ cd ~/zookeeper-3.4.13
$ echo 1 > myid (Server10)
$ echo 2 > myid (Server11)
$ echo 3 > myid (Server12)
```






## Zookeeper 사용법

### 기본 동작 명령어

```
$ cd .. (Zookeeper 기본폴더로 이동)

$ bin/zkServer.sh start(시작)
$ bin/zkServer.sh status(상태확인)
$ bin/zkServer.sh stop(중지)
```



### 클라이언트 조작

#### 생성명령어

```
$ bin/zkCli.sh (클라이언트접속)
[zk: localhost:2181(CONNECTED) 0] create /myzone "HelloWorld"
```

#### 출력값

```
Created /myzone 
```



#### 출력명령어

```
[zk: localhost:2181(CONNECTED) 1] get /myzone "HelloWorld"
```

#### 출력값

```
HelloWorld
cZxid = 0x100000004
ctime = Thu Mar 21 17:48:06 KST 2019
mZxid = 0x100000004
mtime = Thu Mar 21 17:48:06 KST 2019
pZxid = 0x100000004
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 10
numChildren = 0
```

