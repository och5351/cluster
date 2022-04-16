<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Spark 기능

<br>

스파크의 라이브러리는 그래프 분석, 머신러닝, 스트리밍 등 다양한 작업을 지원하며, 컴퓨팅 및 스토ㄹ지 시스템과의 통합을 돕는 역할을 함.

<br>

- spark-submit : 명령으로 운영용 어플리케이션 실행
- Dataset : 타입 안정성(type-safe, 타입 세이프)을 제공하는 구조적 API
- 구조적 스트리밍 : 구조적 API로 개발된 배치 모드의 연산을 스트리밍 방식으로 실행할 수 있는 고수준 API
- 머신러닝과 고급 분석
- RDD : 스파크의 저수준 API
- SparkR
- 서드파티 패키지 에코시스템

<a id="home1"></a>

## 목차

- [1. 운영용 어플리케이션 실행](#1)
- [2. Dataset](#2)
- [3. 구조적 스트리밍](#3)
- [4. 머신러닝, 고급분석](#4)

<a id="2"></a>

## 1. 운영용 어플리케이션 실행

<br>

spark-submit 명령을 사용해 대화형 셸에서 개발한 프로그램을 운영용 어플리케이션으로 쉽게 전환 가능. 어플리케이션 코드를 클러스터에 전송해 실행시키는 역할을 하며 클러스터에 제출된 어플리케이션은 작업이 종료되거나 에러가 발생할 때까지 실행 된다.

<br>

```python
# 스파크를 설치한 디렉터리에서 예제 파일 실행
./bin/spark-submit \
--master local \
./examples/src/main/python/pi.py 10
```

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. Dataset

<br>

Dataset은 자바와 스칼라의 정적 데이터 타입에 맞는 코드, 즉 정적 타입 코드를 지원하기 위해 고안된 스파크의 구조적 API. 동적 타입 언어인 파이썬과 R에서는 사용할 수 없음. <br>
Dataset API는 DataFrame의 레코드를 사용자가 자바나 스칼라로 정의한 클래스에 할당하고 ArrayList(java), Seq(scala) 객체 등의 고정 타입형 컬렉션으로 다룰 수 있는 기능 제공. <br>
Dataset API 는 타입 안정성을 지원하므로 초기화에 사용한 클래스 대신 다른 클래스를 사용해 접근할 수 없음. <br>
자바의 JavaBean 패턴, 스칼라의 케이스 클래스 유형으로 정의된 클래스 지원

> JavaBean 패턴은 생성자에 너무 많은 파라미터가 존재할 때 대안으로 사용하는 패턴으로, 변수의 Setter 메소드를 정의해 매개변수를 전달하는 방식

<br>

Dataset은 필요한 경우에 선택적으로 사용할 수 있는 장점

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="3"></a>

## 3. 구조적 스트리밍

<br>

구조적 스트리밍을 사용하면 구조적 API로 개발된 배치 모드의 연산을 스트리밍 방식으로 실행할 수 있으며, 지연시간을 줄이고 증분 처리할 수 있다.<br>

배치 처리용 코드를 일부 수정하여 스트리밍 처리를 수행하고 값을 빠르게 얻을 수 있다는 장점<br>

프로토타입을 배치 잡으로 개발한 다음 스트리밍 잡으로 변환할 수 있음.<br>

1. 정적 DataFrame 생성

```python
# 1
staticDataFrame = spark\
.read\
.option('inferSchema', 'true')\
.option('header', 'true')\
.csv('./data/retail-data/by-day/*.csv')

# 2
staticDataFrame2 = spark.read.format('csv')\
.option('inferSchema', 'true')\
.option('header', 'true')\
.load('./data/retail-data/by-day/*.csv')
```

<br>

2. 테이블 생성

```python
staticDataFrame.createOrReplaceTempView('retail_data')
staticSchema = staticDataFrame.schema
```

<br>

3. 특정 고객이 대량으로 구매하는 영업 시간

```python
# 익스큐터 갯수 제한 5
spark.conf.set('spark.sql.shuffle.partitions', '5')

# window 함수는 집계 시에 시계열 컬럼을 기준으로 각 날짜에 대한 전체 데이터를 가지는 윈도우, 간격을 통해 처리 요건을 명시할 수 있음(날짜, 타임스탬프)
from pyspark.sql.functions import window, col

staticDataFrame\
.selectExpr(
'CustomerId',
'(UnitPrice * Quantity) as total_cost',
'InvoiceDate')\
.groupBy(
col('CustomerId'), window(col('InvoiceDate'), '1 day'))\
.sum('total_cost')\
.show(5)

# 결과
#+----------+--------------------+------------------+
#|CustomerId|              window|   sum(total_cost)|
#+----------+--------------------+------------------+
#|   13417.0|{2011-12-04 09:00...|            404.83|
#|   15358.0|{2011-12-05 09:00...| 830.0600000000003|
#|   15392.0|{2011-12-05 09:00...|304.40999999999997|
#|   15290.0|{2011-12-05 09:00...|263.02000000000004|
#|   16811.0|{2011-12-05 09:00...|             232.3|
#+----------+--------------------+------------------+
#only showing top 5 rows
```

<br>

4. 1 스트리밍 DataFrame 생성

```python
streamingDataFrame = spark.readStream\
.schema(staticSchema)\
.option('maxFilesPerTrigger', 1)\
.format('csv')\
.option('header', 'true')\
.load('./data/retail-data/by-day/*.csv')
```

4. 2 스트리밍 확인

```python
streamingDataFrame.isStreaming
# true
```

4. 3 지연연산

```python
purchaseByCustomerPerHour = streamingDataFrame\
.selectExpr(
'CustomerId',
'(UnitPrice * Quantity) as total_cost',
'InvoiceDate')\
.groupBy(
col('CustomerId'), window(col('InvoiceDate'), '1 day'))\
.sum('total_cost')
```

4. 4 액션 스트림 시작

```python
purchaseByCustomerPerHour.writeStream\
.format('memory')\
.queryName('customer_purchases')\
.outputMode('complete')\
.start()
```

4. 5 쿼리로 확인(더 많은 데이터를 읽을수록 테이블 구성이 바뀐다.)

```python
spark.sql('''
SELECT *
FROM customer_purchases
ORDER BY `sum(total_cost)` DESC
''')\
.show(5)
```

4. 6 console 출력

```python
purchaseByCustomerPerHour.writeStream\
.format('console')\
.queryName('customer_purchases_2')\
.outputMode('complete')\
.start()
```

<br>

> 메모리나 콘솔로 출력하는 방식과 파일별로 트리거를 수행하는 방식은 운영 환경에서 사용하는 것은 좋지 않다.

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="4"></a>

## 4. 머신러닝, 고급분석

<br>

내장된 MLlib을 사용해 대규모 머신러닝을 수행할 수 있다.
전처리 -> 멍잉 -> 모델학습 / 예측
구조적 스트리밍에서 예측하고자 할 때도 MLlib에서 학습시킨 다양한 예측 모델을 사용할 수 있다. <br>

> k-평균이란?
> k-평균은 데이터에서 k개의 중심이 임의로 할당되는 군집 알고리즘. 중심점(센트로이드[centroid])에 가까운 점들을 군집에 할당하고 할당된 점들의 중심을 계산

 <br>

```python
# SparkSession
from pyspark.sql import SparkSession
# 데이터 포맷, Col
from pyspark.sql.functions import date_format, col
# 스트링인덱서
from pyspark.ml.feature import StringIndexer
# one hot encoder
from pyspark.ml.feature import OneHotEncoder
# 데이터 타입 벡터
from pyspark.ml.feature import VectorAssembler
# 파이프라인
from pyspark.ml import Pipeline
# K means 라이브러리
from pyspark.ml.clustering import KMeans
# Cluster 수행 라이브러리
from pyspark.ml.evaluation import ClusteringEvaluator

spark = SparkSession\
        .builder\
        .appName('Python Spark SQL basic example')\
        .config('spark.some.config.option', 'some-value')\
        .getOrCreate()

sc = spark.sparkContext

# 정적데이터프레임
staticDataFrame = spark\
.read\
.option('inferSchema', 'true')\
.option('header', 'true')\
.csv('./data/retail-data/by-day/*.csv')

# 데이터 프레임 전처리
preppedDataFrame = staticDataFrame \
.na.fill(0)\
.withColumn("day_of_week", date_format(col("InvoiceDate"), "EEEE"))\
.coalesce(5)

# 학습 데이터 셋
trainDataFrame = preppedDataFrame\
.where("InvoiceDate < '2011-07-01'")

# 테스트 데이터 셋
testDataFrame = preppedDataFrame\
.where("InvoiceDate >= '2011-07-01'")

# 스트링인덱서 선언
indexer = StringIndexer()\
.setInputCol('day_of_week')\
.setOutputCol('day_of_week_index')

# 인코더 선언
encoder = OneHotEncoder()\
.setInputCol('day_of_week_index')\
.setOutputCol('day_of_week_encoded')

# 벡터
vectorAssembler = VectorAssembler()\
.setInputCols(['UnitPrice','Quantity','day_of_week_encoded'])\
.setOutputCol('features')

# 파이프라인 선언
transformationPipeline =Pipeline()\
.setStages([indexer, encoder, vectorAssembler])

# 학습 데이터 파이프라인
fittedPipeline = transformationPipeline.fit(trainDataFrame)

# 테스트 데이터 파이프라인
transformedTraining = fittedPipeline.transform(trainDataFrame)

# K means 모델 선언
kmeans = KMeans()\
.setK(20)\
.setSeed(1)

kmModel = kmeans.fit(transformedTraining)

# 모델 데이터 변환
predictionsTrain = kmModel.transform(transformedTraining)
evaluator = ClusteringEvaluator()
transformedTrain =  evaluator.evaluate(predictionsTrain)

# 결과
print("transformedTrain accuracy = " + str(transformedTrain))

centers = kmModel.clusterCenters()
print("Cluster Centers: ")
for center in centers:
    print(center)
```
