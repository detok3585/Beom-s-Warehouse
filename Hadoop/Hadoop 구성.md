# Hadoop 구성

hadoop 2.9.2 버전은 2가지형태로 구성할 수 있다.



1. 독립실행모드
   hdfs(x), jar

   				2. 가상분산모드
          hdfs(o) - namenode, datanode, 
          YARN(o) - resoucemanager, nodemanager



가상분산모드 구성을 알아보자. 3개의 서버로 구성한다. 구성은 다음과 같다.

### server10

```
IP : 172.16.0.110 
dns : 168.126.63.1
      8.8.8.8

설치 프로그램 : jdk 1.8, hadoop 2.9.2, protobuffer 2.5
JPS : namenode, resoucemanager
KEYS : public, private key
```

### server11

```
IP : 172.16.0.111 
dns : 168.126.63.1
      8.8.8.8

설치 프로그램 : jdk 1.8, hadoop 2.9.2, protobuffer 2.5
JPS : datanode, nodemanager
KEYS : public key
```

### server12

```
IP : 172.16.0.112 
dns : 168.126.63.1
      8.8.8.8

설치 프로그램 : jdk 1.8, hadoop 2.9.2, protobuffer 2.5
JPS : datanode, nodemanager
KEYS : public key
```





### sudo 설정방법

1. sudo 권한을 부여할 사용자를 추가한다.(username을 원하는 이름으로 변경하기 바란다.)

   ```
   adduser username
   ```

2. username 사용자를 wheel group에 추가한다. CentOS에서는 wheel group의 멤버가 sudo 권한을 가진다.

   ```
   usermod -aG wheel username
   ```

3. sudo visudo 명령어를 실행하여 /etc/sudoers의 일부설정을 변경한다.

   ```
   sudo visudo #/etc/sudoers 파일 열기
   ```

4. 그리고 wheel no password 설정이 주석처리되어 있다면, 주석처리를 해제한다.

   ```
   (수정전)
   ## Same thing without a password
   # %wheel ALL=(ALL) NOPASSWD: ALL
   
   (수정후)
   ## Same thing without a password
   %wheel ALL=(ALL) NOPASSWD: ALL
   ```

   

## IP설정

```
# vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

su 명령어로 관리자로 접속하여 설정한다.



## hostname설정

```
# vi /etc/hostname
```

su 명령어로 관리자로 접속하여 설정한다.



## hostname설정

```
vi /etc/hosts

172.16.0.110	server10
172.16.0.111	server11
172.16.0.112	server12
```

su 명령어로 관리자로 접속하여 설정한다.



## 방화벽 설정 및 네트워크 어댑터 재시작 명령어

```
# systemctl restart network 
		(Server10, Server11, Server12)
# systemctl stop firewalld
		(Server10, Server11, Server12)
# systemctl disable firewalld
		(Server10, Server11, Server12)
# systemctl restart network 
		(Server10, Server11, Server12)
```



## SSH 구성

server 10에서 키를 생성해 server11,12에 공개키를 주어  SSH접속이 가능하게 구성한다.

server10에서

```
$ ssh-keygen -rsa => public, private key 생성
$ cd .ssh
$ cp id_rsa.pub authorized_keys
$ cd ..
$ chmod 755 ~/.ssh/
$ chmod 644 ~/.ssh/authorized_keys
$ ssh server10
$ exit
$ mkdir ~/.ssh (Server11, Server12)
$ cd ~/.ssh (Server10)
$ scp authorized_keys server11:/home/hadoop/.ssh/ (Server10)
$ scp authorized_keys server12:/home/hadoop/.ssh/ (Server10)
```

server11, server12에서 폴더 및 파일 권한 변경.

```
$ chmod 755 ~/.ssh/
$ chmod 644 ~/.ssh/authorized_keys
```



## 관련 프로그램 설치

```
(Server10, Server11, Server12)
$ su
# yum install net-tools wget -y
# yum install jdk-8u201-linux-x64.rpm
# yum groupinstall "Development Tools"
# yum groupinstall "Development Tools" "Development Libraries"
# yum install openssl-devel cmake
# exit
```



## Hadoop 설치

```linux
$ tar xvzf hadoop-2.9.2.tar.gz
$ cd ~/hadoop-2.9.2/etc/hadoop
$ scp -r ~/hadoop-2.9.2 server11:/home/hadoop
$ scp -r ~/hadoop-2.9.2 server12:/home/hadoop

(Server10, Server11, Server12)
$ cd ~/hadoop-2.9.2/etc/hadoop
$ vi slaves
server11
server12

scp slaves server11:/home/hadoop/hadoop-2.9.2/etc/hadoop/
```



## 환경변수 설정

```
$ vi .bash_profile
	(Server10, Server11, Server12)
```

.bashrc or .bash_profile에 밑의 환경 변수를 추가함

```
# Java 환경 변수
export JAVA_HOME=/usr/java/default
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/bin:$JAVA_HOME/lib/tools.jar

# Hadoop 환경 변수
export HADOOP_HOME=/home/hadoop/hadoop-2.9.2
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_COMMON_LIB_NATIVE_DIR"
export HADOOP_PID_DIR=${HADOOP_HOME}/pids
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin:

# Sqoop 환경 변수
export SQOOP_HOME=/home/hadoop/sqoop-1.4.7.bin__hadoop-2.6.0
export PATH=$PATH:$SQOOP_HOME/bin

# Flume 환경 변수
export FLUME_HOME=/home/hadoop/apache-flume-1.9.0-bin
export PATH=$PATH:$FLUME_HOME/bin
export CLASSPATH=$CLASSPATH:$FLUME_HOME/lib/*:.
```

변경 후 적용하거나 재로그인 한다

```
$ source .bash_profile or exit(재로그인)
```



## Hadoop 환경설정

1. hadoop-env.sh 수정

```
$ cd ~/hadoop-2.9.2/etc/hadoop
$ vi hadoop-env.sh
  export JAVA_HOME=/usr/java/default
  export HADOOP_PID_DIR=/home/hadoop/hadoop-2.9.2/pids
```

2. core-site.xml 수정

```
<configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://server10:9000</value>
        </property>
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/home/hadoop/tmp/hadoop-${user.name}</value>
        </property>
</configuration>
```



3. hdfs-site.xml 수정

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>3</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>/home/hadoop/data/dfs/namenode</value>
    </property>
    <property>
        <name>dfs.namenode.checkpoint.dir</name>
        <value>/home/hadoop/data/dfs/namesecondary</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>/home/hadoop/data/dfs/datanode</value>
    </property>
</configuration>
```



4. mapred-site.xml 수정

```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```



5. yarn-site.xml 수정

```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.nodemanager.local-dirs</name>
        <value>/home/hadoop/data/yarn/nm-local-dir</value>
    </property>
    <property>
        <name>yarn.resourcemanager.fs.state-store.uri</name>
        <value>/home/hadoop/data/yarn/system/rmstore</value>
    </property>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>Server10</value>
    </property>
    <property>
        <name>yarn.web-proxy.address</name>
        <value>0.0.0.0:8089</value>
    </property>
</configuration>
```



### protobuf-2.5.0 설치

```
(Server10, Server11, Server12)
$ cd ~/
$ wget https://github.com/protocolbuffers/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
# yum clean all
# cd /home/hadoop
# cp protobuf-2.5.0.tar.gz /usr/local/
# cd /usr/local
# tar xvfz protobuf-2.5.0.tar.gz
# cd protobuf-2.5.0
# ./configure (comfile작업)
# make
# make install
```



### Hadoop 실행

```
$ cd hadoop-2.9.2
$ bin/hdfs namenode -format
$ sbin/start-dfs.sh
$ sbin/start-yarn.sh
```





