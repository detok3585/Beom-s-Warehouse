# HDInsigh in Azure



## Azure에서 HDInsight를 구성해보자



### 기본설정

Azure 홈 - 리소스 만들기 - 분석 - HDInsight 의 경로로 이동하자. 아래 화면이 나올것이다.

![image](https://user-images.githubusercontent.com/46669509/54793007-d6d1a100-4c83-11e9-96c6-c018c0b1750e.png)

위에서 부터

1. 클러스터 네임
2. 구독
3. 클러스터 타입
4. 클러스터 로그인 유저네임
5. 클러스터 로그인 패스워드
6. SSH 유저네임
7. 리소스 그룹
8. 위치

이 중 필수적으로 설정변경해야 하는 것은 

- 클러스터네임
- 클러스터 로그인 패스워드
- 클러스터 타입

이다.



### 스토리지 설정

![image](https://user-images.githubusercontent.com/46669509/54793071-1c8e6980-4c84-11e9-8940-63776bb26000.png)

위에서 부터

1. 주 저장소 타입
2. 선택방법
3. 저장소 계정

옵션은 생략한다.





### 설정 사항 요약

![image](https://user-images.githubusercontent.com/46669509/54793101-3e87ec00-4c84-11e9-83af-e51a3aa43dd4.png)

설정한 내용들이 맞는지 확인하고 생성한다.