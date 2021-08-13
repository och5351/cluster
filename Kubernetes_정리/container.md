<a href="https://github.com/och5351/cluster#readme">메인으로</a>

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
- [Registry 와 Kubernetes 의 관계](#9)
- [도커와 쿠버네티스의 연동](#10)
- [리눅스 표준 규격과 리눅스 ABI](#11)


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

### 인프라의 사용률 향상


* 하나의 물리서버나 가상 서버 위에서 여러 개의 컨테이너를 돌릴 수 있다.
* CPU와 메모리 사용률을 높여 하드웨어를 효율적을 이용할 수 있다.

<br><br>

### 빠른 기동 시간

* 컨테이너의 기동 시간은 가상 서버나 물리 서버의 기동 시간보다 훨씬 빠르다.
* 운영체제, 애플리케이션, 미들웨어 등 다양한 이미지를 쉽게 얻을 수 있다.
* 설치 작업이나 설정 작업이 줄어든다.
* 네트워크, 볼륨(외부 저장)을 소프트웨어 정의 오브젝트로 작성할 수 있다.

<br><br>

### 불변 실행 환경(Immutable Infrastructure)

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

<br><br>

### 퍼블릭 레지스트리

<br>

누구나 이용할 수 있도록 공개된 레지스트리. 도커 허브의 경우 무료로 사용 가능.(비공개는 과금)

git에 있는 Dockerfile이나 소스 코드를 편집하여 push 하면 자동으로 이미지를 빌드하고 repository에 등록해 주고, container의 취약성을 검사하고 보고 해주는 registry 도 있다.

* Docker Hub
* Quay

<br><br>

### 클라우드 레지스트리

<br>

퍼블릭 클라우드가 제공하는 Registry 서비스. 접근 가능한 계정을 제한하여 비공개로 운영할 수 있다.

* Amazon Elastic Container Registry
* Azure Container Registry
* Google Container Registry
* IBM Colud Container Registry

<br><br>

### 비공개 레지스트리

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

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="10"></a>

# 도커와 쿠버네티스의 연동

<br>

쿠버네티스는 도커를 컨테이너의 런타임 환경으로 사용. 

* 도커 데몬 프로세스인 dockerd 와 연동하여 동작하는 containerd 프로세스는 도커 기업이 개발하다가 CNCF(Cloud Native Computing Foundation)로 기증
* containerd는 이미지 보관 및 전송, 컨테이너 실행, 볼륨과 네트워크 연결과 같은 컨테이너의 라이프 사이클을 호스트에서 완전히 관리할 수 있게 됨
* containerd의 버전 1.1 부터는 CRI(Container Runtime Interface)에 대응하여 네이티브 kubelet 도 연동 가능
* conteinerd는 OCI(Open Container Initiative)의 표준 사양에 준하는 컨테이너 런타임 runC를 사용
* CRI를 통해 컨테이너 실행 요청을 받으면 containerd는 containerd-shim을 만든다. runC는 컨테이너를 띄운 후 바로 종료하며, containerd-shim 이 프로세르로 남음.
* 향후 쿠버네티스의 컨테이너 실행 환경은 도커 설치를 필수로 하지 않게 되어, 보다 심플, 경량화 된 방향으로 개발 진행 중. (public cloud 의 쿠버네티스도 kublet이 containerd와 직접 연계하는 방향으로 진행)

<br><br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128138268-437c318f-60eb-4415-b74e-224097ae7549.png" width="70%" height="70%"/>
</div>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="11"></a>

# 리눅스 표준 규격과 리눅스 ABI

<br>

리눅스 배포판과 커널 버전이 달라도 동작하는 이유는

1. LBS(Linux Base Standard)는 소스 코드를 컴파일한 시점에서 호환성 있는 머신 코드를 생성하도록 ISO 규격으로 표준화되어 있다.
2. 리눅스 ABI(Application Binary Interface)로 인해 리눅스 커널의 버전이 올라가도 유저 공간에서 동작하는 바이너리 레벨의 호환성은 유지된다.

<br>

### 리눅스 커널 기술

<br>

##### 1. Namespace

<br>

네임스페이스는 리눅스 커널에 사용된 기술로 컨테이너가 하나의 독립된 서버와 같이 동작할 수 있게 한다. 네임스페이스를 사용하면 특정 프로세스를 다른 프로세스로부터 분리시켜 같은 네임스페이스 내에서만 접근할 수 있도록 제한할 수 있다.

<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">네임스페이스</th>
            <th colspan="1">의미</th>
            <th colspan="1">역할</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>pid</td>
            <td>PID: Process ID</td>
            <td>리눅스 커널의 프로세스 ID 분리</td>
        </tr>
        <tr>
            <td>net</td>
            <td>NET: Networking</td>
            <td>네트워크 인터페이스(NET) 관리</td>
        </tr>
        <tr>
            <td>ipc</td>
            <td>IPC: Inter Process Communication</td>
            <td>프로세스 간 통신(IPC) 접근 관리</td>
        </tr>
        <tr>
            <td>mnt</td>
            <td>MNT: Mount</td>
            <td>파일 시스템의 마운트 관리</td>
        </tr>
        <tr>
            <td>uts</td>
            <td>UTS: Unix Timesharing System</td>
            <td>커널과 버전 식별자 분리</td>
        </tr>
    </table>
</thead>
</div>

<br><br>

##### 2. 컨트롤 그룹(cgroup)

<br>

도커는 리눅스 커널의 cgroup을 사용한다. cgroup은 프로세스별로 CPU 시간이나 메모리 사용량과 같은 자원을 감시하고 제한한다.

<br><br>

##### 3. 유니온 파일 시스템(UnionFS)

<br>

UnionFS는 다른 파일 시스템에서 파일이나 디렉터리를 투과적으로 겹쳐서, 하나의 일관적인 파일 시스템으로 사용할 수 있게 한다. 도커에서는 UnionFS의 여러 구현체(aufs, vtrfs, overlay2) 중 하나를 선택할 수 있다.
예전에는 aufs가 사용되었으나, Docker CE 버전 17.12 에서는 보다 빠르게 동작하고 구조가 간단한 overlay2가 사용된다.

<br><br>

##### 4. OCI(Open Container Initiative)

<br>

컨테이너의 표준 사양을 책정하기 위해 2015년 6월에 만들어진 단체.

설립 초기에는 도커가 사실상 컨테이너의 표준이었으나, CoreOS가 또 다른 표준화를 진행하고 있어 업계 표준이 필요해짐에 따라, 2017년 7월에 발표한 OCI v1.0은 컨테이너 런타임의 표준 사양인 Runtime Specification v1.0과 이미지 포맷의 표준 사양인 Format Specification v1.0 으로 구성된다.

이에 맞춘 도커의 runC가 있으며, CoreOS의 컨테이너 런타임 rkt도 진행중.

<div align="right"> 

[목차로](#home1) 
</div><br><br>


# 도커 커맨드
<br>

##### 컨테이너 환경 표시
<br>
<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">커맨드 실행</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>docker version</td>
            <td>도커 클라이언트와 서버 버전 표시</td>
        </tr>
    </tbody>
</table>
</div>
<br>

##### 컨테이너의 3대 기능
<br>

##### 1. 컨테이너 이미지 빌드 
<br>
<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">커맨드 실행 예</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>docker build -t 리포지터리:태그 <br> docker image build -t 리포지터리:태그</td>
            <td>현 디렉터리에 있는 Dockerfile을 바탕으로 이미지를 빌드</td>
        </tr>
        <tr>
            <td>docker images <br> docker images ls</td>
            <td>로컬 이미지 목록</td>
        </tr>
        <tr>
            <td>docker rmi 이미지 <br> docker image rm 이미지</td>
            <td>로컬 이미지 삭제</td>
        </tr>
        <tr>
            <td>docker rmi -f 'docker images -aq' <br> docker image prune -a</td>
            <td>로컬 이미지 일괄 삭제</td>
        </tr>
    </tbody>
</table>
</div>
<br>

##### 2. 이미지의 이동과 공유
<br>
<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">커맨드 실행 예</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>docker pull 원격_리포지터리[:태그] <br> docker image pull 원격_리포지터리[:태그]</td>
            <td>원격 리포지터리의 이미지를 다운로드</td>
        </tr>
        <tr>
            <td>docker tag 이미지[:태그] 원격_리포지터리[:태그] <br> docker image tag 이미지[:태그] 원격_리포지터리[:태그]</td>
            <td>로컬 이미지에 태그 부여</td>
        </tr>
        <tr>
            <td>docker login 레지스트리_서버_URL</td>
            <td>레지스트리 서비스에 로그인</td>
        </tr>
        <tr>
            <td>docker push 원격_리포지터리[:태그]<br>docker image push 원격_리포지터리[:태그]</td>
            <td>로컬 이미지를 레지스트리 서비스에 등록</td>
        </tr>
        <tr>
            <td>docker save -o 파일명 이미지<br>docker image save -o 파일명 이미지</td>
            <td>이미지를 아카이브 형식 파일로 기록</td>
        </tr>
        <tr>
            <td>docker load -i 파일명<br>docker image load -i 파일명</td>
            <td>아카이브 형식 파일을 리포지터리에 등록</td>
        </tr>
        <tr>
            <td>docker export &gt 컨테이너명 | 컨테이너ID &lt -o 파일명 <br>docker container export &gt 컨테이너명 | 컨테이너ID &lt -o 파일명 </td>
            <td>컨테이너명 또는 컨테이너ID로 컨테이너를 지정해서 tar 형식 파일로 기록</td>
        </tr>
        <tr>
            <td>docker import 파일명 리포지터리[:태그]<br>docker image import 파일명 리포지터리[:태그]</td>
            <td>파일로 저장된 이미지를 리포지터리에 입력</td>
        </tr>
    </tbody>
</table>
</div>
<br>

##### 3. 컨테이너 실행
<br>
<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">커맨드 실행 예</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>docker run --rm -it 이미지 커맨드<br>docker run container --rm -it 이미지 커맨드</td>
            <td>대화형으로 컨테이너를 기동해서 커맨드를 실행, 종료시에는 컨테이너를 삭제. 커맨드에 sh와 bash를 지정하면 대화형 셸로 리눅스 명령어 실행 가능</td>
        </tr>
        <tr>
            <td>docker run -d -p 5000:80 이미지<br> docker container run -d -p 5000:80 이미지</td>
            <td>백그라운드로 컨테이너를 실행. 컨테이너 내 프로세ㅅ의 표준 출력과 표준 에러 출력은 로그에 보존. 보존된 로그의 출력은 'docker logs' 참조. '-p'는 포트 포워딩으로 '호스트_포트:컨테이너_포트'로 지정</td>
        </tr>
        <tr>
            <td>dockerrun -d --name 컨테이너명 -p 5000:80 이미지<br>dockerrun container -d --name 컨테이너명 -p 5000:80 이미지</td>
            <td>컨테이너에 이름을 지정하여 실행</td>
        </tr>
        <tr>
            <td>docker run -v 'pwd'/html:/usr/share/nginx/html -d -p 5000:80 nginx<br>docker container run -v 'pwd'/html:/usr/share/nginx/html -d -p 5000:80 nginx</td>
            <td>컨테이너에 파일 시스템에 디렉터리를 마운트하면서 실행. '-v'는 '로컬_절대_경로:컨테이너_내_경로'</td>
        </tr>
        <tr>
            <td>docker exec -it &gt 컨테이너명 | 컨테이너 ID &lt sh<br>docker container exec -it &gt 컨테이너명 | 컨테이너 ID &lt sh</td>
            <td>실행 중인 컨테이너에 대해서 대화형 셸을 실행</td>
        </tr>
        <tr>
            <td>docker ps<br>docker container ls</td>
            <td>실행 중인 컨테이너 목록 출력</td>
        </tr>
        <tr>
            <td>docker ps -a<br>docker container ls -a</td>
            <td>정지된 컨테이너도 포함하여 출력</td>
        </tr>
        <tr>
            <td>docker stop &gt 컨테이너명 | 컨테이너 ID &lt<br>docker container stop &gt 컨테이너명 | 컨테이너 ID &lt</td>
            <td>컨테이너의 주 프로세스에 시그널 SIGTERM을 전송하여 종료 요청. 타임 아웃 시 강제 종료 진행</td>
        </tr>
    </tbody>
</table>
</div>

### 출처: 15단계로 배우는 도커와 쿠버네티스