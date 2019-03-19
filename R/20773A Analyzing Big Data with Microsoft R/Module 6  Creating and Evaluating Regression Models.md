# Module 6 : Creating and Evaluating Regression Models

들어가기 앞서 회귀분석 먼저 알아보자.

```R
# Linear Regression
# 모델생성

x = c(151,174,138,186,128,136,179,163,152,131)
y = c(63,81,56,91,47,57,76,72,62,48)
plot(x,y)

# lm() 선형회귀 함수 
l_value = lm(y~x)
l_value

x1 = data.frame(x=170)
x1

# predict() 예측 함수 
result = predict(l_value,x1)
result

# abline(함수를 그래프에 출력)
plot(x,y,abline(lm(y~x)))

# multiple regression
# y = a + b1x1 + b2x2 .......



```

결과값

Call:
lm(formula = y ~ x)

Coefficients:
(Intercept)            x  
   -38.4551       0.6746  



```R
#데이터를 이용해서 회귀분석을 해보자.

tmp_data2 = mtcars[,c("mpg","disp","hp","wt")]
tmp_data2

# mpg = disp + hp + wt
# mpg ~ disp + hp + wt

model = lm( mpg ~  disp + hp + wt, data = tmp_data2)
model

# y = 37.105505 +(-0.000937)x1 + (-0.031157)x2 + (-3.800891)x3
```

결과 값.

Call:
lm(formula = mpg ~ disp + hp + wt, data = tmp_data2)

Coefficients:
(Intercept)         disp           hp           wt  
  37.105505    -0.000937    -0.031157    -3.800891

=>>>>  y = 37.105505 +(-0.000937)x1 + (-0.031157)x2 + (-3.800891)x3





## Lesson 1: Clustering big data

### Why perform clustering?

- Clustering is a type of unsupervised learning:

  - No labeled response data

  - Attempts to split the data into natural groups

  - No measure of accuracy

- Useful for:

  - Exploratory data analysis

  - Finding natural groups in your data

  - To reduce datasets into subsets of similar data

  - Dimension reduction

  - Preprocessing of data before running supervised learning

  - Nonrandom sampling





### K-means clustering

- K-means clustering (also know as partitioning clustering)

  - Divides “n” cases, described by “p” explanatory variables into a small number, “k”, of classes. 

  - You must specify k, the number of classes





### Performing k-means clustering

- The RevoScaleR package provides **rxKmeans** for k-means clustering

  - Optimised for large datasets over clusters

  - Implements the Lloyd algorithm for heuristic clusterin

  ![image](https://user-images.githubusercontent.com/46669509/54587257-eb831e80-4a62-11e9-8ae8-a3e8e9c897cc.png)

  - Use a right-handed formula because there are no response variables
  - Set an appropriate number of clusters for k 
  - The data argument can be a big data resource (XDF)