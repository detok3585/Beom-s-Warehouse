

# Manage and configure Machine Learning Server for operationalization

## Machine Learning Server의 두가지 형태 

#### 1. Enterprise-  노드들을 거미줄처럼 구성하여 가용성을 고려.

A [enterprise configuration](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-machine-learning-server-enterprise) where multiple nodes are configured on multiple machines along with other enterprise features. This configuration can be scaled out or in by adding or removing nodes. Learn more about this setup in the [enterprise configuration](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-machine-learning-server-enterprise) article. For added security, you can [configure SSL](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-https) and authenticate against [Active Directory (LDAP) or Azure Active Directory](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-authentication) in this configuration.

![image](https://user-images.githubusercontent.com/46669551/54585983-f936a500-4a5e-11e9-9a28-ce344974521a.png)

#### 2. One-box- 웹 노드, 컴퓨터 노드, 데이터베이스로 구성.

As the name suggests, a [one-box configuration](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-machine-learning-server-one-box) involves one web node and one compute node run on a single machine. Set-up is a breeze. This configuration is useful when you want to explore what it is to operationalize R and Python analytics using Machine Learning Server. It is perfect for testing, proof-of-concepts, and small-scale prototyping, but might not be appropriate for production usage. This configuration is covered in this article. Learn more in this [One-box configuration](https://docs.microsoft.com/en-us/machine-learning-server/operationalize/configure-machine-learning-server-one-box) article.

![image](https://user-images.githubusercontent.com/46669551/54584052-91ca2680-4a59-11e9-85df-1d4d89722553.png)



### 아래의 링크를 통해 설치를 실시

https://docs.microsoft.com/en-us/machine-learning-server/install/machine-learning-server-windows-install

### Download 를 진행하여  Server로 활용할 Machine에 설치한다.

![image](https://user-images.githubusercontent.com/46669551/54582553-832d4080-4a54-11e9-852c-db91a1ae6a65.png)



![image](https://user-images.githubusercontent.com/46669551/54582576-9b9d5b00-4a54-11e9-825d-31c2bcb9c2c2.png)

![image](https://user-images.githubusercontent.com/46669551/54582594-aeb02b00-4a54-11e9-80f2-513febc9dfd4.png)



![image](https://user-images.githubusercontent.com/46669551/54582675-f9ca3e00-4a54-11e9-9cd5-3a2218cf9655.png)



![image](https://user-images.githubusercontent.com/46669551/54582616-bf60a100-4a54-11e9-93f3-2c024f41f393.png)

![image](https://user-images.githubusercontent.com/46669551/54582639-d69f8e80-4a54-11e9-86ed-693f139d83ac.png)

![image](https://user-images.githubusercontent.com/46669551/54582746-3a29bc00-4a55-11e9-872f-e2ecc376bc5d.png)





## 환경변수

Intel MKL (Math Kernel Library) 를 사용하기 때문에 환경변수를 설정해 주어야한다.

1. 제어판에서 **시스템 및 보안** > **시스템** > **고급 시스템 설정** > **환경 변수를** 클릭.
2. 시스템 변수를 생성

- 변수 이름 :  `MKL_CBWR`
- 변수 값 : `AUTO`





### R Server가 설치된 Machine으로 이동하여 Server로써의 구성을 실시

# R Setting

1. C : \ Program Files \ Microsoft \ ML Server \ R_SERVER \ bin \ x64로 이동.
2. **Rgui.exe** 실행
3. 명령 줄에 `search()` 를 입력해서 설치된 패키지 확인.
4. `print(Revo.version)` 입력하여 소프트웨어 버전을 확인.
5. `rxSummary(~., iris) `   명령어를 입력해서 출력값 확인 ` rxSummary`는 RevoScaleR에서 제공하는 함수





# Python Setting

Python runs when you execute a .py script or run commands in a Python console window.

1. C : \ Program Files \ Microsoft \ ML Server \ PYTHON_SERVER로 이동
2. **Python.exe를** 실행
3. 명령 행에 `help()`를 입력해 도움말 확인.
4. `revoscalepy`를 입력하여 패키지확인.
5. 아래 스크립트를 입역해서 출력값 확인

```python
import os
import revoscalepy 
sample_data_path = revoscalepy.RxOptions.get_option("sampleDataDir")
ds = revoscalepy.RxXdfData(os.path.join(sample_data_path, "AirlineDemoSmall.xdf"))
summary = revoscalepy.rx_summary("ArrDelay+DayOfWeek", ds)  
print(summary)
```

Output from the sample dataset should look similar to the following:


  ```python
Summary Statistics Results for: ArrDelay+DayOfWeek
File name: ... AirlineDemoSmall.xdf
Number of valid observations: 600000.0

        Name       Mean     StdDev   Min     Max  ValidObs  MissingObs
0  ArrDelay  11.317935  40.688536 -86.0  1490.0  582628.0     17372.0

Category Counts for DayOfWeek
Number of categories: 7

            Counts
DayOfWeek         
1          97975.0
2          77725.0
3          78875.0
4          81304.0
5          82987.0
6          86159.0
7          94975.0
  ```



# Azure Setting

open admin command prompt window

Enter the following command to check availability of the CLI: `az ml admin --help`. If you receive the following error: `az: error argument _command_package: invalid choice: ml`, follow the instructions to re-add the extension to the CLI.

##### Enable web service deployment and remote connections

1. 관리자 명령 프롬프트를 실행
2. `az ml admin bootstrap` 입력.
3. 패스워드 설정

![image](https://user-images.githubusercontent.com/46669551/54584189-0d2bd800-4a5a-11e9-84e3-1996136e4896.png)

















### 원격으로 접속할 Client 서버에 R Client를 설치

# R Client 설치 

```
https://aka.ms/rclient/
```



# R Studio for Client  

#### R studio를 실행시켜 Global Option을 확인하면, Default값이 R_Server에 위치함

![image](https://user-images.githubusercontent.com/46669551/54584375-8297a880-4a5a-11e9-8b13-16035aff6bea.png)

### Command를 이용하여 R Server에 연결

#### R-Studio에 아래의 Command를 실행하여 원격으로 접속이 가능한지 확인한다. 

```R
remoteLogin(deployr_endpoint = "http://172.16.0.102:12800",session = TRUE, diff = TRUE, commandline = TRUE, username = "admin", password = "Pa$$w0rd")
```













