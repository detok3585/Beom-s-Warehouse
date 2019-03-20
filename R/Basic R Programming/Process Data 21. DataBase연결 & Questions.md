# DataBase연결 

#### install.packages('RMySQL')

library(RMySQL)

mysqlconnection = dbConnect(MySQL(), dbname = 'employees', 

​                            user = 'hadoop', 

​                            password = 'Pa$$w0rd123',

​                            host = '192.168.137.100')

 

dbListTables(mysqlconnection) **<u># DB의 Table을 확인</u>**

result <- dbGetQuery(mysqlconnection, "SELECT * FROM employees")

 

class(result)

str(result)

dim(result)

head(result)



### 문제

#### 남녀 비율의 분포를 출력 하시오

result$gender

library(ggplot2)

qplot(result$gender)

colnames(result)

 

dbListTables(mysqlconnection) **<u># DB의 Table을 확인</u>**

result2 <- dbGetQuery(mysqlconnection, "SELECT * FROM departments")

result2 **<u># DB의 departments Table을 확인</u>**

 

dbListTables(mysqlconnection) **<u># DB의 Table을 확인</u>**

result3 <- dbGetQuery(mysqlconnection, "SELECT * FROM salaries")

result3 **<u># DB의 salaries Table을 확인</u>**

 

 