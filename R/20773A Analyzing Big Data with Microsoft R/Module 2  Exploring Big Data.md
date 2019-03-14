#  Module 2 : Exploring Big Data



## Lesson 1: Understanding ScaleR data sources
### ScaleR data sources

- ScaleR functions can access many different types of data, including:

  - Fixed and variable length text files (CSV) 

  - SAS 

  - SPSS 

  - Teradata 

  - XDF 

  - SQL Server and other ODBC databases

    OBDC : 데이터베이스 연결 라이브러리

- ScaleR provides data source objects for each type of data

- Not all data sources are available in every compute context

  적용되지 않는 데이터 타입도 존재함. 확장해서 사용.



## Reading and writing data using ScaleR data sources

- You can manually iterate through data:

  - **rxOpen**

  - **rxIsOpen**

  - **rxReadNext**

  - **rxWriteNext**

  - **rxClose**

- **rxReadNext** and **rxWriteNext** read and write “chunks” of data

- A chunk can be a data frame or list, depending on the data source



```R
list.files(rxGetOption("sampleDataDir"))
#샘플데이터 파일들의 리스트를 출력
file.path(rxGetOption("sampleDataDir"), "claims.txt")
#샘플데이터 폴더에 있는 `claims.txt`파일의 경로를 출력
df = rxImport(myfilepath)
#myfilepath의 경로에 있는 파일을 읽어와 df에 저장.
```



rxImport() : 데이터를 불러와 Data.Frame 형태로 변환.

RxTextData() : 데이터를 불러와 TextData로 저장



## Working with SQL Server data sources

- You can connect to SQL Server using the **RxSqlServerData** or **RxOdbcData** data sources

  - The **RxOdbcData** data source is generalized

  - The **RxSqlServerData** data source is optimized for SQL Server

- The **RxSqlServerData** data source references a table or query

- ScaleR provides the following helper functions for use with **RxSqlServerData** data sources
  - **rxSqlServerDropTable**

  - **rxSqlServerTableExists**

  - **rxExecuteSQLDDL**



## ScaleR file systems

- ScaleR provides direct access to files stored:

  - Using the computer’s native file system

  - Using HDFS

- To access native files, set the file system to **RxNativeFileSystem** with the **rxSetFileSystem** function

- To access HDFS files, set the file system to **RxHdfsFileSystem**

- You can only use **RxHdfsFileSystem** in a supported compute context, such as **RxHadoopMR**



## Working with Hadoop data sources

- Hadoop stores data using HDFS

- If you are running in a Hadoop compute context you can use the following helper functions:
  - **rxHadoopCopyFromClient**
  - **rxHadoopCopyFromLocal**
  - **rxHadoopCopy**
  - **rxHadoopMove**
  - **rxHadoopRemove**
  - **rxHadoopRemoveDir**
  - **rxHadoopListFiles**
  - **rxHadoopFileExists**
  - **rxHadoopCommand** 



## Demonstration: Reading data from SQL Server and HDFS

### Reading data stored in SQL Server

```R
# Connect to SQL Server

sqlConnString <- "Driver=SQL Server;Server=70.12.114.149;Database=VideoShop;uid=sa;pwd=Pa$$w0rd!3585;"

connection <- RxSqlServerData(connectionString = sqlConnString,table = "dbo.VS_CUSTOMER", rowsPerRead = 1000)

# Use R functions to examine the data in the Airports table

head(connection)

rxGetVarInfo(connection)

rxSummary(~., connection)

```



### Reading data stored in HDFS 

```R
# Create a Hadoop compute context
context <- RxHadoopMR(sshUsername = "hadoop", 
                      sshHostname = "192.168.137.101")

rxSetComputeContext(context)

# List the contents of the /user/instructor folder in HDFS
# hadoop fs -ls /user/hadoop
# hdfs namenode -format
# hadoop dfs -ls/

rxHadoopCommand("fs -ls /user/hadoop")

# hadoop fs -put intput(local) output(hdfs, path)
# Source => HDFS, SqlServer, Text(CSV....), SAS, SPSS, ....

# Connect directly to HFDS on the Hadoop VM
hdfsConnection <- RxHdfsFileSystem()
rxSetFileSystem(hdfsConnection)

# Create a data source for the CensusWorkers.xdf file
workerInfo <- RxXdfData("/user/hadoop/CensusWorkers.xdf")

# Perform functions that read from the CensusWorkers.xdf file
head(workerInfo)
rxSummary(~., workerInfo)

```





## Lesson 2: Reading and writing XDF data



### The XDF format

- XDF : Extensible Data Format

- Traditionally, R uses data frames

  - These cache data in memory

    caching : 메모리에 적재한다.

    오버헤드가 적다 -> 딜레이 타임이 작다.

    메모리에서 읽기 최적화된 포맷.

  - The amount of available memory can limit the size of datasets that you can easily process

- The XDF format breaks files into chunks

  - Each chunk is loaded into memory, processed, and written back to disk

  - ScaleR functions can process chunks in parallel

  - Sometimes it might be necessary for a chunk to be processed more than once

- XDF improves scalability, but there are tradeoffs



### Importing data into an XDF object

- Use the **rxImport** command to import data held in other formats into an XDF object

- Use the **numRows** and **rowSelection** arguments to filter rows

- Use the **varsToKeep** and **varsToDrop** arguments to filter columns

- Use the **rowsPerRead** argument to set the chunk size

- Use the **overwrite** and **append** arguments to overwrite or append to the end of an existing XDF file



### Controlling data schemas

- Use the **colClasses** argument to override the type of a column
  - Useful for indicating that a column is a factor

- Use the **colInfo** argument to specify the layout of fixed format data in a text file



### Transforming data on import

- Transform data on import with the **transforms** argument

  - A list of transformations

  - Can also create new columns

  - Can reference any variable in the file

  - Internal variables provide limited state information about the import process

- You must reference external objects using **transformObjects**

- Consider implementing complex transformations in a transform function (see Module 4)



### Refactoring variables

- Use **rxFactor** to

  - Convert nonfactor variables into factors

  - Refactor existing factor variables

- Works with XDF objects and data frames

- Supports column level filtering (**varsToKeep** and **varsToDrop**)

- Does not support row level filtering or transformations



### Importing composite data



- The **rxImport** functions can import multiple text files
  - Specify a folder as the data source

- The output can be a composite XDF

  - Specify a folder as the destination

  - Set the **createCompositeSet** argument to TRUE

  - **rxImport** creates a **metadata** folder and a **data** folder holding the files

- To read a composite XDF file, create an **RxXdfData** data source over the folder holding the **metadata** and **data** folders

```R
myfilelists <- list.files(rxGetOption("sampleDataDir"))
myfilepath <- file.path(rxGetOption("sampleDataDir"), "claims.txt")
df <- rxImport(myfilepath)
str(df)
class(df)
tmp_data <- RxTextData(myfilepath)
class(tmp_data)
tmp_data$colNames
myfilepath2 <- file.path(rxGetOption("sampleDataDir"), "AirlineDemoSmall.csv")
myfilepath2
df2 <- rxImport(myfilepath2)
head(df2)
df2
df3 <- read.csv(myfilepath2)
df3
df4 = rxImport(inData = myfilepath2, outFile = "c:/Test_Data/AirlineDemoSmall.xdf", overwrite = TRUE, stringsAsFactors = TRUE)
rxGetInfo(df4, getVarInfo = TRUE)
View(df2)
list.files(rxGetOption("sampleDataDir"))
df5 = rxImport(rxGetOption("sampleDataDir"), "AirlineDemoSmall.xdf")

claims = file.path((rxGetOption("sampleDataDir")), "claims.txt")
claims
tmp_data = rxImport(inData = claims, outFile = "c:/Test_Data/claims.xdf", overwrite = TRUE, stringsAsFactors = TRUE)
rxGetInfo(tmp_data, getVarInfo = TRUE)

# XDF to data.frame
df10 = rxDataStep(inData = tmp_data)
class(df10)
View(df10)

# Filering Data
rxImport(infile, outFile = outfile, numRows = 10000, rowSelection = (cost >= 0))
```



### Splitting large XDF files

- Use **rxSplit** to divide a large file vertically
  - Useful if you want to process parts of a file separately, or in parallel

- You can split by:

  1. Factor, where each file contains the data for a different factor value

  - Number of rows, to create uniformly sized files

- The **rxSplit** function also supports filtering and transformations 



### Combining XDF files

- Use **rxMerge** to combine files

- Different types of merges supported

  - **Union** (files must have the same variables)

  - **OneToOne**

  - Inner and outer merges (relational)



#### Exerciser Conbine

#### Table 1

```markdown
# Table 1
| AccountNo | Billee | Patient |
|-----------|--------|---------|
| 0538      | Rich C | 1       |
| 0538      | Rich C | 2       |
| 0538      | Rich C | 3       |
| 0763      | Tom D  | 1       |
| 1534      | Kath P | 1       |
```

#### Create Data.Frame 1

```R
# Create Data.Frame 1
acct <- c(0538, 0538, 0538, 0763, 1534)
billee <- c("Rich C", "Rich C", "Rich C", "Tom D", "Kath P")
patient <- c(1, 2, 3, 1, 1)
acctDF <- data.frame(acct = acct, billee = billee, patient = patient)
```

#### Table 2

```markdown
# Table 2
| AccountNo Patient Procedure | Billee | Patient |
|-----------------------------|--------|---------|
| 0538 3 OffVisit             | Rich C | 1       |
| 0538 2 AdultPro             | Rich C | 2       |
| 0538 2 OffVisit             | Rich C | 3       |
| 0538 3 2 SurfCom            | Tom D  | 1       |
| 0763 1 OffVisit             | Kath P | 1       |
```

#### Create Data.Frame 2

```R
# Create Data.Frame 2
acct <- c(0538, 0538, 0538, 0538, 0763, 0763, 0763)
patient <- c(3, 2, 2, 3, 1, 1, 2)
type <- c("OffVisit", "AdultPro", "OffVisit", "2SurfCom", "OffVisit", "AdultPro", "OffVisit")
procedureDF <- data.frame(acct = acct, patient = patient, type = type)
```

#### Inner Join

```R
# Inner join
rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "inner", matchVars = c("acct", "patient"))
```

####  Inner Join Result

```markdown
# Inner Join Result
|   | acct | billee | patient | type     |
|---|------|--------|---------|----------|
| 1 | 538  | Rich C | 2       | AdultPro |
| 2 | 538  | Rich C | 2       | OffVisit |
| 3 | 538  | Rich C | 3       | OffVisit |
| 4 | 538  | Rich C | 3       | 2SurfCom |
| 5 | 763  | Tom D  | 1       | OffVisit |
| 6 | 763  | Tom D  | 1       | AdultPro |
```

#### Outer Join

```R
rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "left", matchVars = c("acct", "patient"))
rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "right", matchVars = c("acct", "patient"))
rxMerge(inData1 = acctDF, inData2 = procedureDF, type = "full", matchVars = c("acct", "patient"))
```

#### Outer Join Result

```markdown
# Left Join
|   | acct | billee | patient | type     |
|---|------|--------|---------|----------|
| 1 | 538  | Rich C | 1       |   <NA>   |
| 2 | 538  | Rich C | 2       | AdultPro |
| 3 | 538  | Rich C | 2       | OffVisit |
| 4 | 538  | Rich C | 3       | OffVisit |
| 5 | 538  | Rich C | 3       | 2SurfCom |
| 6 | 763  | Tom D  | 1       | OffVisit |
| 7 | 763  | Tom D  | 1       | OffVisit |
| 8 | 1534 | Kath P | 1       |   <NA>   |

# Right Join
|   | acct | billee | patient | type     |
|---|------|--------|---------|----------|
| 1 | 538  | Rich C | 2       | AdultPro |
| 2 | 538  | Rich C | 2       | OffVisit |
| 3 | 538  | Rich C | 3       | OffVisit |
| 4 | 538  | Rich C | 3       | 2SurfCom |
| 5 | 763  | Tom D  | 1       | OffVisit |
| 6 | 763  | Tom D  | 1       | AdultPro |
| 7 | 763  |  <NA>  | 1       | OffVisit |

# Full Join
|   | acct | billee | patient | type       |
|---|------|--------|---------|------------|
| 1 | 538  | Rich C | 1       | <NA>       |
| 2 | 538  | Rich C | 2       | AdultPro   |
| 3 | 538  | Rich C | 2       | OffVisit   |
| 4 | 538  | Rich C | 3       | OffVisit   |
| 5 | 538  | Rich C | 3       | 2SurfCom   |
| 6 | 763  | Tom P  | 1       | 1 OffVisit |
| 7 | 763  | Tom P  | 1       | 1 AdultPro |
| 8 | 763  | <NA>   | 2       | OffVisit   |
| 9 | 1534 | Kath P | 1       | <NA>       |
```





#### One-to-One Join

```markdown
# First Data Set
| 1 | a | x |
| 2 | b | y |
| 3 | c | z |
```

```
# Create First Data Set
myData1 <- data.frame( x1 = 1:3, y1 = c("a", "b", "c"), z1 = c("x", "y", "z"))
```

```markdown
# Second Data Set
| 101 | d | u |
| 102 | e | v |
| 103 | f | w |
```

```R
# Create Second Data Set
myData2 <- data.frame(x2 = 101:103, y2 = c("d", "e", "f"), z2 = c("u", "v", "w"))
```

```R
# One-to-One Join
rxMerge(inData1 = myData1, inData2 = myData2, type = "oneToOne")
```

```markdown
# One-to-One Join Result
| x1 | y1 | z1 | x2    | y2 | z2 |
|----|----|----|-------|----|----|
| 1  | 1  | a  | x 101 | d  | u  |
| 2  | 2  | b  | y 102 | e  | v  |
| 3  | 3  | c  | z 103 | f  | w  |
```



#### Union Join Result

```
# Create Data Set
names(myData2) <- c("x1", "x2", "x3")

# Union Join
rxMerge(inData1 = myData1, inData2 = myData2, type = "union")
```

```markdown
# Union Join Result
|   | x1  | y1 | z1 |
|---|-----|----|----|
| 1 | 1   | a  | x  |
| 2 | 2   | b  | y  |
| 3 | 3   | c  | z  |
| 4 | 101 | d  | u  |
| 5 | 102 | e  | v  |
| 6 | 103 | f  | w  |
```



### Lab: Exploring big data

- LON-DEV에서 Visual Studio로 LON-SQLR에 접속해 Table 만들어주기 

```
setwd("E:\\Demofiles\\Mod02")
conString <- "Server=172.16.0.2;Database=AirlineData;uid=sa;pwd=1234"
airportData <- RxSqlServerData(connectionString = conString, table = "Airports")
colClasses <- c(
	"iata" = "character",
	"airport" = "character",
	"city" = "character",
	"state" = "factor",
	"country" = "factor",
	"lat" = "numeric",
	"long" = "numeric")
csvData <- RxTextData(file = "airports.csv", colClasses = colClasses)
rxDataStep(inData = csvData, outFile = airportData, overwrite = TRUE)
```



### Exercise1: Importing and transforming CSV data

```
# EX 1 - Run locally

setwd("E:\\Labfiles\\Lab02")

# Examine the structure of the data file
flightDataSampleCsv <- "2000.csv"
flightDataSample <- rxImport(flightDataSampleCsv, numRows = 10)

# View Variable Info 
rxGetVarInfo(flightDataSample)
```



```R
# The structure of the data
flightDataColumns <- c("Year" = "factor",
                       "DayofMonth" = "factor",
                       "DayOfWeek" = "factor",
                       "UniqueCarrier" = "factor",
                       "Origin" = "factor",
                       "Dest" = "factor",
                       "CancellationCode" = "factor"
                       )

# Read the CSV file and write it out as an XDF file
flightDataXdf <- "2000.xdf"
rxOptions(reportProgress = 1)
flightDataSampleXDF <- rxImport(inData = flightDataSampleCsv, outFile = flightDataXdf, overwrite = TRUE, append = "none", colClasses = flightDataColumns)

# Check the structure of the new file
rxGetVarInfo(flightDataXdf)
```



```R
# Compare sizes of XDF and CSV files at this point (using File Explorer, not R)

# Compare performance of CSV and XDF

system.time(csvDelaySummary <- rxSummary(~., flightDataSampleCsv))
# print(csvDelaySummary)

system.time(xdfDelaySummary <- rxSummary(~., flightDataSampleXDF))
# print(xdfDelaySummary)

# Generate crosstabs and cubes for cancelled flights - still comparing performance
system.time(csvCrossTabInfo <- rxCrossTabs(~as.factor(Month):as.factor(Cancelled == 1), flightDataSampleCsv))
# print(csvCrossTabInfo)

system.time(xdfCrossTabInfo <- rxCrossTabs(~as.factor(Month):as.factor(Cancelled == 1), flightDataSampleXDF))
# print(xdfCrossTabInfo)

system.time(csvCubeInfo <- rxCube(~as.factor(Month):as.factor(Cancelled), flightDataSampleCsv))
# print(csvCubeInfo)

system.time(xdfCubeInfo <- rxCube(~as.factor(Month):as.factor(Cancelled), flightDataSampleXDF))
# print(xdfCubeInfo)

# Tidy up memory
rm(flightDataSample, flightDataSampleXDF, csvDelaySummary, xdfDelaySummary, 
   csvCrossTabInfo, xdfCrossTabInfo, csvCubeInfo, xdfCubeInfo)
```

XDF 파일로 연산하였을 때가 CSV파일로 연산하였을 때보다 약 10배 빠르다.....



### Exercise 2: Combing and transforming data

```
# EX 2 - Run remotely on R Server

# Preparation: Log into the LON-RSVR VM and create the Data share over the C:\Temp folder
# Copy the CSV files from E:\Setup\Data to \\LON-RSVR\\Data

remoteLogin("http://LON-RSVR.ADATUM.COM:12800", session = TRUE, diff = TRUE, commandline = TRUE)

pause()

putLocalObject(c("flightDataColumns"))

resume()

ls()

# Transform the data - create a combined Delay column, filter all cancelled flights, and discard FlightNum, TailNum, and CancellationCode
# Test import and transform over a small sample first
flightDataSampleXDF <- rxImport(inData = "\\\\LON-RSVR\\Data\\2000.csv", outFile = "\\\\LON-RSVR\\Data\\Sample.xdf", overwrite = TRUE, append = "none", colClasses = flightDataColumns,
                                transforms = list(
                                  Delay = ArrDelay + DepDelay + ifelse(is.na(CarrierDelay), 0, CarrierDelay) + ifelse(is.na(WeatherDelay), 0, WeatherDelay) + ifelse(is.na(NASDelay), 0, NASDelay) + ifelse(is.na(SecurityDelay), 0, SecurityDelay) + ifelse(is.na(LateAircraftDelay), 0, LateAircraftDelay),
                                  MonthName = factor(month.name[as.numeric(Month)], levels=month.name)),
                                rowSelection = (Cancelled == 0),
                                varsToDrop = c("FlightNum", "TailNum", "CancellationCode"),
                                numRows = 1000
)

head(flightDataSampleXDF, 100)

# Combine separate CSV files containing data for each year into one big XDF file, performing the same transformations (which have now been tested)
rxOptions(reportProgress = 1)

delayXdf <- "\\\\LON-RSVR\\Data\\FlightDelayData.xdf"
flightDataCsvFolder <- "\\\\LON-RSVR\\Data"
flightDataXDF <- rxImport(inData = flightDataCsvFolder, outFile = delayXdf, overwrite = TRUE, append = ifelse(file.exists(delayXdf), "rows", "none"), colClasses = flightDataColumns,
                          transforms = list(
                            Delay = ArrDelay + DepDelay + ifelse(is.na(CarrierDelay), 0, CarrierDelay) + ifelse(is.na(WeatherDelay), 0, WeatherDelay) + ifelse(is.na(NASDelay), 0, NASDelay) + ifelse(is.na(SecurityDelay), 0, SecurityDelay) + ifelse(is.na(LateAircraftDelay), 0, LateAircraftDelay),
                            MonthName = factor(month.name[as.numeric(Month)], levels=month.name)),
                          rowSelection = (Cancelled == 0),
                          varsToDrop = c("FlightNum", "TailNum", "CancellationCode"),
                          rowsPerRead = 500000
)

exit
```





Exercise 3: Incorporating data from SQL Server into an XDF file







Exercise 4: Refactoring data and generating summaries