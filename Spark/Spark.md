<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Spark 정리

<br>

통합 컴퓨팅 엔진이며, 클러스터 환경에서 데이터를 병렬로 처리하는 라이브러리 집합.
병렬 처리 오픈소스 엔진.

지원언어

- Python
- Java
- Scala
- R

<a id="home1"></a>

## 목차

- [1. Spark 설명](#1)
- [2. Spark 역사](#2)
- [3. Spark 설치](#3)
- [4. Spark 실행 설명](#4)
- [5. 스파크 어플리케이션 구조](#5)
- [6. SparkSession](#6)

<br>

<a id="1"></a>

## 1. Spark 설명

<br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162107944-7a3332ac-6595-4f74-9494-9765bd95e815.png" width="70%" height="70%"/>
</div>

<br><br>

- 스파크는 '빅데이터 어플리케이션 개발에 필요한 통합 플랫폼을 제공하자'는 핵심 폭표를 가지고 있음. <br><br>

- 간단한 데이터 읽기부터 SQL 처리, 머신러닝 그리고 스트림 처리에 이르기까지 다양한 데이터 분석 작업을 같은 연산 엔진과 일관성 있는 API로 수행할 수 있도록 설계. <br><br>

- 스파크의 개발 사상은 <i><b><u>주피터 노트북의 대화형 분석 도구, 운영용 어플리케이션 개발 등 </u></b></i> 과 같이 다양한 처리 유형과 라이브러리를 결합해 수행. <br><br>

- 조합형 API를 제공하므로 작은 코드 조각이나 기존 라이브러리를 사용해 애플리케이션 만들기 용이, 직접 스파크 기반 라이브러리 제작 가능. (ex. SQL 데이터 Read, ML 라이브러리로 머신러닝 모델 2가지 프로세스를 한번에 수행 가능) <br><br>

- 스파크의 역할은 저장소 시스템의 데이터를 연산하는 역할만 수행. 저장소 역할은 수행하지 않음. 다양한 저장소 연결 가능(Hadoop, Azure Storage, S3, Cassandra, Kafka 등)<br><br>

- 스파크 코어 엔진 자체는 최초 공개 후 큰 변화가 없었지만 라이브러리의 경우 많은 기능을 제공하기 위해 발전(스파크 SQL, MLlib, 스파크 스트리밍, GraphX, 다양한 저장소 시스템 커넥터 등 제공) <br><br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

### 2. Spark 역사

<br>

1. 스파크의 첫 번째 버전은 배치 어플리케이션만 지원.
2. 얼마 지나지 않아 대화형 데이터 분석이나 ad-hoc-query 같은 강력한 기능 제공(AMPLap 은 스칼라 인터프리터를 단순히 스파크에 접목하여 대화형 시스템 제공)
3. 샤크 개발(2011, 대화형으로 SQL을 실행)
4. AMPLap 의 여러 그룹이 MLlib, 스파크 스트리밍, GraphX 만들기 시작.
5. 2013년 30개 이상의 UC 버클리 대학교 외부 조직에 100명 이상의 기여자가 있는 프로젝트로 성장
6. AMPLap 의 아파치 재단 기증, AMPLap 은 데이터브릭스 설립

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="3"></a>

## 3. Spark 설치

<br>

1. 로컬 환경에 스파크 내려받기

   > http://spark.apache.org/downloads.html

   1. Hadoop 버전 선택
   2. Spark 버전 선택
   3. 다운로드
   4. 압축해제 후 spark-3.2.1-bin-hadoop3.2/bin 에 pyspark 실행

<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162178368-15cd68cc-d28b-4a37-966c-920e2f9cff89.png" width="70%" heigh="70" />
</div>

<br>

2. jupyter notebook 에서 pyspark 구동

```
! pip install pyspark
```

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="4"></a>

### 4. Spark 실행 설명

<br>

<table>
    <thead>
    <tr>
        <th colspan="1">스파크 실행 형식</th>
        <th colspan="1">설명</th>
        <th colspan="1">리눅스/Mac OS</th>
        <th colspan="1">윈도우</th>
    </tr>
    </thead>
    <tbody>
        <tr>
            <td>기본</td>
            <td>스파크에서 스칼라를 사용할 때 실행</td>
            <td>spark-shell<br>
            <td>spark-shell.cmd spark-shell2.cmd</td>
        </tr>
        <tr>
            <td>PySpark</td>
            <td>스파크에서 파이썬을 사용할 때 실행</td>
            <td>pyspark</td>
            <td>pyspark.cmd, pyspark2.cmd</td>
        </tr>
        <tr>
            <td>SparkR</td>
            <td>스파크에서 R을 사용할 때 실행</td>
            <td>sparkR</td>
            <td>sparkR.cmd, sparkR2.cmd</td>
        </tr>
        <tr>
            <td>SparkSQL</td>
            <td>스파크에서 SQL을 사용할 때 실행</td>
            <td>spark-sql</td>
            <td>-</td>
        </tr>
        <tr>
            <td>스파크 애플리케이션</td>
            <td>스파크 애플리케이션을 클러스터 환경에서 구동할 때  사용</td>
            <td>spark-sumit</td>
            <td>spark-submit.cmd, spark-submit2.cmd</td>
        </tr>
    </tbody>
</table>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="5"></a>

## 5. 스파크 어플리케이션 구조

<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162205862-10eec13c-4f74-42d7-9ea7-4be946d4c966.png" width="100%" height="70%" />
</div>

스파크 애플리케이션은 드라이버, 익스큐터 프로세스로 구성됨

- 드라이버 프로세스
  - 클러스터 노드 중 하나에서 실행됨
  - main() 함수를 실행
  - 스파크 애플리케이션 정보의 유지 관리
  - 사용자 프로그램이나 입력에 대한 응답
  - 전반적인 익스큐터 프로세스의 작업과 관련된 분석, 배포, 스케줄링
  - 애플리케이션의 수명 주기 동안 관련 정보 모두 유지

<br>

- 익스큐터 프로세스
  - 드라이버가 할당한 코드를 실행
  - 코드 진행 상황을 드라이버 노드에 보고

<br>

- 클러스터 매니저
  - 스파크 Stand alone 클러스터 매니저, 하둡 Yarn, 메소스 중 선택 가능
  - 사용 가능한 자원을 파악하기 위해 사용

<br>

- SparkSession
  - 사용자는 스파크 코드를 실행하기 위해 SparkSession 객체를 진입점으로 사용
  - 스파크는 사용자를 대신해 Python 이나 R로 작성한 코드를 익스큐터의 JVM에서 실행할 수 있는 코드로 변환

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162208170-233551e9-96e4-4f2a-8b0a-ef8fcc2f5dfb.png" width="70%" height="70%">
</div>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="6"></a>

## 6. SparkSession

<br>

1. SparkSession 불러오기

```python
from pyspark.sql import SparkSession
```

<br>

2. SparkSession 초기화

```python
spark = SparkSession\
        .builder\
        .appName('Python Spark SQL basic example')\
        .config('spark.some.config.option', 'some-value')\
        .getOrCreate()
```

<br>

3. 사용

```python
myRange = spark.range(1000).toDF('number')
```
