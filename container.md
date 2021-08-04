<a id="home1"></a>

# 컨테이너란?

하나의 리눅스 프로세스가 마치 전용 서버에서 동작하고 있는 것 같은 분리 상태이며, 리눅스 커널의 네임스페이스와 컨트롤 그룹(cgroup)이라는 기술을 기반으로 한다.

<br><br>

## 목차

- [컨테이너와 가상 서버의 차이점](#1)
- [컨테이너의 장점](#2)
- [도커 아키텍처](#3)
- [도커 데몬](#4)
- [도커 클라이언트](#5)
- [이미지](#6)
- [컨테이너](#7)
- [도커 레지스트리](#8)


<br><br>

<a id="1"></a> 

# 컨테이너와 가상 서버의 차이점
<br>
<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">특징</th>
            <th colspan="1">가상 서버</th>
            <th colspan="1">컨테이너</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>이미지 크기(CentOS 7.4의 경우)</td>
            <td>최소 1.54GB</td>
            <td>최소 0.20GB</td>
        </tr>
        <tr>
            <td>메모리 사용량</td>
            <td>기본 640MB</td>
            <td>기본 512MB</td>
        </tr>
        <tr>
            <td>벤치 마크 성능 비교</td>
            <td>65%(Xen HVM 가상 서버)</td>
            <td>90%</td>
        </tr>
        <tr>
            <td>OS 기동 시간</td>
            <td>분 단위</td>
            <td>초 단위</td>
        </tr>
    </table>
</thead>
<br><br>

<img src="https://user-images.githubusercontent.com/45858414/128109064-84d5312a-7640-4d85-9e60-9d674785ba56.png" width="70%" height="70%"/>
</div>
<br><br>

* Windows나 Mac에서 컨테이너를 돌리려면 리눅스 커널을 위해 가상 서버 필요(Hyper-V[windows10], HyperKit[Mac] 위에서 LinuxKit 기동).
* LinuxKit 은 컨테이너를 실행하기 위한 경량의 리눅스 서브 시스템으로 도커(Docker), IBM, 리눅스 파운데이션(Linux Foundation), 마이크로소프트(Microsoft), ARM, 휴렛 패커드(Hewlett Packard), 인텔(Intel)과 같은 회사가 만듬
* 컨테이너는 하이버바이저상의 가상 서버에서도 사용할 수 있어 퍼블릭 클라우드의 '가상 서버'나 온프레미스의 Openstack 위에서 많이 활용 됨
<br><br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128111801-372e29b2-5b3d-4ac5-8800-7528aa0580cb.png" width="70%" height="70%"/>

</div>
<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>



<a id="2"></a> 

# 컨테이너의 장점

<br><br>

인프라의 사용률 향상
---

* 하나의 물리서버나 가상 서버 위에서 여러 개의 컨테이너를 돌릴 수 있다.
* CPU와 메모리 사용률을 높여 하드웨어를 효율적을 이용할 수 있다.

<br><br>

빠른 기동 시간
---

* 컨테이너의 기동 시간은 가상 서버나 물리 서버의 기동 시간보다 훨씬 빠르다.
* 운영체제, 애플리케이션, 미들웨어 등 다양한 이미지를 쉽게 얻을 수 있다.
* 설치 작업이나 설정 작업이 줄어든다.
* 네트워크, 볼륨(외부 저장)을 소프트웨어 정의 오브젝트로 작성할 수 있다.

<br><br>

불변 실행 환경(Immutable Infrastructure)
---

* 애플리케이션 실행에 필요한 소프트웨어를 모두 포함하여 컨테이너를 작성할 수 있다.
* 컨테이너를 조합하여 시스템을 구성함으로써 특징 서버 환경에 대한 종속성을 배제할 수 있따.
* 개발 환경과 운영 환경의 차이를 줄일 수 있다.
<div align="right"> 

[목차로](#home1) 
</div><br><br><br>

<a id="3"></a> 

# 도커 아키텍처

<br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128115987-5ddec64e-f51c-49a4-8099-a1c33eacd93d.png" width="70%" height="70%"/>
</div>
<br><br>

* 리눅스 커널이 제공하는 기능을 활용하면 자체적 컨테이너를 만드는 것이 가능하나, 재사용이나 공유가 어렵다.
* 도커는 Build(작성), Ship(이동), Run(실행) 기능 지원
* 도커는 도커 데몬 서버와 클라이언트인 도커 커맨드, 이미지 보관소인 레지스트리로 구성



<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# 도커 데몬

* 클라이언트인 도커 커맨드의 명령을 받아들여서 도커 오브젝트인 이미지, 컨테이너, 볼륨, 네트워크 등을 관리
* 네트워크 너머에 있는 원격 클라이언트로부터 요청을 받는 것 가능

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 도커 클라이언트

도커 커맨드는 컨테이너를 조작하는 커맨드 라인 유저 인터페이스로 도커 데몬의 클라이언트다.
도커 커맨드는 도커 API를 사용하여 도커 데몬에 요청을 보낸다.

```
    1. docker build : 베이서 이미지에 기능을 추가하여 새로운 이미지를 만들 때 사용
    2. docker pull : 레지스트리에서 이미지를 로컬에 다운로드할 때 사용
    3. docker run : 이미지를 바탕으로 컨테이너를 실행

```
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a>

# 이미지

이미지는 읽기 전용인 컨테이너의 템플릿을 말한다.(컨테이너를 기동하기 위한 실행 파링과 설정 파일의 묶음)
컨테이너를 실행하면 이미지에 담긴 미들웨어나 애플리케이션이 설정에 따라 기동한다.

예시 ) Jenkins 컨테이너

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128120835-f87d1327-72a4-4f07-9e64-6a4bc97b3727.png" width="70%" height="70%"/>
</div>
<br><br>


* 대부분의 이미지는 다른 이미지에 기반하여 만들어진다. (웹 서버인 Nginx의 컨테이너는 리눅스 배포판 중 하나인 데이안에 기반하여 만들어 짐)
* 이미지를 만들 때는 기반 이미지와 설치 스크립트 등을 Dockerfile에 기재하여 빌드한다.
<br>

예시 ) Nginx 컨테이너

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128124729-89f1a80f-ad42-4b76-8e46-b0e19aeb8bec.png" width="70%" height="70%"/>
</div>
<br>
1. docker build 명령어를 통해 읽기
2. 기반이 되는 데비안 이미지가 로컬에 없으면 레지스트리로 부터 다운
3. 데비안 이미지를 컨테이너로 기동
4. Nginx 패키지를 설치하고 설정 파일을 추가
5. 새로운 이미지 Nginx로 로컬 리포지터리에 저장

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"></a>

# 컨테이너

컨테이너는 하나의 프로세스라고 불 수 있다.
리눅스의 네임스페이스나 컨트롤 그룹(cgroups)을 통해 다른 프로세스들과 완전히 분리되어 실행되는 프로세스이다.
그러나 컨테이너는 정지된 상태로도 관리되기 때문에 보다 명확하게 표현하자면 '실행 가능한 이미지의 인스턴스' 라고 할 수 있다.

* 'docker run' 명령어를 통해 이미지는 컨테이너로 변환되어 하나의 인스턴스가 된다. (하나의 IP 주소를 가지는 독립된 서버처럼 동작)
* 'docker stop' 혹은 'docker kill' 명령어를 통해 정상 종료 혹은 강제 종료를 할 수 있다. 정지 상태는 삭제된 상태가 아니다.
* 'docker rm' 에 의해 지워지기 전까지 기동했을 때의 샐행 옵션과 로그들을 간직한다.
* 'docker start' 명령어를 사용하여 정지된 컨테이너를 다시 사용할 수 있지만 정지 전 할당된 IP 주소는 유지되지 않는다.
<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128132916-7e1abfe0-ae50-458d-9729-e43c654a0770.png" width="70%" height="70%"/>
</div>
<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="8"></a>

# 도커 레지스트리

<br>

* 도커 레지스트리는 컨테이너의 이미지가 보관 되는 곳이다. 도커는 기본으로 도커 허브에 있는 이미지를 찾도록 설정되어 있다.
* 레지스트리 : 리포지터리를 여러개 가지는 보관 서비스
* 리포지터리 : 하나의 이미지에 대해 태그를 사용하여 다양한 출시 버전을 보관


## 퍼블릭 레지스트리

<br>

누구나 이용할 수 있도록 공개된 레지스트리. 도커 허브의 경우 무료로 사용 가능.(비공개는 과금)

git에 있는 Dockerfile이나 소스 코드를 편집하여 push 하면 자동으로 이미지를 빌드하고 repository에 등록해 주고, container의 취약성을 검사하고 보고 해주는 registry 도 있다.

* Docker Hub
* Quay

<br><br>

## 클라우드 레지스트리

<br>

퍼블릭 클라우드가 제공하는 Registry 서비스. 접근 가능한 계정을 제한하여 비공개로 운영할 수 있다.

* Amazon Elastic Container Registry
* Azure Container Registry
* Google Container Registry
* IBM Colud Container Registry

<br><br>

## 비공개 레지스트리

<br>

회사나 팀 전용으로 Registry를 구축하여 운영하는 경우에 해당. 오픈 소스로 이용할 수 있는 Registry software는 아래와 같다.

* Harbor
* GitLab Container Registry
* registry

쿠버네티스를 코어로 하는 소프트웨어 제품들 중에는 레지스트리 기능이 포함된 경우도 있다.

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="9"></a>

# Registry 와 Kubernetes 의 관계

<br>

쿠버네티스에서도 Registry에서 이미지를 다운로드 받아 컨테이너를 실행함.

1. docker build로 이미지 빌드
2. docker push로 이미지를 Registry에 등록
3. kubectl 커맨드로 매니페스트에 기재한 오브젝트들의 생성 요청
4. 매니페스트에 기재된 Repository로 부터 컨테이너의 이미지를 다운
5. Container 를 Pod 위에서 기동

예시) 

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128136339-cda2870d-b611-46de-ba92-9d3869dc582b.png" width="70%" height="70%"/>
</div>