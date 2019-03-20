# Windows 용 Machine Learning Server 설치

### 구성방식

ML 서버 구성은 2가지 방식이 있다.

1. One-box - 웹 노드, 컴퓨터 노드, 데이터베이스로 구성.

![image](https://user-images.githubusercontent.com/46669509/54591188-11152580-4a6d-11e9-8c4e-275f54233f20.png)

2. Enterprise -  노드들을 거미줄처럼 구성하여 가용성을 고려.

![image](https://user-images.githubusercontent.com/46669509/54591172-06f32700-4a6d-11e9-9352-7c5fe53dab96.png)





## 설치방법

#### 이미지파일

이미지 파일은 `[Visual Studio Dev Essentials]`(https://www.visualstudio.com/dev-essentials/)`에서 다운로드 받는다.

가상머신에 운영체제 설치하는 방법은 생략



#### 환경변수

Intel MKL (Math Kernel Library) 를 사용하기 때문에 환경변수를 설정해 주어야한다.

1. 제어판에서 **시스템 및 보안** > **시스템** > **고급 시스템 설정** > **환경 변수를** 클릭.
2. 시스템 변수를 생성

- 변수 이름 :  `MKL_CBWR`
- 변수 값 : `AUTO`



## 설치확인

#### R

1. C : \ Program Files \ Microsoft \ ML Server \ R_SERVER \ bin \ x64로 이동.
2.  **Rgui.exe** 실행
3. 명령 줄에 `search()` 를 입력해서 설치된 패키지 확인.
4. `print(Revo.version)` 입력하여 소프트웨어 버전을 확인.
5. `rxSummary(~., iris) `   명령어를 입력해서 출력값 확인 ` rxSummary`는 RevoScaleR에서 제공하는 함수



### **파이썬**

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

```
#출력값
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



### CLI

1. 관리자 명령 프롬프트를 실행
2. `az ml admin bootstrap` 입력.
3. 패스워드 설정





