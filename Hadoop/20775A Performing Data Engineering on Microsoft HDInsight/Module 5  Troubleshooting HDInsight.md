# Module 5 : Troubleshooting HDInsight

## Log를 보고 장애를 처리함을 목적으로 하는 챕터이다.

### Module Overview

- **Analyze HDInsight logs**
- **YARN logs**
- **Heap dumps**
- **Operations Management Suite**

## Lesson 1: Analyze HDInsight logs

- Logs written to Azure tables
- Logs written to Blob storage
- Tools for accessing logs
- Demonstration: Analyze HDInsight logs

### Logs written to Azure tables

- Logs written to Azure Tables

  - Linux-based clusters

    - hdinsight-agent log

    - syslog

    - ambari-server.log

    - ambari-agent.log

    - daemon logs

- Windows-based clusters
  - setuplog
  - hadoopinstalllog
  - hadoopservicelog

### Logs written to Blob storage

- Logs written to Blob storage

  - Task-level logs

  - YARN logs

  - These logs provide more granular information to help find the root cause of an issue

### Tools for accessing logs

- Tools for accessing log files

  - Azure Storage Explorer

  - Power Query for Excel

  - Visual Studio

## Demonstration: Analyze HDInsight logs

In this demonstration, you will see how to:

- Open an SSH connection to the head node

- Download log files

- Use Power Query to view HDInsight log files

## Lesson 2: YARN logs (Resource Manager / Node manager)

- YARN Timeline Server
- YARN application and logs
- YARN tools
- Demonstration: Viewing YARN logs

### YARN Timeline Server

- **Key components of YARN**:
  - Resource Manager
  - Node Manager
  - Application Master
  - Container

- YARN Timeline Server—generic information:
  - Application ID
  - User information
  - Number of attempts needed
  - Number of containers used

### YARN application and logs

- Application attempts

- Log aggregation

- Location:
  - /app-logs/<user>/logs/<applicationId>   <- 이곳에 Log가 남게 된다.

### YARN tools

- Command-line interface (CLI)  20775A(5-28)
- Resource Manager user interface (UI)

## Demonstration: Viewing YARN logs

In this demonstration, you will see how to:

- View YARN logs using the YARN CLI
- View YARN logs using the Resource Manager UI

## Lesson 3: Heap dumps 

[Hadoop 은 JAVA로 만들어져 있기 때문에 용량이 모자랄 수 있음 따라서 Heat dump를 이용해서 메모리 상태를 확인할 수 있도록 지원해준다.]

- Overview of heap dumps
- Heap dump configuration
- Enable heap dumps
- Demonstration: Enable heap dumps

### Overview of heap dumps

Heap dumps:

- Contain state of application memory

- Include values of variables at dump creation time

- Can be enabled for:

  - **Hcatalog**

  - **Hive**

  - **Map-reduce**

  - **YARN**

  - **HDFS**

     

### Heap dump configuration

- Heap dump configuration:
  - Uses JVM options 
  - MapReduce requires two sets of options

```
*_OPTS
```

```
mapred_site.xml
```



### Enable heap dumps

- Enable heap dumps
- Dump path
- Trigger scripts

구동을 하다가 메모리 상태의 오류가 생기면 Dump파일이 생성 되는데 그 파일에 접속하여 디버깅을 진행한다. 

## Demonstration: Enable heap dumps

In this demonstration, you will see how to:

- Enable heap dumps for HDFS

```
HADOOP_NAMENODE_OPTS="-server
```

```
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/
```

##  Lesson 4: Operations Management Suite

Targeting해 놓은 부분의 장애가 생기면 Alart 또는 Log를 통해 해결할 수 있도록 해준다.

- Overview of OMS
- Monitoring resources
- Alert management
- Runbooks
- Demonstration: Monitor resources with OMS

### Overview of OMS

- Group of Azure services to manage cloud and on-premises resources

- OMS core services:
  - Log Analytics
  - Azure Automation
  - Azure Backup
  - Azure Site Recovery

### Monitoring resources

**Log** **Analytics**:

- **Extract** **data**
- **Query** **data**
- **Analyze** **data**
- **Data** **sources**:
  - Custom logs
  - Window Event logs
  - Windows Performance counters
  - Linux Performance counters
  - IIS logs
  - Syslog

### Alert management

- Create alerts using alert rules
- Alert actions
  - Notify using email
  - Start a webhook
  - Trigger a runbook

### Runbooks

- Automating processes
- Resource access considerations
- Launching a runbook
- Managing configurations

## Demonstration: Monitor resources with OMS

In this demonstration, you will see how to:

- Create an OMS workspace
- Add solutions to an OMS workspace
- Connect a VM to OMS Log Analytics
- View and analyze OMS data

### Lab: Troubleshooting HDInsight

- Exercise 1: Analyze HDInsight logs
- Exercise 2: Analyze YARN log files
- Exercise 3: Monitor Azure resources using the OMS