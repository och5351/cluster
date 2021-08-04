<a id="home1"></a>

# 컨테이너란?

하나의 리눅스 프로세스가 마치 전용 서버에서 동작하고 있는 것 같은 분리 상태이며, 리눅스 커널의 네임스페이스와 컨트롤 그룹(cgroup)이라는 기술을 기반으로 한다.

<br><br>

## 목차

- [컨테이너와 가상 서버의 차이점](#1)
- [컨테이너의 장점](#2)
- [도커 아키텍처](#3)
- [kube-controller-manager](#kube-contoroller-manager)
- [cloud-controller-manager](#cloud-controller-manager)


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


<img src="https://user-images.githubusercontent.com/45858414/128109064-84d5312a-7640-4d85-9e60-9d674785ba56.png" width="70%" height="70%"/>

<br>

* Windows나 Mac에서 컨테이너를 돌리려면 리눅스 커널을 위해 가상 서버 필요(Hyper-V[windows10], HyperKit[Mac] 위에서 LinuxKit 기동).
* LinuxKit 은 컨테이너를 실행하기 위한 경량의 리눅스 서브 시스템으로 도커(Docker), IBM, 리눅스 파운데이션(Linux Foundation), 마이크로소프트(Microsoft), ARM, 휴렛 패커드(Hewlett Packard), 인텔(Intel)과 같은 회사가 만듬
* 컨테이너는 하이버바이저상의 가상 서버에서도 사용할 수 있어 퍼블릭 클라우드의 '가상 서버'나 온프레미스의 Openstack 위에서 많이 활용 됨
<br><br>
<img src="https://user-images.githubusercontent.com/45858414/128111801-372e29b2-5b3d-4ac5-8800-7528aa0580cb.png" width="70%" height="70%"/>

</div>


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


* 리눅스 커널이 제공하는 기능을 활용하면 자체적 컨테이너를 만드는 것이 가능하나, 재사용이나 공유가 어렵다.
* 도커는 Build(작성), Ship(이동), Run(실행) 기능 지원
* 도커는 도커 데몬 서버와 클라이언트인 도커 커맨드, 이미지 보관소인 레지스트리로 구성


</div>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

# 도커 데몬

<div align="center">
* 클라이언트인 도커 커맨드의 명령을 받아들여서 도커 오브젝트인 이미지, 컨테이너, 볼륨, 네트워크 등을 관리
* 네트워크 너머에 있는 원격 클라이언트로부터 요청을 받는 것 가능
</div>

<div align="right"> 

[목차로](#home1) 
</div><br><br>




