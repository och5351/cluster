<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Spark 개념

<br>

통합 컴퓨팅 엔진이며, 클러스터 환경에서 데이터를 병렬로 처리하는 라이브러리 집합.
병렬 처리 오픈소스 엔진.

<a id="home1"></a>

## 목차

- [1. SparkSession](#1)
- [2. DataFrmae](#2)
- [3. 파티션](#3)
- [4. 트랜스포메이션](#4)
- [5. 지연연산](#5)
- [6. 액션](#6)
- [7. Spark UI](#7)
- [8. 데이터 읽기](#8)
- [9. SQL DataFrame](#9)

<br>

<a id="1"></a>

## 1. SparkSession

> SparkSession 인스턴스는 사용자가 정의한 처리명령을 클러스터에서 실행.

> 하나의 SparkSessio은 하나의 스파크 애플리케이션에 대응

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

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. DataFrame

> 테이블의 데이터를 로우와 컬럼으로 단순하게 표현

> 컬럼과 컬럼의 타입을 정의한 목록을 스키마라고 부름

> 스파크는 R과 Python 의 DataFrame을 쉽게 변환 가능(Python pandas df -> spark dataframe)

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="3"></a>

## 3. 파티션

> 스파크는 모든 익수큐터가 병렬로 작업을 수행할 수 있도록 팡티션이라 불리는 청크 단위로 데이터 분할

> 파티션은 클러스터의 물리적 머신에 존재하는 로우의 집합

> DataFrame 의 파티션은 실행 중에 데이터가 컴퓨터 클러스터에서 물리적으로 분산되는 방식을 나타냄

> 파티션이 하나일 경우 익스큐터가 아무리 많아도 병렬성은 1 반대로 파티션이 많아도 익스큐터가 하나일 겅우 병렬성 1

> DataFrame을 사용하면 파티션을 수동 혹은 개별적으로 처리할 필요 없음

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="4"></a>

## 4. 트랜스포메이션

> 스파크의 핵심 데이터 구조는 불변성, 한 번 생성하면 변경할 수 없음

> DataFrame 을 변경하려면 원하는 변경 방법을 스파크에 전달 필요, 이를 트랜스포메이션이라고 함.

<br>

```python
myRange = spark.range(1000).toDF('number')

divisBy2 = myRange.where("number % 2 = 0")
```

<br>

> 위 코드를 실행해도 결과가 출력되지 않는다. 이는 추상적인 트뢘스포메이션만 지정한 상태이기 때문에 액션을 호출하지 않으면 스파크는 실제 트랜스포메이션을 수행하지 않는다.

<br>

1. 좁은 의존성
   좁은 의존성을 가진 트랜스포메이션은 각 입력 파티션이 하나의 출력 파티션에만 영향을 미침. where 구문은 좁은 의존성을 가짐. <b><i><u>파이프라이닝</u></i></b> 을 수행

   <br>

   <div align="center">
   <img src="https://user-images.githubusercontent.com/45858414/162236024-021fb809-4b2d-4cb2-8162-dd170e1fa58b.png" width="70%" height="70%" />
   </div>

<br>

2. 넓은 의존성
   넓은 의존성을 가진 트랜스포메이션은 하나의 입력 파티션이 여러 출력 파티션에 영향을 미침. <b><i><u>셔플</u></i></b> 로 스파크가 클러스터에서 파티션을 교환

   <br>

   <div align="center">
   <img src="https://user-images.githubusercontent.com/45858414/162237239-b049bad4-45ad-4dd1-86fe-00c2bbb8d81e.png" width="70%" height="70%" />
   </div>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="5"></a>

## 5. 지연연산

> 스파크가 연산 그래프를 처리하기 직전까지 기다리는 동작 방식을 의미

> 특정 연산 명령이 내려진 즉시 데이터를 수정하지 않고 원시 데이터에 적용할 트랜스포메이션의 실행 계획을 생성

> DataFrame의 조건절 pushdown

> 복잡한 스파크 job이 원천 데이터에서 하나의 row만 가져오는 필터를 가지고 있다면 필요한 레코드 하나만 읽는 것이 가장 효율적.

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="6"></a>

## 6. 액션

> 실제 연산을 수행하려면 액션 명령 필요

> 트랜스포메이션으로부터 결과를 계산하도록 지시하는 명령

```python
myRange = spark.range(1000).toDF('number')

divisBy2 = myRange.where("number % 2 = 0")

divisBy2.count()
```

<br>

세 가지 유형의 액션

1. 콘솔에서 데이터를 보는 액션
2. 각 언어로 된 네이티브 객체에 데이터를 모으는 액션
3. 출력 데이터소스에 저장하는 액션

<br>

액션을 지정하면 스파크 잡이 시작되고, 스파크 잡은 필터(좁은 트랜스포메이션)를 수행 후 파티션별로 레코드 수를 카운트(넓은 트랜스포메이션) 한다. 그리고 각 언어에 적합한 네이티브 객체에 결과를 모음.

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="7"></a>

## 7. Spark UI

> 드라이버 노드의 4040 포트로 접속 가능

> 로컬 모드일 경우 http://localhost:4040

> 스파크 잡의 상태, 환경 설정, 클러스터 상태 등의 정보 확인 가능

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="8"></a>
