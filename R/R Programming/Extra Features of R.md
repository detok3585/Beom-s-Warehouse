# 추가 기능

rep(‘#’, times=i) **<u>#반복 출력</u>**

paste(rep(‘#’, times=i), collapse = “”)) **<u>#반복 출력 한 것을 붙히기</u>**

print((paste(rep(‘#’, times=i), collapse = “”))) **<u>#반복 출력 한 것을 붙여 출력하기</u>**

var1 <- c(0,1,2,3)

var2 <- c('Heoolo','R programming')

c(TRUE, 1) -> var3

print(var1)

cat("var1 is", var1,"\n")

cat(var1,var2,var3) **<u># cat 은 변수들을 합쳐 출력할 수 있다</u>**

print(ls()) **<u># 전체 목록을 출력</u>**

data11 <- c(10, NA, 20, 30) **<u># NA는 값이 없음을 의미</u>**

1/0  **<u># Inf = Infinity(무한대)</u>**

0/0  **<u># NaN = Not a Number(숫자가 아니다)</u>**

print(ls()) **<u># 전체 지정된 변수 출력</u>**

ls(pattern = "data") **<u># 전체 지정 변수 중 문자열로 찾기</u>**

rm(pattern = "data") **<u># 문자열을 같고있는 변수를 삭제하기</u>**

rm(list = ls()) **<u># 전체 지정된 변수 삭제</u>**

getwd() **<u># working directory 알아보기</u>** 

setwd("c:/share") **<u># working directory 변경하기</u>** 

setwd("C:/RWorkShop")

 

# Package 호출 및 설치

\#.libPaths()

\#library()

\#search()

\#install.packages('XML')

\#install.packages('ggplot2')

\#library(XML)

\#library(ggplot2)

\#remove.packages("ggplot2")

\#remove.packages("XML")

\#help(package=“패키지명”)

\#help(package=“ggplot2”)