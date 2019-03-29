# Module 10 : Stream Analytics

## Module Overview

- Stream Analytics
- Process streaming data with Stream Analytics
- Managing Stream Analytics jobs

### Big Data Flow Architecture (pic)

![image](https://user-images.githubusercontent.com/46669551/55204997-680cbe80-5214-11e9-80ac-d18e41e8026e.png)

## Lesson 1: Stream Analytics

- What is Stream Analytics?
- Capabilities of Stream Analytics
- Stream Analytics compared to Apache Storm

### What is Stream Analytics?

- Stream processing:
  - Analyze continuous stream of events with rates up to millions of events per second
  - Ability to get and respond to real-time insights 

- Stream Analytics
  - Managed real-time event processing engine
  - Use for declarative query model based on T-SQL variant
  - Support for time-sensitive processing
  - Integration with Azure services for input, output and reference data 
  - Low cost

### Capabilities of Stream Analytics

The key capabilities of Stream Analytics include:

- Fully managed cloud service
- Declarative query model for streaming data
- Support for scalability to achieve high throughput of events
- Integration with Azure services 
- Pay-as-you-use model

### Stream Analytics compared to Apache Storm
- Stream Analytics vs. Apache Storm:
  - Both PaaS on Azure platform
  - Proprietary vs. open-source
  - Input/output data: tight integration with Azure services vs. extended and customizable implementation
  - Pay per streaming job vs. pay for HDInsight cluster time
  - Temporal queries and aggregates: out-of-box support vs. custom implementation

## Lesson 2: Process streaming data with Stream Analytics
- Create an event hub
- Create a Stream Analytics job
- Build a Stream Analytics query
- Demonstration: Process streaming data

### Event Hub Process (pic)

![image](https://user-images.githubusercontent.com/46669551/55203098-920eb280-520d-11e9-9605-3e0a636bf978.png)

### Create an event hub

- Azure Event Hub 

  어떠한 event hub Service 이던지 다섯가지 항목의 요소를 갖게 된다.

  - Namespace
  - Shared access policy
  - Partitions
  - Message retention
  - Archive

- Hyper-scale event processing

  - Can process up to millions of events per second
  - One of the supported input sources for Stream Analytics jobs
  - Created within a unique Service Bus namespace
  - Uses Shared Access Signature token-based policy to authorize event publishers

### Create a Stream Analytics job

- A Stream Analytics job consists of:
  - Inputs – one or many incoming data streams 
  - A query that is used to transform an incoming data stream
  - Outputs – one or many outgoing services where the job execution results are stored or viewed

### Stream Analytics query language:

- Uses a variant of T-SQL language
  - Supports standard data types and type conversion functions
  - Supports known constructs such as SELECT, FROM, WHERE, GROUP BY, HAVING, and others
  - Native support for windowing operators:
    - Tumbling Window
    - Hopping Window
    - Sliding Window

## Demonstration: Process streaming data
In this demonstration, you will see how to:

- Create an event hub
- Create a Stream Analytics job
- Develop a Stream Analytics job query
- Test a Stream Analytics job query

![image](https://user-images.githubusercontent.com/46669551/55204778-9c33af80-5213-11e9-8615-8c8da25ec03b.png)

## Lesson 3: Managing Stream Analytics jobs
- Monitoring jobs in the Portal
- Monitor jobs with logs
- <u>Monitor jobs with PowerShell</u>
- Demonstration: Managing Stream Analytics jobs

### Monitoring jobs in the Portal

- You can monitor a number of metrics for Stream Analytics jobs in the Azure Portal:
  - Streaming Units % Utilization
  - Input events
  - Output events
  - Data conversion errors
  - Runtime errors
  - Input event bytes

### Monitor jobs with logs

- Stream Analytics offers two categories of logs to troubleshoot Stream Analytics jobs:
  - Activity logs
  - Diagnostics logs

- Diagnostics logs distinguish between:
  - Events related to **Authoring** of a Stream Analytics job
  - Events related to **Execution** of a Stream Analytics job

### Monitor jobs with PowerShell

- Azure PowerShell cmdlets can be used with Stream Analytics
- Manage and monitor jobs:
  - Get-AzureRMStreamAnalyticsJob
  - Start-AzureRMStreamAnalyticsJob
  - Get-AzureRMStreamAnalyticsTransformation

- Manage and monitor inputs:
  - Get-AzureRMStreamAnalyticsInput
  - Test-AzureRMStreamAnalyticsInput

- Manage and monitor outputs:
  - Get-AzureRMStreamAnalyticsOutput
  - Test-AzureRMStreamAnalyticsOutput

## Demonstration: Managing Stream Analytics jobs
In this demonstration, you will see how to:

- Manage a Stream Analytics job:
  - In the Azure Portal
  - Using PowerShell scripts

- Monitor job execution using Stream Analytics diagnostic logs

![image](https://user-images.githubusercontent.com/46669551/55205370-f2095700-5215-11e9-9f9e-ac91a0433dad.png)

> 주의사항 : INTO에는 출력의 blob 명을 넣어주고 FROM에는 입력의 Event Hub명을 넣어준다.
>
> 왜냐하면 쿼리는 저장된 데이터를 다시 불러와 데이터를 처리하는 형태이기 때문이다.

![image](https://user-images.githubusercontent.com/46669551/55205520-8ffd2180-5216-11e9-9ad0-d0dd23e507f5.png)



## Lab: Implement Stream Analytics

- Exercise 1: Process streaming data with Stream Analytics
- Exercise 2: Managing Stream Analytics jobs

### Lab Scenario

> You work as a data engineer at Contoso Consulting Services in a division of the company that helps its customers to collect and analyze sensor data, such as temperature and humidity, received from Internet-connected devices installed in offices and residential properties. One of your clients, SmartHomesDevelopers, is planning to deploy such sensors in a newly-built block of flats and wants to understand the capabilities of real-time data collection from the sensors and how this data can be used for real-time data analysis. You decide to build a proof of concept to showcase the capabilities of Stream Analytics and the supporting Azure services.

### Lab Review

> Based on your own organization’s requirements towards streaming and batch data processing, what additional value do you think streaming capabilities can bring?