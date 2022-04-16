<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Spark 개념

<br>

통합 컴퓨팅 엔진이며, 클러스터 환경에서 데이터를 병렬로 처리하는 라이브러리 집합.
병렬 처리 오픈소스 엔진.

<a id="home1"></a>

## 목차

- [1. 구조적 API 개요](#1)
- [2. DataFrame과 DataSet](#2)
- [3. 구조적 API의 실행 과정](#3)
- [4. 스키마](#4)
- [5. 컬럼과 표현식](#5)

<br><br>
<a id="1"></a>

## 1. 구조적 API 개요

구조적 API의 3가지 분산 컬렉션 API

- Dataset
- DataFrame
- SQL 테이블과 뷰

<br>

> 배치 <-> 스트리밍 변환 가능

<br>

아래 3가지 기본 개념 숙지

1. 타입형(typed)/비타입형(untyped) API의 개념과 차이점
2. 핵심 용어
3. 스파크가 구조적 API의 데이터 흐름을 해석하고 클러스터에서 실행하는 방식

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. DataFrame과 DataSet

DataFrame 과 Dataset은 잘 정의된 로우와 컬럼을 가지는 분산테이블 형태의 컬렉션.

> 기본적으로 테이블과 뷰는 DataFrame과 같음. 대신 테이블은 DataFrame 코드 대신 SQL을 사용.

1. Schema

- 스키마는 DataFrame의 컬럼명과 데이터 타입 정의.
- 스키마는 데이터소스에서 얻거나 직접 정의 가능.
- 여러 데이터 타입으로 구성되므로 어떤 데이터 타입이 어느 위치에 있는지 정의하는 방법 필요

2. 스파크의 구조적 데이터 타입 개요

- 스파크는 사실상 프로그래밍 언어며 카탈리스트 엔진을 사용하여 실행 계획 수립과 처리에 사용하는 자체 데이터 타입 정보를 가지고 있음.
- 스파크는 자체 데이터 타입을 지우너하는 여러 언어 API와 직접 매핑되며, 매핑 테이블을 가지고 있음.

2. 1 DataFrame VS Dataset

   DataFrame : 비타입형, 데이터 타입이 있기는 하지만 스키마에 명시된 데이터 타입의 일치 여부를 런타임이 되서 확인.
   Dataset : 타입형, 스키마에 명시된 데이터 타입의 일치 여부를 컴파일 타임에 확인. JVM 기반의 언어인 스칼라와 자바에서만 지원.

> 스파크의 DataFrame은 Row 타입으로 구성된 Dataset.

> Row 타입은 스파크가 사용하는 '연산에 최적화된 인메모리 포맷'의 내부적인 표현 방식.

> Row 타입을 사용하면 가비지 컬렉션과 객체 초기화 부하가 있는 FVM 데이터 타입을 사용하는 대신 자체 데이터 포맷을 사용하기 때문에 매우 효율적인 연산 가능.

<br><br>

2. 2 컬럼

- 정수형이나 문자열 같은 단순 데이터 타입
- 배열이나 맵 같은 복합 데이터 타입
- null 값

<br><br>

2. 3 Row

   로우는 데이터 레코드, DataFrame 의 레코드는 Row 타입을 구성.
   Row는 SQL, RDD, 데이터소스에서 얻거나 직접 만들 수 있음

<br><br>

2. 4 스파크 데이터 타입
   <br>

2.4.1 scala

```scala
import org.apache.spark.sql.types._

val b = ByteType
```

2.4.2 Java

```java
import org.apache.spark.sql.types.DataTypes;

ByteType x = DataTypes.ByteType;
```

2.4.3 python

```python
from pyspark.sql.types import *

b = ByteType()
```

<br><br>

2.4.4 파이썬 데이터 타입 매핑

| 스파크 데이터 타입 | 파이썬 데이터 타입                                                                                                                                                                              | 데이터 타입 생성/접근용 API                                                                                           |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| ByteType           | int, long [숫자는 런타임에 1바이트 크기의 부호형 정수로 변환. -128 ~ 127 사이의 값을 가짐.]                                                                                                     | ByteType()                                                                                                            |
| ShortType          | int, long [숫자는 런타임에 2바이트 크기의 부호형 정수로 변환. -32768 ~ 32767 사이의 값을 가짐.]                                                                                                 | ShortType()                                                                                                           |
| IntegerType        | int, long [파이썬은 '정수형' 데이터 타입의 숫자를 관대하게 정의. 매우 큰 숫자값을 IntegerType()에서 사용하면 스파크 SQL에서 거부할 수 있음. 숫자값이 너무 크면 LongType을 사용]                 | IntegerType()                                                                                                         |
| LongType           | long [숫자는 런타임에 8바이트 크기의 부호형 정수로 변환. -9223372036854775808 ~ 9223372036854775807 사이의 값을 가짐. 더 큰 숫자를 사용하려면 decimal.Decimal 형으로 변환해 DecimalType을 사용] | LongType()                                                                                                            |
| FloatType          | float [숫자는 런타임에 4바이트 크기의 단정도 부동소수점으로 변환.]                                                                                                                              | FloatType()                                                                                                           |
| DoubleType         | float                                                                                                                                                                                           | DoubleType()                                                                                                          |
| DecimalType        | decimal.Decimal                                                                                                                                                                                 | DecimalType()                                                                                                         |
| StringType         | string                                                                                                                                                                                          | StringType()                                                                                                          |
| BinaryType         | bytearray                                                                                                                                                                                       | BinaryType()                                                                                                          |
| BooleanType        | bool                                                                                                                                                                                            | BooleanType()                                                                                                         |
| TimestampType      | datetime.datetime                                                                                                                                                                               | TimestampType()                                                                                                       |
| DateType           | datetime.date                                                                                                                                                                                   | DateType()                                                                                                            |
| ArrayType          | list, tuple, array                                                                                                                                                                              | ArrayType(elementType,[containsNull]) [containsNull의 기본값은 True]                                                  |
| MapType            | dict                                                                                                                                                                                            | MapType(keyType, valueType, [valueContainsNull]) [containsNull의 기본값은 True]                                       |
| StructType         | list, tuple                                                                                                                                                                                     | StructType(fields) [괄호안의 fields는 StructField 객체로 이루어진 파이썬 list 타입이며 같은 필드명은 사용할 수 없음.] |
| StructField        | 이 필드의 데이터 타입과 대응되는 파이썬 데이터 타입. IntegerType을 사용하는 StructField는 파이썬의 int 데이터 타입을 사용                                                                       | StructField(name, dataType, [nullable]) [nullable의 기본값은 True]                                                    |

> 스파크 공식 문서 : http://bit.ly/2EdflXW

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="3"></a>

## 3. 구조적 API의 실행 과정

진행과정

1. DataFrame/DataSet/SQL을 이용해 코드 작성
2. 정상적인 코드라면 스파크가 논리적 실행 계획으로 변환
3. 스파크는 논리적 실행 계획을 물리적 실행 계획을 변환하며 그 과정에서 추가적인 최적화를 할 수 있는지 확인
4. 스파크는 클러스터에서 물리적 실행 계획(RDD 처리)을 실행

> 카탈로그 옵티마이저

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/163678970-c8b33ae0-e7d0-400f-a13c-92ed336c5902.png' width="70%" height="70%">
</div>
<br><br>

3. 1 논리적 실행계획

<br>

> 구조적 API의 논리적 실행 계획 수립 과정

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/163679247-82719fbc-f13d-4019-92a3-849c4ab45568.png' width='70%' heigt='70%' />
</div>
<br>

- 논리적 실행 계획 단계에서는 추상적 트랜스포메이션만 표현. 이 단계에서는 드라이버나 익스큐터의 정보를 고려하지 않음.
- 사용자 코드는 검정 전 논리적 실행 계획으로 변환(코드의 유효성, 테이블이나 컬럼의 존재 여부 판단 과정, 실행 계획 검증하지 않은 상태)
- 스파크 분석기는 컬럼과 테이블을 검증하기 위해 카탈로그, 모든 테이블의 저장소 그리고 DataFrame 정보를 활용(필요한 테이블, 컬럼이 카탈로그에 없다면 검증 전 논리적 실행 계획이 만들어지지 않음.)
- 테이블과 컬럼에 대한 검증 결과는 카탈리스트 옵티마이즈로 전달.
- 카탈리스트 옵티마이저는 조건절 푸시다운이나 선택절 구문을 이요해 논리적 실행 계획을 최적화하는 규칙 모음(필요한 경우 도메인에 최적화된 규칙을 적용할 수 있는 카탈리스트 옵티마이저의 확장형 패키지 제작 가능)

<br><br>

3. 2 물리적 실행 계획

<br>

> 물리적 실행 계획 수립 과정

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/163679564-088ed3fb-1eb8-4ade-b21e-30197c68d80f.png' width='70%' heigt='70%'/>
</div>
<br><br>

- 스파크 실행 계획이라고 불리는 물리적 실행 계획(논리적 실행 계획을 클러스터 환경에서 실행하는 방법을 정의)
- 사용하려는 테이블의 크기나 파티션 수 등의 물리적 속성을 고려해 지정된 조인 연산 수행에 필요한 비용을 계산하고 비교
- 물리적 실행 계획은 일련의 RDD와 트랜스포메이션으로 변환
- 스파크는 DataFrame, Dataset, SQL로 정의된 쿼리를 RDD 트랜스포메이션으로 컴파일 함(스파크를 컴파일러 라고 부르기도 함)

<br><br>

3. 3 실행
   <br>

   스파크는 물리적 실행 계획을 선정한 다음 저수준 프로그래밍 인터페이스인 RDD를 대상으로 모든 코드를 실행. 스파크는 런타임에 전체 태스크나 스테이지를 제거할 수 있는 자바 바이트 코드를 생성해 추가적인 최적화를 수행.

<br><br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="4"></a>

## 4. 스키마

<br>

스키마는 DataFrame의 컬럼명과 데이터 타입을 정의. 데이터소스에서 얻거나 직접 정의 가능.

> 비정형 분석(ad-hoc analysis) 에서는 스키마-온-리드가 대부분 잘 동작.(CSV, JSON같은 일반 텍스트 파일은 다소 느릴 수 있음.) 그러나 Long 데이터 타입을 Integer 데이터 타입으로 잘못 인식 하는 등의 정밀도 문제 발생 가능. ETL 작업에 스파크를 사용하면 직접 스키마를 정의 해야한다. ETL 데이터 타입을 알기 힘든 CSV나 JSON등의 데이터소스를 사용하는 경우 스키마 추론 과정에서 읽어 들인 샘플 데이터의 타입에 따라 스키마를 결정해버릴 수도 있다.

<br>

4. 1 미국 교통통계국 항공운항 데이터 예제

```python
from pyspark.sql import SparkSession

spark = SparkSession\
        .builder\
        .appName('Python Spark Schema example')\
        .config('spark.some.config.option', 'some-value')\
        .getOrCreate()

sc = spark.sparkContext

spark.read.format('json').load('./data/flight-data/json/2015-summary.json').schema

#결과
#StructType(List(StructField(DEST_COUNTRY_NAME,StringType,true),
#StructField(ORIGIN_COUNTRY_NAME,StringType,true),
#StructField(count,LongType,true)))
```

- StructField는 이름, 데이터 타입, 컬럼이 값이 없가나 null일 수 있는지 지정하는 boolean 값을 가짐.
- 필요한 경우 컬럼과 관련된 메타데이터를 지정 가능
- 스키마는 복합 데이터 타입인 StructType을 가질 수 있다.

<br><br>

4. 2 DataFrame에 스키마를 만들고 적용하는 예제

```python
from pyspark.sql.types import StructField, StructType, StringType, LongType

myManualSchema = StructType([
    StructField('DEST_COUNTRY_NAME',StringType(),True),
    StructField('ORIGIN_COUNTRY_NAME',StringType(),True),
    StructField('count',LongType(),False, metadata={'hello':'world'})
])

df = spark.read.format('json').schema(myManualSchema)\
.load('./data/flight-data/json/2015-summary.json')
```

<br>
<div align="right">

[목차로](#home1)

</div><br><br>
<a id="5"></a>

## 5. 컬럼과 표현식

스파크의 컬럼은 표현식을 사용해 레코드 단위로 계산한 값을 단순하게 나타내는 논리적인 구조. DataFrame을 통하지 않으면 외부에서 컬럼에 접근할 수 없으며 컬럼내용을 수정하려면 트랜스포메이션을 사용해야 함.

<br>

5. 1 컬럼

<br>

col 함수나 column 함수를 사용하여 컬럼명을 인수로 받는다. 컬럼은 컬럼명을 카탈로그에 저장된 정보와 비교하기 전까지 미확인 상태로 남는다. 분석기가 동작하는 단계에서 컬럼과 테이블을 분석.

```python
from pyspark.sql.functions import col, column

col('someColumnName')
column('someColumnName')
```

<br><br>

5. 2 명시적 컬럼 참조
   <br>

   DataFrame의 컬럼은 col 메소드로 참조. col 메소드를 사용해 명시적으로 컬럼을 정의하면 스파크 분석기 실행 단계에서 컬럼 확인 절차를 생략.

<br><br>

5. 3 표현식
   <br>

   DataFrame을 정의할 때 컬럼은 표현식. 표현식은 DataFrame 레코드의 여러 값에 대한 트랜스포메이션 집합을 의미. 여러 컬럼명을 입력으로 받아 식별하고, '단일 값'을 만들기 위해 다양한 표현식을 각 레코드에 적용하는 함수.

   > 단일값은 Map이나 Array 같은 복합 데이터 타입일 수 있다.

   표현식은 expr 함수로 간단히 사용 가능.

   > expr('someCol') 은 col('someCol') 구문과 동일하게 동작

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="6"></a>

## 6. 컬럼과 표현식
