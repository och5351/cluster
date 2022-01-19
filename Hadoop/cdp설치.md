<a href="https://github.com/och5351/cluster#readme">메인으로</a>
<a id="home1"></a>

# CDP?

<br>

하둡 및 에코 시스템들을 한번에 설치 할 수 있도록 구성한 클라우데라의 솔루션

<br><br>

## 목차

- [VM Setting](#1)
- [Linux setting(CentOS 7)](#2)
- [VirtualBox 하드디스크 용량 증설(CentOS7)](#3)
- [CDP 설치](#4)
- [Trouble Shooting](#5)

<br><br>
<a id="1"></a>

# VM Setting

<br>

1. CDP 가입 후 wget link 활성화<br>
   <a href="https://www.cloudera.com/downloads/cdp-private-cloud-trial/cdp-private-cloud-base-trial.html">CDP 다운로드</a>

<br>

2. [파일] -> [환경설정] -> [네트워크] -> [NAT 네트워크] -> [추가]

<div align="center">

<img src="https://user-images.githubusercontent.com/45858414/147914202-faeda3a9-b9fc-4ed4-91ad-20ab6c76d878.png" width="70%" height="70%" />

<img src="https://user-images.githubusercontent.com/45858414/147914623-94aaa0af-fae6-448a-9bf1-1c55958dcaa0.png" width="70%" height="70%" />

</div>

3. [파일] -> [호스트 네트워크 관리자] -> [네트워크] -> [만들기]

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147914306-6b34a65c-3350-4011-83f2-f381a36d0159.png" width="70%" height="70%">
</div>

4. 가상머신 만들기 메모리 8GB 설정

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147914117-a92c9c9a-56aa-4f63-8905-650912249166.png" width="70%" height="70%">
</div>

5. core 수 설정

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147914337-0e514901-7837-47f7-b65d-2e32ca24b720.png" width="70%" height="70%">
</div>

6. (어뎁터1)포트포워딩 설정

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915013-2f43967d-2b13-468d-b881-e3d458da3390.png" width="70%" height="70%" />
</div>

7. (어뎁터2) 설정

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915086-19140e01-3234-420d-9827-0333d920329e.png" width="70%" height="70%" />
</div>

<br>

[목차로](#home1)

<br><br>
<a id="2"></a>

# Linux setting(CentOS 7)

<br>

```bash
# cdp 계정 sudo 권한 부여(root 로그인)
root > useradd cdp
root > usermod -aG wheel cdp
root > sudo visudo
# 아래와 같이 주석 처리
```

```
## Same thing without a password
%wheel ALL=(ALL) NOPASSWD: ALL
```

```bash
root > su - cdp
cdp > sudo bash
# root로 접근이 되면 sudo 권한 받은 것

# IP 관련 설정
root > sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
```

```bash
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=a4d7c7a8-54ee-4ce0-8994-9932e2b6a8fc
DEVICE=enp0s8
ONBOOT=yes
IPADDR=192.168.56.101 # 102, 103
PREFIX=24
GATEWAY=192.168.56.1
IPV6_PRIVACY=no
```

```bash
# Hostname 설정
root > hostname # localhost.localhost
root > sudo hostnamectl set-hostname hadoop01.och.com
root > hostname # hadoop01.och.com

# Host 파일 수정
root > sudo vi /etc/hosts
192.168.56.101 hadoop01.och.com hadoop01
192.168.56.102 hadoop02.och.com hadoop02
192.168.56.103 hadoop03.och.com hadoop03

# 네트워크 재시작 및 테스트
root > sudo systemctl restart network
root > ping 8.8.8.8
PING 8.8.8.8(8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=39.3 ms
64 bytes from 8.8.8.8: icmp_seq=1 ttl=114 time=39.3 ms
... # Ping test

# 방화벽 해제 및 기타 매개변수 설정
root > sudo systemctl stop firewalld
root > sudo systemctl disable firewalld
root > vi /etc/selinux/config
SELINUX=disabled # 추가
root > sudo sysctl -w vm.swappiness=1
root > sudo vi /etc/sysctl.conf
vm.swappiness=1 # 추가

root > echo never > /sys/kernel/mm/transparent_hugepage/defrag
root > echo never > /sys/kernel/mm/transparent_hugepage/enabled
root > vi /etc/rc.local
echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo never > /sys/kernel/mm/transparent_hugepage/enabled

root > sudo systemctl restart network

root > sudo yum update -y
root > reboot
```

```bash
cdp > wget http~ # Cloudera 에서 활성화 시킨 링크 wget
cdp > chmod u+x cloudera-manager-installer.bin
cdp > cloudera-manager-installer.bin
# 이 후 화면들 설치 진행
```

<br>

[목차로](#home1)

<br><br>
<a id="3"></a>

# VirtualBox 하드디스크 용량 증설(CentOS7)

<br>

1. 버추얼박스가 설치된 위치(경로) 확인 (보통 C:\Program Files\Oracle\VirtualBox)
2. 용량을 조절할 가상머신 파일 위치(경로) 확인(가상머신이름.vdi 또는 vdmk)
3. CMD창에서 버추얼박스가 설치된 위치로 이동

   1. 윈도우에서 단축키 [Win Key] + [R]
   2. cmd 라고 입력 후 엔터(Enter)
   3. cd C:\Program Files\Oracle\VirtualBox

<br>

4. CMD창에서 용량 재조절 명령 \*\* 명령 실행 전, 가상머신과 VirtualBox를 꺼야한다.

다음 명령어를 실행한다.

- 가상머신 파일 위치 경로가 다르면 경로까지 작성

VboxManage modifyhd 파일명.vdi --resize 원하는용량

- 1기가로 조절 예) VboxManage modifyhd testlinux.vdi --resize 1024
- 10기가로 조절 예) VboxManage modifyhd testlinux.vdi --resize 10240
- 50기가로 조절 예) VboxManage modifyhd testlinux.vdi --resize 51200

> 메가바이트 단위로 쓴다. 50G = 1024M \* 50 = 51200

명령어를 실행하게 되면, 저장소 크기가 바뀐 것을 VirtualBox에서 확인할 수 있다.

5. 해당 가상머신에 접속
6. 파티션 설정

- 리눅스(Centos 7)의 경우

1. 용량 확인

```bash
df -h
```

> 가상 하드 디스크 자체는 커졌지만 하드 디스크 내의 파티션은 그대로이기 때문에 추가된 디스크의 영역까지 파티션 또한 늘려주어야 한다.

<br>

2. 디스크 파티션 테이블 확인 및 설정

```bash
fdisk -l

fdisk /dev/sda
```

순서대로 다음을 입력한다.

```
d  ( 파티션 삭제 )

2  ( 기존 파티션 중 부팅 디스크가 아닌 두번째 파티션 삭제. /dev/sda2 삭제 )

n  ( 새로운 파티션 생성 )

p  ( Primary 파티션 선택 )

2  ( 2번 파티션 지정 )

[엔터]  ( 실린더 처음 값 default )

[엔터]  ( 실린더 맨 마지막 값 default )

w       ( 변경사항 기록 )
```

<br>

3. 재부팅하여 적용

```bash
reboot
```

<br>

4. 물리 볼륨 조절

```bash
pvresize /dev/sda2

pvscan  (반영여부 확인 )
```

<br>

5. 논리 볼륨 공간 추가

```bash
lvextend -l +100%FREE /dev/mapper/centos-root
```

( 증설되어 사용가능해진 저장소 용량을 논리볼륨에 매핑한다. 이때 사용가능한 모든 용량 매핑 (100%FREE) )

<br>

6. 논리 볼륨 조절

```bash
lvresize -r -l +100%FREE /dev/mapper/centos-root
```

( 수정내용 반영 )

<br>

7. 확인

```bash
df -h
```

<br>

[목차로](#home1)

<br><br>
<a id="4"></a>

# CDP 설치

<br>

1. admin/admin 로그인 후 클러스터 이름 설정 (http://192.168.56.101:7180/ 접속)

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915603-5abc6258-e482-4a1d-b5a3-266b99ca2aee.png" width="70%" height="70%" />
</div>

2. 지정한 hostname을 전부 적어주기

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915677-d8fdaccd-1bc5-4ae6-ba36-55053b90ccf5.png" width="70%" height="70%" />
</div>

3. 다음

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915740-586e3c47-b19a-40d2-9fcd-37c2b87d003e.png" width="70%" height="70%" />
</div>

4. JDK 설치 cloudera에서 권장하는 버전을 설치한다.

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915790-9683e148-e445-4037-9946-afbbc2983395.png" width="70%" height="70%" />
</div>

5. 로그인 정보를 적어준다. root/1234

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915832-7efd3585-7715-4d6e-b656-cb0aff05279f.png" width="70%" height="70%" />
</div>

6. 각 노드들에 CDP 설치

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915875-bfce794b-6e2d-494d-ac8f-b145c99a989d.png" width="70%" height="70%" />
</div>

7. Pracels 설치

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147915992-b3b81968-bd62-47e7-87a2-acde9aa98132.png" width="70%" height="70%" />
</div>

8. Inspect

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147917299-d4f694c2-d273-4fda-8cf1-b0b425bc17fc.png" width="70%" height="70%" />
</div>

9. 사용자 정의 설치

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147917580-0c431a40-a514-4958-9a5c-c73b70173399.png" width="70%" height="70%" />
</div>

10. 데이터베이스 설정

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147918042-faad190b-3f84-4846-b12e-ea84c449439a.png" width="70%" height="70%" />
</div>

<br>

[목차로](#home1)

<br><br>
<a id="5"></a>

# Trouble Shooting

<br>

1. Java heap 영역 공간

Service monitor를 실행시키는 도중

```
JAVA_HOME=/usr/java/jdk1.8.0_232-cloudera
CONF_DIR=/var/run/cloudera-scm-agent/process/1546333941-cloudera-mgmt-SERVICEMONITOR
CMF_CONF_DIR=
Removing any leveldbjni library files left over from previous runs
Executing: /usr/java/jdk1.8.0_232-cloudera/bin/java -server -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -Dmgmt.log.file=mgmt-cmf-mgmt-SERVICEMONITOR-hadoop02.och.com.log.out -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -Dsun.security.krb5.disableReferrals=true -Djdk.tls.ephemeralDHKeySize=2048 -Dfirehose.schema.dir=/opt/cloudera/cm/schema -XX:PermSize=128m -Dsun.rmi.transport.tcp.handshakeTimeout=10000 -Dsun.rmi.transport.tcp.responseTimeout=10000 -Dlibrary.leveldbjni.path=/run/cloudera-scm-agent/process/1546333941-cloudera-mgmt-SERVICEMONITOR -Xms320864256 -Xmx320864256 -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/mgmt_mgmt-SERVICEMONITOR-e6fcf51707319d12dae18829ba0ac0b1_pid5441.hprof -XX:OnOutOfMemoryError=/opt/cloudera/cm-agent/service/common/killparent.sh -Dsun.security.krb5.disableReferrals=true -Djdk.tls.ephemeralDHKeySize=2048 -cp /run/cloudera-scm-agent/process/1546333941-cloudera-mgmt-SERVICEMONITOR:/usr/share/java/mysql-connector-java.jar:/opt/cloudera/cm/lib/postgresql-42.2.14.jre7.jar:/usr/share/java/oracle-connector-java.jar:/opt/cloudera/cm/lib/*: com.cloudera.cmon.firehose.Main --pipeline-type SERVICE_MONITORING --mgmt-home /opt/cloudera/cm
java.lang.OutOfMemoryError: Java heap space
Dumping heap to /tmp/mgmt_mgmt-SERVICEMONITOR-e6fcf51707319d12dae18829ba0ac0b1_pid5441.hprof ...
Dump file is incomplete: 장치에 남은 공간이 없음
```

<br>

[목차로](#home1)
