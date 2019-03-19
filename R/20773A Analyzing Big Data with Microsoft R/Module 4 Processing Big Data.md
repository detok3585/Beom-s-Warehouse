# Module 4 Processing Big Data

1. #### Transforming big data

2. #### Managing big datasets

#### 

## Lesson 1: Transforming big data

### When should a transformation be permanent?

- Transformations can be permanent or transient

  영구적이거나 일시적으로 만들 수 있다.

- Permanent transformations incur I/O and storage costs

  입출력속도와 저장공간의 비용을 고려하여 영구적인 변환을 실시한다.

- Transient transformations can consume considerable resources if repeated often

  일시적인 변환은 세션을 종료할때 마다 사라짐. 

- Consider the costs and benefits concerning:

  - Reuse

  - Storage resources

  - Processing resources

  - Volatility of the data

재사용 여부, 스토리지 리소스, 프로세싱 리소스, 데이터의 변동성 등을 고려하여 결정한다.



### Using the rxDataStep function

- Use **rxDataStep** to implement transformations in XDF objects

  - XDF data is read as chunks into memory
    XDF 데이터는 데이터를 조각내서 메모리에서 읽는다.

  - Each chunk is processed and then written back to disk

    각각의 청크데이터는 처리 된 후에 디스크에 기록된다.

- Ensure that the chunk size is not too big to fit into memory

  청크 사이크는 기본적인 메모리 사이즈보다 클 수 없다.

  - Tune using **rowsPerRead** and **blocksPerRead**

    **rowsPerRead** : 한번에 읽을 수 있는 row 개수

    **blocksPerRead :** 한번에 읽을 수 있는 block 개수

- Specify transformations in the **transforms** list

  - Avoid transformations that require simultaneous access to all observations in the dataset

  - Add R variables to the transforms closure using the **transformObjects** list

```R
# rxGetInfo Fuction - 파일의 정보 출력
rxGetInfo("C:\\Data\\FlightDelayData.xdf", getBlockSizes = TRUE)

# Result
File name: C:\Data\FlightDelayData.xdf 
Number of observations: 11602437 
Number of variables: 29 
Number of blocks: 27 
Rows per block (first 10): 483202 485743 128712 485188 470856 190555 493648 494317 54169 490696
Compression type: zlib 
```

```R

```



### Adding new variables to a dataset

- Transforms contain statements of the form:

  ​		  *var* = *expression*

  - If *var* exists in the dataset, it is modified

  - If *var* doesn’t exist, it is created and added to every row

- If you are adding a categorical variable, specify the levels and labels explicitly to avoid inconsistencies between chunks



### Subsetting variables in a dataset

- Use **varsToDrop** or **varsToKeep** to specify which variables to remove or retain in the result

- Use **rowSelection** to delete entire rows
  - Specify a logical expression that references at least one variable in the dataset (or one of the special ***.rx*** variables)

- Limit the scope of the transformation using **startRow**, **numRows**, **startBlock**, and **numBlocks**



### Incorporating third-party packages into a transformation

- External packages are not included in the transforms closure

- Specify packages referenced by transformations using the **transformPackages** argument



### Using custom transformation functions

- Use custom functions to perform complex transformations

  - Reference the function using **transformFunc**

  - The function runs once for each chunk

- The function is given a chunk of data in the form of a list of vectors
  - One vector for each variable specified by using **transformVars**

- The function can:
  - Update information in these vectors
  - Add new vectors to the list
  - Delete vectors

- State information is available in the ***.rx*** variables

- Use **.rxGet** and **.rxSet** to pass information between chunks



### Reblocking an XDF file

- Transformations and subsetting can cause blocks in XDF files to become unbalanced

  - Some blocks might contain many fewer rows than others

  - Some blocks might be nearly empty

- Use **rxDataStep** with the **rowsPerRead** argument to reblock the file

![image](https://user-images.githubusercontent.com/46669509/54501432-a11d7700-4968-11e9-94b7-af95d03cfde9.png)

**rowsPerRead**



### Demonstration: Using a transformation function to calculate a running total

```R
# Connect to R Server
remoteLogin(deployr_endpoint = "http://LON-RSVR.ADATUM.COM:12800", session = TRUE, diff = TRUE, commandline = TRUE, username = "admin", password = "Pa55w.rd")
## 연결 안될시 양쪽 머신의 Services에서 Remote Aceess Auto Connetion Manager, Remote Aceess Connetion Manager service를 켜준다.

# Examine the dataset
FlightDelayData <- "\\\\LON-RSVR\\Data\\FlightDelayData.xdf"
rxGetInfo(FlightDelayData, getVarInfo = TRUE, getBlockSize = TRUE)
head(RxXdfData(FlightDelayData))

# Create the transformation function
addRunningTotal <- function (dataList) {
  
  # Check to see whether this is a test chunk
  if (.rxIsTestChunk) {
    return(dataList)
  }
  
  # Retrieve the current running total for the distance from the environment
  runningTotal <- as.double(.rxGet("runningTotal"))
  
  # Add a new vector for holding the running totals 
  # and add it to the list of variable values
  runningTotalVarIndex <- length(dataList) + 1
  dataList[[runningTotalVarIndex]] <- rep(as.numeric(NA), times = .rxNumRows)
  names(dataList)[runningTotalVarIndex] <- "RunningTotal"
  
  # Iterate through the values for the Distance variable and accumulate them
  idx <- 1
  for (distance in dataList[[1]]) {
    runningTotal <- runningTotal + distance
    dataList[[runningTotalVarIndex]][idx] <- runningTotal
    idx <- idx + 1
  }
  
  # Save the running total back to the environment, ready for the next chunk
  .rxSet("runningTotal", as.double(runningTotal))
  return(dataList)
}

# Run the transformation
EnhancedFlightDelayData <- "\\\\LON-RSVR\\Data\\EnhancedFlightDelayData.xdf"

rxOptions("reportProgress" = 2)
rxDataStep(inData = FlightDelayData, outFile = EnhancedFlightDelayData, 
           overwrite = TRUE, append = "none",
           transforms = list(ObservationNum = .rxStartRow : (.rxStartRow + .rxNumRows - 1)),
           transformFunc = addRunningTotal,
           transformVars = c("Distance"),
           transformObjects = list(runningTotal = 0),
           numRows = 2000000,
           rowsPerRead = 200000
)

# View the results
rxGetInfo(EnhancedFlightDelayData, getVarInfo = TRUE, getBlockSize = TRUE)
head(RxXdfData(EnhancedFlightDelayData))
tail(RxXdfData(EnhancedFlightDelayData))

# Plot a line to visualize the results using a random sample of the data
rxLinePlot(RunningTotal ~ ObservationNum, EnhancedFlightDelayData, 
           rowSelection = rbinom(.rxNumRows, size = 1, prob = 0.01)
)


```





## Lesson 4: Managing big datasets

### Considerations for sorting big data

- Sorting big data is expensive

- Alternatives to sorting:

  - Generate cross-tabulations using **rxCrossTabs** or **rxCube**. 

    - Data is sorted by variable using a single pass through the data

    - You can wrangle the results as a data frame

    - Scale noninteger numeric variables if necessary

- Use custom transformation functions to calculate aggregate results by group

- Use ScaleR functions such as **rxQuantiles**, **rxLorenz** and **rxLoc** rather than the more traditional R equivalents
  - Many ScaleR functions don’t require data to be presorted, and can process datasets in a single pass



### Sorting and removing duplicates from big data

- Use **rxSort** to sort large datasets and data frames

  - Specify sort keys using **sortByVars**

  - Specify sort order using **decreasing**

  - Factors are sorted by level, not name

- Data is sorted in memory if possible, or by using a merge sort
  - Use **varsToKeep**/**varsToDrop** to reduce the size of the dataset and improve performance (can make the difference between sorting in memory or on disk)

- Remove rows with duplicate key values using **removeDupKeys**
  - Record the frequency of removed rows with **dupFreqVar**



### Joining big datasets

- Use **rxMerge** to combine datasets

  - **union** appends one dataset to another vertically

  - **oneToOne** appends one dataset to another horizontally

- **rxMerge** also supports relational-style joins

  - Inner joins combine rows where key values in both datasets match

  - Rows in either dataset that have no match in either do not appear

  - Outer joins (left, right, and full) can retain rows with no matches in the other dataset by generating **NA** values

- Joins require the data to be sorted on the join keys

- If joining on factors, ensure that they have the same levels in both datasets



### Demonstration: Sorting data with rxSort

- Tuning a sort

- Removing duplicates from sorted data

```R
# Connect to R Server
remoteLogin(deployr_endpoint = "http://LON-RSVR.ADATUM.COM:12800", session = TRUE, diff = TRUE, commandline = TRUE, username = "admin", password = "Pa55w.rd")

# Examine the data
flightDelayDataSubset = RxXdfData("\\\\LON-RSVR\\Data\\FlightDelayData.xdf")
head(flightDelayDataSubset)                            

# Sort it by Origin (this will take approx 90 seconds)
sortedFlightDelayData = "SortedFlightDelayData.xdf"
sortedData <- rxSort(inData = flightDelayDataSubset, outFile = sortedFlightDelayData, overwrite = TRUE,
                     sortByVars = c("Origin"), decreasing = c(TRUE))

# Note the factor levels for Origin
rxGetVarInfo(sortedData, varsToKeep = c("Origin"))

# View the data. It should be sorted in descending order of Origin
head(sortedData,200)

#Sort the data again. This should be much quicker as it only uses a subset of the variables
sortedData <- rxSort(inData = flightDelayDataSubset, outFile = sortedFlightDelayData, overwrite = TRUE,
                     sortByVars = c("Origin"), decreasing = c(TRUE),
                     varsToKeep = c("Origin", "Dest", "Distance", "CRSDepTime"))

# View the data
head(sortedData,200)

# De-dup routes
sortedData <- rxSort(inData = flightDelayDataSubset, outFile = sortedFlightDelayData, overwrite = TRUE,
       sortByVars = c("Origin"), decreasing = c(TRUE),
       varsToKeep = c("Origin", "Dest", "Distance"),
       removeDupKeys = TRUE, dupFreqVar = "RoutesFrequency"
)

# View the data
head(sortedData,200)


```







## Lab: Processing big data

- ### Exercise1: Merging the airport and flight delay datasets

#### 요약

목적 : airport와 flight delay데이터셋 병합

1. R서버가 있는 LON-RSVR 머신에 접속
2. 두 데이터 셋을 불러옴
3. airportData에서 'iata', filghtDelayData에서 'Origin','Dest' 변수 정보를 추출(팩터정보)
4. 추출한 변수정보를 바탕으로 중복값을 제거하여 리팩터레벨을 생성
5. 팩터 변수를 만든 리팩터레벨을 이용하여 다시 생성.
6. 만든 팩터 변수에서 변수정보 추출(팩터정보)
7. 병합하기 위해 리팩터된 airport변수의 열을 'Origin'으로 변경
8. airport 데이터를 다시 블록화
9. 병합
10. 결과확인

```R
#EX 1

# Connect to R Server - R 서버가 있는 LON-RSVR 머신에 접속.
remoteLogin(deployr_endpoint = "http://LON-RSVR.ADATUM.COM:12800", session = TRUE, diff = TRUE, commandline = TRUE, username = "admin", password = "Pa55w.rd")


# Examine the factor levels in each dataset - 데이터 셋에서 각 요소들 추출
airportData = RxXdfData("\\\\LON-RSVR\\Data\\airportData.xdf")
flightDelayData = RxXdfData("\\\\LON-RSVR\\Data\\flightDelayData.xdf")

iataFactor <- rxGetVarInfo(airportData, varsToKeep = c("iata"))
print(iataFactor)

originFactor <- rxGetVarInfo(flightDelayData, varsToKeep = c("Origin"))
print(originFactor)

destFactor <- rxGetVarInfo(flightDelayData, varsToKeep = c("Dest"))
print(destFactor)
# 실행결과 - 해당 변수에서 팩터 추출.
# Var 1: iata
#       3377 factor levels: 00M 00R 00V 01G 01J ... ZER ZPH ZUN ZZV CBM

# Create a set of levels for refactoring the datasets - 각 팩터에서 팩터레벨들을 모아 다시 팩터링함.
refactorLevels <- unique(c(iataFactor$iata[["levels"]],
                           originFactor$Origin[["levels"]],
                           destFactor$Dest[["levels"]]))
# refactorLevels
# [1] "00M"  "00R"  "00V"  "01G"  "01J"  "01M"  "02A"  "02C"  "02G"  "03D" 
# [11] "04M"  "04Y"  "05C"  "05F"  "05U"  "06A"  "06C"  "06D"  "06M"  "06N" 


# Refactor the datasets - 데이터셋들은 다시 팩터화.
rxOptions(reportProgress = 2)
refactoredAirportDataFile <- "\\\\LON-RSVR\\Data\\RefactoredAirportData.xdf"
refactoredAirportData <- rxFactors(inData = airportData,
                                   outFile = refactoredAirportDataFile,
                                   overwrite = TRUE,
                                   factorInfo = list(iata = 
                                                     list(newLevels = refactorLevels)))
# rxFactors 메소드 : 팩터 레벨과 인덱스를 새 집합에 매핑하여 팩터 변수를 다시 만듦.

refactoredFlightDelayDataFile <- "\\\\LON-RSVR\\Data\\RefactoredFlightDelayData.xdf"
refactoredFlightDelayData <- rxFactors(
    inData = flightDelayData, 
    outFile = refactoredFlightDelayDataFile, 
    overwrite = TRUE, 
    factorInfo = list(Origin = list(newLevels = refactorLevels),
                      Dest = list(newLevels = refactorLevels))
)

# Verify the new factor levels in each dataset. They should all be the same - 리팩터된 요소들로 다시 팩터레벨을 추출.
iataFactor <- rxGetVarInfo(refactoredAirportData, varsToKeep = c("iata"))
print(iataFactor)

originFactor <- rxGetVarInfo(refactoredFlightDelayData, varsToKeep = c("Origin"))
print(originFactor)

destFactor <- rxGetVarInfo(refactoredFlightDelayData, varsToKeep = c("Dest"))
print(destFactor)

# Rename the iata variable as Origin - names must match when joining - iata 변수의 이름을 변경해준다. 조인할때 이름이 같아야 하기 때문에 변경한다.
names(refactoredAirportData)[[1]] <- "Origin" 

# reblock the airport data file - airport데이터를 다시 블록화한다
reblockedAirportDataFile <- "\\\\LON-RSVR\\Data\\reblockedAirportData.xdf"
reblockedAirportData <- rxDataStep(refactoredAirportData, 
                                   reblockedAirportDataFile,
                                   overwrite = TRUE
                                  )

# Perform the merge to add the timezone of the Origin airport - 병합 
mergedFlightDelayDataFile <- "\\\\LON-RSVR\\Data\\MergedFlightDelayData.xdf"
mergedFlightDelayData <- rxMerge(inData1 = refactoredFlightDelayData,
                                 inData2 = reblockedAirportData,
                                 outFile = mergedFlightDelayDataFile,
                                 overwrite = TRUE,
                                 type = "inner",
                                 matchVars = c("Origin"),
                                 autoSort = TRUE,
                                 varsToKeep2 = c("timezone", "Origin"),
                                 newVarNames2 = c(timezone = "OriginTimeZone"),
                                 rowsPerOutputBlock = 500000
                                )

# Check that the data now contains OriginTimeZone variable - 결과를 확인한다.
rxGetVarInfo(mergedFlightDelayData)
head(mergedFlightDelayData)
tail(mergedFlightDelayData)

```



- ### Exercise 2: Transforming departure and arrival dates to UTC

```R
# EX2

# Generate a sample of the data to transform
rxOptions(reportProgress = 1)

flightDelayDataSubsetFile <- "\\\\LON-RSVR\\Data\\flightDelayDataSubset.xdf"
flightDelayDataSubset <- rxDataStep(inData = mergedFlightDelayData,
                                    outFile = flightDelayDataSubsetFile,
                                    overwrite = TRUE,
                                    rowSelection = rbinom(.rxNumRows,
                                                          size = 1, 
                                                          prob = 0.005)
)

rxGetInfo(flightDelayDataSubset, getBlockSizes = TRUE)

# Date manipulation uses the lubridate package
install.packages("lubridate")

# Add departure and arrival times recorded as UTC to the dataset 
standardizeTimes <- function (dataList) {
  
  # Check to see whether this is a test chunk
  if (.rxIsTestChunk) {
    return(dataList)
  }
  
  # Create a new vector for holding the standardized departure time 
  # and add it to the list of variable values
  departureTimeVarIndex <- length(dataList) + 1
  dataList[[departureTimeVarIndex]] <- rep(as.numeric(NA), times = .rxNumRows)
  names(dataList)[departureTimeVarIndex] <- "StandardizedDepartureTime"
  
  # Do the same for standardized arrival time
  arrivalTimeVarIndex <- length(dataList) + 1
  dataList[[arrivalTimeVarIndex]] <- rep(as.numeric(NA), times = .rxNumRows)
  names(dataList)[arrivalTimeVarIndex] <- "StandardizedArrivalTime"
  
  departureYearVarIndex <- 1
  departureMonthVarIndex <- 2
  departureDayVarIndex <- 3
  departureTimeStringVarIndex <- 4
  elapsedTimeVarIndex <- 5
  departureTimezoneVarIndex <- 6
  
  # Iterate through the rows and add the standardized arrival and departure times
  for (i in 1:.rxNumRows) {
    
    # Get the local departure time details
    departureYear <- dataList[[departureYearVarIndex]][i]
    departureMonth <- dataList[[departureMonthVarIndex]][i]
    departureDay <- dataList[[departureDayVarIndex]][i]
    departureHour <- trunc(as.numeric(dataList[[departureTimeStringVarIndex]][i]) / 100)
    departureMinute <- as.numeric(dataList[[departureTimeStringVarIndex]][i]) %% 100
    departureTimeZone <- dataList[[departureTimezoneVarIndex]][i]
    
    # Construct the departure date and time, including timezone
    departureDateTimeString <- paste(departureYear, "-",
                                     departureMonth, "-",
                                     departureDay, " ", 
                                     departureHour, ":", 
                                     departureMinute, sep="")
    departureDateTime <- as.POSIXct(departureDateTimeString, tz = departureTimeZone)
    
    # Convert to UTC and store it
    standardizedDepartureDateTime <- format(departureDateTime, tz="UTC")
    dataList[[departureTimeVarIndex]][i] <- standardizedDepartureDateTime 

    # Calculate the arrival date and time
    # Do this by adding the elapsed time to the departure time
    # The elapsed time is stored as the number of minutes (an integer)
    elapsedTime = dataList[[5]][i]
    standardizedArrivalDateTime <- format(as.POSIXct(standardizedDepartureDateTime)
                                          + minutes(elapsedTime))
    
    # Store it
    dataList[[arrivalTimeVarIndex]][i] <- standardizedArrivalDateTime 
  }
  
  # Return the data including the new variables
  return(dataList)
}

# Transform the sample data
flightDelayDataTimeZonesFile <- "\\\\LON-RSVR\\Data\\flightDelayDataTimezones.xdf"
flightDelayDataTimeZones <- rxDataStep(inData = flightDelayDataSubset,
                                       outFile = flightDelayDataTimeZonesFile,
                                       overwrite = TRUE,
                                       transformFunc = standardizeTimes,
                                       transformVars = c("Year", 
                                                         "Month", 
                                                         "DayofMonth", 
                                                         "DepTime",
                                                         "ActualElapsedTime", 
                                                         "OriginTimeZone"),
                                       transformPackages = c("lubridate")
                         			   )

# Verify the results
rxGetVarInfo(flightDelayDataTimeZones)
head(flightDelayDataTimeZones)
tail(flightDelayDataTimeZones)
```



- ### Exercise 3: Calculating cumulative average delays for each route

```R
# EX3

# Sort the flight delay data by the departure time
rxOptions(reportProgress = 1)
sortedFlightDelayDataFile <- "sortedFlightDelayData.xdf"
sortedFlightDelayData <- rxSort(inData = flightDelayDataTimeZones, 
                                outFile = sortedFlightDelayDataFile, overwrite = TRUE,
                                sortByVars = c("StandardizedDepartureTime")
                               )

# Verify that the data has been sorted
head(sortedFlightDelayData)
tail(sortedFlightDelayData)

# Add cumulative average flight delays for each route
calculateCumulativeAverageDelays <- function (dataList) {
  
  # Check to see whether this is a test chunk
  if (.rxIsTestChunk) {
    return(dataList)
  }
  
  # Add a new vector for holding the cumulative average delay 
  # and add it to the list of variable values
  cumulativeAverageDelayVarIndex <- length(dataList) + 1
  dataList[[cumulativeAverageDelayVarIndex]] <- rep(as.numeric(NA), times = .rxNumRows)
  names(dataList)[cumulativeAverageDelayVarIndex] <- "CumulativeAverageDelayForRoute"

  originVarIndex <- 1
  destVarIndex <- 2
  delayVarIndex <- 3
  
  # Retrieve the vector containing the cumulative delays recorded so far for each route
  cumulativeDelays <- .rxGet("cumulativeDelays")

  # Retrieve the vecor containing the number of times each route has occurred so far
  cumulativeRouteOccurrences <- .rxGet("cumulativeRouteOccurrences")

  # Iterate through the rows and add the standardized arrival and departure times
  for (i in 1:.rxNumRows) {
  
    # Get the route and delay details
    origin <- dataList[[originVarIndex]][i]
    dest <- dataList[[destVarIndex]][i]
    routeDelay <- dataList[[delayVarIndex]][i]

    # Create a string that identifies the route
    route <- paste(origin, dest, sep = "")

    # Retrieve the current cumulative delay and number of occurrences for each route
    delay <- cumulativeDelays[[route]]
    occurrences <- cumulativeRouteOccurrences[[route]]
    
    # Update the cumulative statistics
    delay <- ifelse(is.null(delay), 0, delay) + routeDelay
    occurrences <- ifelse(is.null(occurrences), 0, occurrences) + 1

    # Work out the new running average delay for the route
    cumulativeAverageDelay <- delay / occurrences
    
    # Store the data and updated stats
    dataList[[cumulativeAverageDelayVarIndex]][i] <- cumulativeAverageDelay 
    cumulativeDelays[[route]] <- delay
    cumulativeRouteOccurrences[[route]] <- occurrences
  }
 
  # Save the lists containing the cumulative data so far
  .rxSet("cumulativeDelays", cumulativeDelays)
  .rxSet("cumulativeRouteOccurrences", cumulativeRouteOccurrences)
  
  # Return the data including the new variable
  return(dataList)
}

# Perform the transformation
flightDelayDataWithAveragesFile <- "\\\\LON-RSVR\\Data\\flightDelayDataWithAverages.xdf"
flightDelayDataWithAverages <- rxDataStep(inData = sortedFlightDelayData,
                                          outFile = flightDelayDataWithAveragesFile, overwrite = TRUE,
                                          transformFunc = calculateCumulativeAverageDelays,
                                          transformVars = c("Origin", "Dest", "Delay"),
                                          transformObjects = list(cumulativeDelays = list(), cumulativeRouteOccurrences = list())
                                         )

# Verify the results
rxGetVarInfo(flightDelayDataWithAverages)
head(flightDelayDataWithAverages)
tail(flightDelayDataWithAverages)

# Plot the delays for the Atlanta/Phoenix route
rxLinePlot(CumulativeAverageDelayForRoute ~ as.POSIXct(StandardizedDepartureTime), type = c("p", "r"),
           flightDelayDataWithAverages,
           rowSelection = (Origin == "ATL") & (Dest == "PHX"),
           yTitle = "Cumulative Average Delay for Route",
           xTitle = "Date"
          )

```

