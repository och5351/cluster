<a href="https://github.com/och5351/cluster#readme">메인으로</a>
<a id="home1"></a>

# Oracle Client 설치 & 적용

<br>

## 목차

- [Oracle Client 설치](#1)

<br>
<a id="1"></a>

# Docker registry 설치

<br>

1. Oracle Instant Client RPM 다운

    <a href="https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html">오라클 인스턴스 클라이언트</a>

* oracle-instantclient-basic-버전.x86_64.rpm
* oracle-instantclient-devel-버전.x86_64.rpm
* oracle-instantclient-sqlplus-버전.x86_64.rpm


<br>

2. 설치

``` bash
yum install -y oracle-instantclient-basic-버전.x86_64.rpm
yum install -y oracle-instantclient-devel-버전.x86_64.rpm
yum install -y oracle-instantclient-sqlplus-버전.x86_64.rpm
```

<br>

3. PATH 지정

```bash
vi /etc/bashrc # or /etc/profile

# /etc/bashrc

ORACLE_HOME=/usr/lib/oracle/11.2/client64
TNS_ADMIN=/etc/oracle
CFLAGS=-I/usr/include/oracle/11.2/client64
LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
NLS_LANG=KOREAN_KOREA.KO16MSWIN949
PATH=$ORACLE_HOME/bin:$PATH
export TNS_ADMIN LD_LIBRARY_PATH PATH CFLAGS NLS_LANG ORACLE_HOME

```

<br>

4. tnsnames.ora 작성

```bash
ORC01=  # 원하는 이름
    (DESCRIPTION = 
        (ADDRESS_LIST = 
            (ADDRESS = (PROTOCOL = TCP)(HOST = XXX.XXX.XXX.XXX)(PORT = XXXX)) 
        ) 
        (CONNECT_DATA = 
            (SID = "") 
        ) 
    ) 
```
<br>

5. ORACLE 접속

```bash
ID/PASSWD@ORC01
```


[목차로](#home1)