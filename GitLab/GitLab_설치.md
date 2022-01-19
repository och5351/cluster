<a href="https://github.com/och5351/cluster#readme">메인으로</a>
<a id="home1"></a>

# GitLab 이란?

<br>

GitLab은 이슈, 코드 리뷰, CI/CD를 단일 UI로 통합해서 볼 수 있는 오픈소스 프로젝트

설치형 Github라는 컨셉으로 시작된 프로젝트이기 때문에 Github와 비슷함
서비스형 원격 저장소를 운영하는 것에 대한 비용이 부담되거나 소스코드의 보안이 중요한 프로젝트에 적당

<br><br>

## 목차

- [GitLab 설치](#1)
- [Root 비밀번호 분실 시](#2)

<br><br>
<a id="1"></a>

# GitLab 설치

<br>


> 환경 : VirtualBox, CentOS7, gitlab-ce

<br>

1. 초기 세팅

```bash
 root@localhost > vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
 # onboot=no -> onboot=yes
 root@localhost > vi /etc/sysconfig/network-scripts/ifcfg-enp0s8
 # bootproto=dhcp -> bootproto=none
 # onboot=no -> onboot=yes
 # IPADDR=192.168.56.107 # 원하는 IP
 # PREFIX=24
 # GATEWAY=192.168.0.1
 # IPV6_PRIVACY=no
 root@localhost > sudo systemctl restart network
 root@localhost > yum update -y
```
<br>

VM Portfowarding은 호스트 IP = localPC, 게스트 IP = enp0s3 ip(Private ip)

Port : 80, 443, 25

<br>

2. 의존성 설치

```bash
 root@localhost > sudo apt install -y ca-certificates curl openssh-server
```


<br>

3. gitlab ce repository 추가

```bash
 root@localhost > curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash

 # The repository is setup! You can now install packages.
 # 위 메시지 출력 확인
```

<br>

4. gitlab 설치

```bash
 root@localhost > yum install gitlab-ce
```

<br>

5. gitlab 설정 변경 및 configuration

```bash
 root@localhost > sudo vi /etc/gitlab/gitlab.rb

 # external_url=http://$enp0s3_ip 로 변경
 root@localhost > sudo gitlab-ctl reconfigure
```

<br>

6. gitlab 접속

http://$enp0s3_ip 로 접속 <br>
id = root / password = 설정할 패스워드 입력

[Root 비밀번호 분실 시](#2)

<br>

[목차로](#home1)

<br><br>
<a id="2">
</a>

# Root 비밀번호 분실 시

<br>

1. 서버 접속

<br>

2. gitlab-rails 콘솔 실행

```bash
 root@localhost > gitlab-rails console -e production
```

<br>

3. User 정보 획득

```bash
 gitlab-rails > user = User.where(id: 1).first
```

<br>

4. User paswword 변경

```bash
 # 반드시 8자리 이상 비밀번호 설정
 gitlab-rails > user.password='12345678' # 변경할 비밀번호 ' 기입
 gitlab-rails > user.password_confirmation='12345678' # 비밀번호 확인
```

<br>

5. User 정보 저장

```bash
 gitlab-rails > user.save
 # true
 # false 시 비밀번호 8자리 확인
```
<br>

[목차로](#home1)