<a href="https://github.com/och5351/cluster#readme">메인으로</a>
<a id="home1"></a>

# NiFi 란?

<br>

"아파치 나이파이(Apache NiFi, 나이아가라파일즈/NiagaraFiles의 준말)는 소프트웨어 시스템 간 데이터 흐름을 자동화하도록 설계된 아파치 소프트웨어 재단의 소프트웨어 프로젝트이다. 과거에 NSA에 의해 개발되었고 2014년 기술 전송 프로그램의 일부로서 오픈 소스화된 나이아가라파일즈(NiagaraFiles) 소프트웨어에 기반을 둔다"

<br><br>

## 목차

- [NiFi 설치](#1)

<br><br>
<a id="1"></a>

# NiFi 설치

<br>

0. 설치 전 VM Network Setting (네트워크 2개)

```bash
 root@localhost > vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
 # onboot=no -> onboot=yes
 root@localhost > vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
 # bootproto=dhcp -> bootproto=none
 # onboot=no -> onboot=yes
 # IPADDR=192.168.56.104
 # PREFIX=24
 # GATEWAY=192.168.0.1
 # IPV6_PRIVACY=no

```

1. NiFi 다운 v1.12.0 & jdk 1.8 (현재 설치 기준 대로 하면 최근 Release 인 1.15.2 는 적용되지 않는다.)

<div align="center">
<a href="https://archive.apache.org/dist/nifi/1.12.0/nifi-1.12.0-bin.tar.gz">
https://archive.apache.org/dist/nifi/1.12.0/nifi-1.12.0-bin.tar.gz</a>
</div>

```bash
 root@localhost > yum update -y
 root@localhost > yum install -y wget java-1.8.0-openjdk-devel.x86_64
```

<br>

2. 설치 후 압축 해제 

<br>

```bash
 root@localhost > tar xvfz nifi-1.12.0-bin.tar.gz
```

3. NiFi에 Java Home 등록

<br>

```bash
 root@localhost > readlink -f /bin/javac
 # /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64/bin/javac
 root@localhost > vi nifi-1.12.0/bin/nifi-env.sh
 # #The java implementation to use.
 # #export JAVA_HOME=/usr/lib/java... => export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64/bin/javac
```

4. 방화벽 등록

<br>

```bash
 root@localhost > firewall-cmd --zone=public --add-port=8000/tcp --permanent
 # success
 root@localhost > firewall-cmd --zone=public --add-port=8080/tcp --permanent
 # success
 root@localhost > firewall-cmd --reload
 root@localhost > firewall-cmd --list-all
 #public (active)
 # target: default
 # icmp-block-inversion: no
 # interfaces: enp0s3 enp0s8
 # sources: 
 # services: dhcpv6-client ssh
 # ports: 8000/tcp 8080/tcp
 # protocols: 
 # masquerade: no
 # forward-ports: 
 # source-ports: 
 # icmp-blocks: 
 # rich rules:
```

<br>

5. NiFi 실행

<br>

```bash
 root@localhost > ./nifi-1.12.0/bin/nifi.sh start
```

<br>

6. Web 접속

<br>

```
 http://$NiFi_IP:8080/nifi
```

<br>

```bash
 # 포트 변경 시
 root@localhost > vi nifi-1.12.0/conf/nifi.properties
 # /http.port= 변경 
```

[목차로](#home1)
