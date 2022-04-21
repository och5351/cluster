<a href="https://github.com/och5351/cluster#readme">메인으로</a>

# Sqoop

Sqoop (SQL for Hadoop)은 관계형 데이터베이스(RDB)와 분산 파일 시스템(HDFS) 사이의 양방향 데이터 전송을 위해 설계된 툴이다. 오직 두 포인트 사이의 데이터 통신을 쉽게 다루기 위해서 개발된 프로젝트.

<a id="home1"></a>

## 목차

- [1. Sqoop](#1)
- [2. Sqoop 설치](#2)
- [3. 스쿱 커넥터](#3)
- [4. Sqoop Import](#4)
- [5. 생성된 코드](#5)
- [6. Import 상세](#6)
- [7. 데이터 작업](#7)

<br>

<a id="1"></a>

## 1. Sqoop

하둡 플랫폼의 강력함은 다양한 데이터 타입을 다룰 수 있다는데 있다. 하지만 HDFS 외부의 스토리지 저장소에 있는 데이터에 접근하려면 맵리듀스 프로그램은 외부 API를 이용해야 한다. 회사의 주요 데이터는 RDBMS와 같은 구조적인 데이터 저장소에 주로 저장되며, 아파치 스쿱은 구조화된 데이터 저장소에서 데이터를 추출해서 하둡으로 보내 처리할 수 있도록 해주는 오픈 소스 도구다.

<br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. Sqoop 설치

<br>

> 스쿱 2는 스쿱 1의 구조적 한게를 해결하기 위해 개발. 스쿱 1은 명령행 도구고 자바 API를 제공하지 않았기 때문에 다른 프로그램에 내장하기 어려움. 또한 스쿱 1의 모든 커넥터는 모든 출력 포맷에 대한 정의를 포함해야 하므로 새로운 커넥터를 작성하는 것은 매우 어려움. 스쿱 2는 잡을 실행하는 서버 컴포넌트 뿐만 아니라 명령행 인터페이서(CLI), 웹 UI, REST API, 자바 API 등 다양한 클라이언트 제공.

> 스쿱 1은 안정적인 릴리즈 지원한다. 스쿱2의 개발은 매우 활발하나 스쿱 1과 비교했을 때 부족한 점이 일부 있다.

<br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="3"></a>

## 3. Sqoop 커넥터

<br>

스쿱 커넥터는 이 프레임워크를 사용하여 스쿱이 임포트와 익스포트하게 해주는 모듈식 컴포넌트다. 스쿱은 MySQL, PostgreSQL, 오라클, SQL 서버, DB2, 네티자 등 다양한 관계형 데이터베이스에서 작동하는 커넥터를 제공한다. 또한 자바의 JDBC 프로토콜을 지원하는 어떤 데이터베이스에도 연결할 수 있도록 제네릭 JDBC 커넥터도 제공한다. couchbase 같은 NoSQL 저장소에서 대용량 전송이 가능하며, 내장 커넥터에는 없으므로 따로 내려 받아서 사용한다.

<br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="4"></a>

## 4. Sqoop Import

<br>

스쿱의 import 도구는 맴리듀스 잡을 실행하여 대상 데이터베이스에 접속하고 테이블을 읽는다. 일반적으로 임포트 처리 성능을 향상시키기 위해 맴 테스크 4 개를 병렬로 실행할 것을 권장한다. 각 테스크는 임포트된 결과를 지정된 디렉터리 아래에 각각 다른 이름의 파일로 저장.

<br>

기본적으로 스쿱은 임포트한 데이터로 콤마로 구분된 텍스트 파일을 생성한다. 컬럼 구분자는 명시적으로 지정할 수 있으며, 필드 내용을 구분하는 데 필드 포함 및 구분 문자를 사용할 수 있다.

<br>

스쿱은 텍스트 파일 외에 다른 파일 포맷으로도 임포트한 데이터를 저장할 수 있다. 기본인 텍스트 파일은 사람이 판독할 수 있는 형태로 데이터를 표현하며, 플랫폼 독립적이고, 단순한 구조를 지니고 있다. 하지만 텍스트 파일은 바이너리 필드(데이터베이스의 VARBINARY 컬럼)를 가질 수 없고, (null 값의 표현을 제어하는 --null-string 임포트 옵션을 사용하더라도) null 값과 문자열 "null" 값을 갖는 문자열 필드를 구분할 수 없다.

<br>

이를 해결하기 위해 스쿱은 시퀀스 파일, 에이브로 데이터 파일, 파케이 파일을 지원한다. 이런 바이너리 포맷은 임포트 된 데이터를 가장 정확하게 표현해준다. 또한 데이터를 압축해서 저장하면서도, 동일한 파일의 다른 영역을 병렬 처리하는 맵리듀스 기능을 그대로 사용할 수 있다. 그러나 스쿱의 현재 버전은 에이브로 데이터 파일 또는 시퀀스 파일을 하이브에 로드할 수 없다. 에이브로 데이터 파일은 수동으로 하이브에 로드할 수 있으며, 파케이 데이터 파일은 스쿱을 사용해서 하이브에 바로 로드할 수 있다.

> 시퀀스 파일 : 자바 기반임.

> 에이브로 데이터 파일 : 다양한 언어로 처리 가능.

> 파케이 파일 : 다양한 언어로 처리 가능.

<br>
<div align="right">

[목차로](#home1)

</div><br><br>
<a id="5"></a>

## 5. 생성된 코드

<br>

데이터베이스 테이블의 내용을 HDFS에 저장하는 것 외에도 스쿱은 로컬 디렉터리에 자바 소스(widgets.java)을 생성한다. 스쿱은 HDFS에 쓰기 전에 데이터베이스 원본의 특정 테이블 데이터를 역직렬화하기 위해 미리 생성된 자바 코드를 사용한다.
생성된 클래스(widgets)는 임포트된 테이블에서 가져온 레코드를 유지할 수 있다. 이 클래스는 맵리듀스에서 레코드를 처리하거나 레코드를 HDFS에 시퀀스 파일로 저장할 대 사용된다. 시퀀스 파일에 저장하는 임포트 과정에서 스쿱은 생성된 클래스를 사용해서 시퀀스 파일의 키-값 쌍 포맷의 '값' 항목에 임포트한 각 행을 저장한다.

<br>

```bash
# 예
% sqoop codegen --connect jdbc:~~~ \
> --table widgets --class-name Widget
```

codegen 은 import 하지 않고 코드만 생성.
Widget 클래스 생성 후 Widget.java 파일에 기록.

<br>

스쿱은 현재 에이브로 기반의 직렬화와 스키마 생성을 지원하므로, 생성된 코드를 통합하는 과정을 굳이 거치지 않아도 프로젝트에서 스쿱을 사용할 수 있다.

<br>
<div align="right">

[목차로](#home1)

</div><br><br>
<a id="6"></a>

## 6. Import 상세

<br>

스쿱은 테이블에서 행을 추출하는 맵리듀스 잡을 실행하여 데이터베이스에서 테이블을 임포트하고 HDFS에 그 레코드를 기록한다.

<br>

스쿱도 하둡처럼 자바로 작성되어있으며, JDBC API를 활용하여 RDBMS에 저장된 데이터에 접근할 수 있도록 한다.

> 대부분의 데이터베이스 벤더는 JDBC API, 자사 데이터베이스 서버에 접속하는 데 필요한 코드가 포함된 JDBC 드라이버를 제공.

<br>
<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/163817786-43572dd3-ae45-4c47-a86a-94bcae9d6ad0.png' height='70%' weight='70%' />
</div>

> Scoop은 DB에 접속할 때 사용된 접속 문자열의 URL을 기반으로 어떤 드라이버를 로드할지 예측한다. 필요한 JDBC 드라이버를 내려받아 스쿱 클라이언트에 미리 설치해야 한다. 스쿱이 어떤 JDBC 드라이버가 적합한지 모를 때는 --driver 인자에 명시적으로 지정해줘야 한다.

<br>

Import 순서

1. 임포트를 시작하기 전에 JDBC로 임포트 대상 테이블 검사(모든 컬럼의 목록과 SQL 데이터 타입 추출)
2. SQL 데이터 타입(VARCHAR, INTEGER 등)을 맵리듀스 어플리케이션의 필드에 적용될 자바 자료형(String, Integer)으로 매핑
3. 스쿱의 코드 생성기는 이 정보를 이용하여 테이블에서 추출한 레코드에 적용할 테이블 특화 클래스 생성

<br>

ex. Widget Class 레코드의 각 컬럼 호출 메소드 예제
<br>

<b>Java 레코드 호출 메소드</b>

```Java
public Integer get_id();
public String get_widget_name();
public java.math.BigDecimal get_price()
public java.sql.Date get_design_date();
public Integer get_version();
public String get_design_comment();
```

<br>
<b>Widget 클래스를 위한 DBWritable 인터페이스 직렬화 메소드</b>

```Java
public void readFields(ResultSet __dbResults) throws SQLException;
public void write(PreparedStatement __dbStmt) throws SQLException;
```

<br>

- JBDC의 ResultSet 인터페이스는 쿼리에서 레코드를 하나씩 추출하는 커서 제공.
- readFields() 메소드는 ResultSet 데이터의 한 행에 있는 컬럼을 읽어서 Widget 객체의 필드 값을 채운다.
- write() 메소드로 새로운 Widget 행을 테이블에 추가하며 이러한 과정을 exporting 이라고 한다.
- 스쿱이 구동시킨 맵리듀스 잡은 JDBC를 통해 데이터베이스의 테이블을 읽을 때 InputFormat 을 이용.
- 하둠에 포함된 DataDriveDBInputFormat은 쿼리 결과를 여러 개의 맵 테스크로 나눈다.

<br>

테이블을 읽을 때 다음과 같음

```SQL
SELECT COL1, COL2, COL3, ... FROM TABLENAME
```

쿼리를 다수의 노드에 분산시키면 import 성능을 높일 수 있는데, 쿼리를 분리하기 위해 분할 기준 컬럼(splitting column) 이 중요.
메타데이터 정보를 기반으로 분할에 적합한 컬럼 예상하며 테이블에 PK가 있으며 이 컬럼을 분할 기준 컬럼으로 사용.(기본 키 컬럼의 최댓값과 최솟값을 조회한 후 테스크 수를 기준으로 각 맵 테스크가 수행할 쿼리를 정함.)

<br>

ex. Scoop MapReduce

예로 100,000개의 Entry 가 있는 widgets 테이블이 있다고 가정하였을 때, 이 테이블에는 0 ~ 99,999까지의 값이 있는 id 컬럼이 있다. 이 테이블을 import 할 때 스쿱은 id를 테이블의 기본 키 컬럼으로 결정.

MapReduce job을 시작할 때 import를 수행하는 데 사용되는 DataDrivenDBInputFormat은 SELECT MIN(id), MAX(id) FROM widgets와 같은 쿼리문을 수행한다. 이 쿼리의 결괏값으로 전체 데이터의 범위를 산정할 수 있다. 병렬로 실행하는 맵테스크 수를 다섯 개로(-m 5) 지정했다면 각 맵 테스크는 아래와 같은 쿼리를 수행.

```SQL
SELECT id, widget_name, ...
FROM widgets
WHERE id >= 0
AND id < 20000

SELECT id, widget_name, ...
FROM widgets
WHERE id >= 20000
AND id < 40000 -- 20000개 씩 split
```

<br>

병렬 작업을 효율적으로 수행하기 위한 핵심은 분할 컬럼을 선택하는 것.
사용자가 import job을 실행할 때 데이터의 실제 분포를 바영할 수 있도록 특정 분할 컬럼을 지정할 수 있다.(--split-by 인자, -m 1의 경우 단인 테스크로 실행하며 분할 과정 없음).

<br>

- import 제어하기

  스쿱으로 전체 테이블을 한 번에 import 할 수 도 있고 테이블의 일부 컬럼만 지정할 수도 있다. -where 인자에 WHERE 절을 지정하면 원하는 행의 범위를 제한할 수 있다. 예로 0~99,999까지 위젯을 임포트했고, 이 달에 1,000개의 위젯이 추가되었다면 WHERE id >= 100,000 절에 지정하여 import 하면 된다. 컬럼 변환과 같은 세밀한 제어를 수행할 때는 --query 인자로 지정하면 된다.

<br>

- import와 일관성

  데이터베이스를 조회하는 맵 테스크는 독립된 프로세스에서 병렬로 실행되므로 데이터베이스의 단일 트랜젝션을 공유할 수 없다. 이러한 데이터 일관성 문제를 해결하는 가장 좋은 방법은 import 를 수행하는 동안 테이블의 기존 행을 수정하는 포르세스의 실행을 막는 것 이다.

<br>

- 증분 import

  HDFS에 있는 데이터와 데이터베이스에 저장된 데이터의 동기화를 유지하기 위해 주기적으로 import 작업을 수행한다. 이러한 작업이 가능하려면 새로운 데이터를 확인할 필요가 있다. Sqoop 은 --check-column으로 지정된 컬럼의 값이 특정 값(--last-value로 지정한)보다 큰 행을 import 하는 기능을 제공한다.
  <br>
  --last-value 인자로 지정한 값은 계속 증하는 행의 ID와 같다. MySQL의 AUTO_INCREMENT로 지정한 기본 키 컬럼이 좋은 예다. 이런 방식은 데이터베이스 테이블에 새로운 행이 계속 추가되지만 기존 행의 값이 변경되지 않을 때 적합하다. 이런 모드를 추가모드(append mode)라고 하며 --incremental append 옵션을 지정하여 사용할 수 있다. 다른 옵션은 시간을 기반으로 증분 임포트(--incremental lastmodified를 지정해서)를 수행하는 것으로, 기존 행의 값이 변경될 수 있고 해당 레코드에 최종 수정 시간을 기록하는 컬럼(체크 컬럼)이 있을 때 적합한 방식이다.
  <br>
  증분 임포트를 실행하면 Sqoop은 출력의 마지막 부분에 다음 import 에서 --last-value를 지정할 값을 보여준다. 물론 이 값은 증분 import를 수동으로 수행할 때 유용하며, 주기적으로 import 를 실행할 때는 Sqoop의 job 저장 기능을 활요하는 것이 좋다. 이 기능은 마지막 값을 자동으로 저장하고 다음 job을 수행할 때 그 값을 이용한다. shell에서 sqoop job -help를 입력하면 job 저장 기능에 대한 자세한 설명을 볼 수 있다.

<br>

- 직접 모드 import

  스쿱은 import를 수행하는 다양한 전략을 선택할 수 있는 구조로 되어 있다. 대부분의 데이터베이스는 앞에서 언급한 DataDriveDBInputFormat 기반의 접근 방법을 사용한다. 일부 데이터베이스는 데이터를 매우 빨리 추출할 수 있는 특정 도구를 제공하기도 한다. MySQL의 mysqldump 어플리케이션은 JDBC 채널보다 훨씬 빨리 테이블을 읽을 수 있다.
  <br>
  직접모드는 JDBC 방식과 같은 범용이 아니기 때문에 사용자가 직접 --direct 인자로 지정해주어야 한다. MySQL의 직접 모드는 CLOB나 BLOB 컬럼과 같은 대형 객체를 처리할 수 없기 때문에 주의해야 한다(Sqoop은 대형 객체 컬럼의 데이터를 HDFS에 저장할 때 JDBC 기반 API를 이용하기 때문).
  <br>
  이러한 도구를 제공하는 데이터베이스는 Sqoop의 성능을 크게 높일 수 있다. MySQL 의 직접 모드 import는 JDBC 기반의 import보다 효율이 높다(Map task 와 걸리는 시간 측정에서). 직접 모드도 다수의 Map Task를 병렬로 실행한다. 각 테스크는 mysqldump 프로그램의 인스턴스를 생성하고 그 결과를 읽는다. MySQL 외에 PostgreSQL, 오라클, 네티자도 직접 모드 import를 지원한다.

  데이터베이스의 내용에 접근하는 데 직접 모드를 사용하더라도 메타데이터 정보는 JDBC를 통해 조회된다.

<br>
<div align="right">

[목차로](#home1)

</div><br><br>
<a id="7"></a>

## 7. 데이터 작업

<br>

<br>
<div align="right">

[목차로](#home1)

</div><br><br>
<a id="8"></a>

## 8. ㅁㅁㅁ
