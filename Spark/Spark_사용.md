<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Spark 사용

<br>

통합 컴퓨팅 엔진이며, 클러스터 환경에서 데이터를 병렬로 처리하는 라이브러리 집합.
병렬 처리 오픈소스 엔진.

<a id="home1"></a>

## 목차

- [1. 데이터 읽기](#1)
- [2. SQL DataFrame](#2)

<br>

<a id="1"></a>

## 1. 데이터 읽기

<br>

SparkSession의 DataFrameReader 클래스를 사용하여 데이터 읽음.

> 스키마 추론 기능

```python
flightData2015 = spark\
.read\
.option('inferSchema', 'true')\
.option('header', 'true')\
.csv('./data/flight-data/csv/2015-summary.csv') # 데이터 준비
```

<hr>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162369998-8800bf5a-7c26-4272-834c-24962fea0cb5.png" width="70%" height="70%" />
<br>
<p>DataFrame에서 CSV 파일을 읽어 로컬 배열이나 리스트 형태로 변환하는 과정</p>
</div>

<hr>

> head 명령과 같은 결과

```python
flightData2015.take(3)

# 결과
#[Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Romania', count=15),
# Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Croatia', count=1),
# Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Ireland', count=344)]
```

> sort 메소드는 DataFrame 을 변경하지 않는다. 트랜스포메이션으로 sort 메소드를 사용하여 이전의 DataFrame을 변환해 새로운 DataFrame 을 생성해 반환한다.

<hr>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162370747-293430c7-06f6-4c3d-8162-918934dd8641.png" width="70%" height="70%" /><br><br>
<p>DataFrame을 이용한 데이터 읽기, 정렬, 수집</p>
</div>
<hr>

> 특정 DataFrame 객체에 explain 메소드를 호출하면 DataFrame의 계보(lineage)나 스파크의 쿼리 실행 계획을 확인할 수 있음.

<hr>

```python
flightData2015.sort('count').explain()

# 결과
# == Physical Plan ==
# AdaptiveSparkPlan isFinalPlan=false
# +- Sort [count#28 ASC NULLS FIRST], true, 0
#   +- Exchange rangepartitioning(count#28 ASC NULLS FIRST, 200), ENSURE_REQUIREMENTS, [id=#71]
#      +- FileScan csv [DEST_COUNTRY_NAME#26,ORIGIN_COUNTRY_NAME#27,count#28] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[file:/Users/och/Desktop/spark/pyspark/data/flight-data/csv/2015-summar..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,ORIGIN_COUNTRY_NAME:string,count:int>
```

> sort, exchange, filescan

<hr>

트랜스포메이션 실행 계획을 세우기 위한 액션 호출

```python
spark.conf.set('spark.sql.shuffle.partitions', '5')

flightData2015.sort('count').take(2)

#[Row(DEST_COUNTRY_NAME='United States', ORIGIN_COUNTRY_NAME='Singapore', count=1),
# Row(DEST_COUNTRY_NAME='Moldova', ORIGIN_COUNTRY_NAME='United States', count=1)]
```

> 셔플 수행 시 기본적으로 200개의 셔플 파티션 생성

> Transformation 의 논리적 실행 계획은 DataFrame의 계보를 정의

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. SQL DataFrame

<br>

사용자가 SQL이나 DataFrame(R, Python, Scala, Java) 으로 비즈니스 로직을 표현하면 스파크에서 실제 코드를 실행하기 전에 그 로직을 기본 실행 계획(explain 메소드를 호출해 실행 계획을 확인할 수 있음) 으로 컴파일 함. 스파크 SQL을 사용하면 모든 DataFram을 테이블이나 뷰(임시 테이블)로 등록한 후 SQL 쿼리를 사용할 수 있음.

> createOrReplaceTempView 메소드를 호출하면 모든 DataFrame을 테이블이 나 뷰로 만들 수 있음

1. 테이블 생성

```python
flightData2015.sort('count').createOrReplaceTempView('flight_data_2015')
```

<br>

2. 실행 계획

```python
sqlWay = spark.sql('''
SELECT DEST_COUNTRY_NAME, count(1)
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
''')

dataFrameWay = flightData2015\
.groupBy('DEST_COUNTRY_NAME')\
.count()
```

<br>

3. 확인

```python
sqlWay.explain()

# 결과
# == Physical Plan ==
# AdaptiveSparkPlan isFinalPlan=false
# +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[count(1)])
#    +- Exchange hashpartitioning(DEST_COUNTRY_NAME#26, 5), ENSURE_REQUIREMENTS, [id=#93]
#       +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[partial_count(1)])
#          +- FileScan csv [DEST_COUNTRY_NAME#26] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[file:/Users/och/Desktop/spark/pyspark/data/flight-data/csv/2015-summar..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string>

dataFrameWay.explain()

# 결과
# == Physical Plan ==
# AdaptiveSparkPlan isFinalPlan=false
# +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[count(1)])
#    +- Exchange hashpartitioning(DEST_COUNTRY_NAME#26, 5), ENSURE_REQUIREMENTS, [id=#106]
#       +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[partial_count(1)])
#          +- FileScan csv [DEST_COUNTRY_NAME#26] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[file:/Users/och/Desktop/spark/pyspark/data/flight-data/csv/2015-summar..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string>
```

<br>

4. max 함수

```python
from pyspark.sql.functions import max

flightData2015.select((max('count'))).take(1)
```

<hr>

5. 1 SQL 예제

```python
maxSql = spark.sql('''
SELECT DEST_COUNTRY_NAME, sum(count) as destination_total
FROM flight_data_2015
GROUP BY DEST_COUNTRY_NAME
ORDER BY sum(count) DESC
LIMIT 5
''')

maxSql.explain()

# 결과
# == Physical Plan ==
# AdaptiveSparkPlan isFinalPlan=false
# +- TakeOrderedAndProject(limit=5, orderBy=[destination_total#61L DESC NULLS LAST], output=[DEST_COUNTRY_NAME#26,destination_total#61L])
#    +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[sum(count#28)])
#       +- Exchange hashpartitioning(DEST_COUNTRY_NAME#26, 5), ENSURE_REQUIREMENTS, [id=#231]
#          +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[partial_sum(count#28)])
#             +- FileScan csv [DEST_COUNTRY_NAME#26,count#28] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[file:/Users/och/Desktop/spark/pyspark/data/flight-data/csv/2015-summar..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,count:int>
```

5. 2 pyspark 예제

```python
flightData2015\
.groupBy('DEST_COUNTRY_NAME')\
.sum('count')\
.withColumnRenamed('sum(count)','destination_total')\
.sort(desc('destination_total'))\
.limit(5)\
.explain()

# 결과
# == Physical Plan ==
# AdaptiveSparkPlan isFinalPlan=false
# +- TakeOrderedAndProject(limit=5, orderBy=[destination_total#116L DESC NULLS LAST], output=[DEST_COUNTRY_NAME#26,destination_total#116L])
#    +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[sum(count#28)])
#       +- Exchange hashpartitioning(DEST_COUNTRY_NAME#26, 5), ENSURE_REQUIREMENTS, [id=#214]
#          +- HashAggregate(keys=[DEST_COUNTRY_NAME#26], functions=[partial_sum(count#28)])
#             +- FileScan csv [DEST_COUNTRY_NAME#26,count#28] Batched: false, DataFilters: [], Format: CSV, Location: InMemoryFileIndex(1 paths)[file:/Users/och/Desktop/spark/pyspark/data/flight-data/csv/2015-summar..., PartitionFilters: [], PushedFilters: [], ReadSchema: struct<DEST_COUNTRY_NAME:string,count:int>
```

<br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/162392662-1fc50534-30b0-4615-91ad-9e2fe0c8bc2b.png" height="70%" /> <br>
<p>DataFrame의 변환 흐름</p>
</div>

    1. 데이터를 읽음
    2. 데이터를 그룹화. GroupBy 메소드가 호출 되면 RelationalGroupedDataset 반환, 키 혹은 키셋을 기준으로 그룹을 만들고 각 키에 대한 집계 수행
    3. 집계 유형을 지정하기 위해 컬럼 표현식이나 컬럼명을 인수로 사용하는 sum 메소드를 사용. 별도의 DataFrame 을 생성. 아무런 연산이 이루어지지 않는 트랜스포메이션.
    4. withColumnRenamed 메소드에 원본 컬럼명과 신규 컬럼명을 인수로 지정. 연산은 일어나지 않음.
    5. desc Column 객체를 반환
    6. limit
    7. explain
