<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# 컨트롤러란?
<br>

컨트롤러는 파드를 제어한다. 하드에게 부여할 워크로드의 타입, 즉 처리에 따라서 적절한 컨트롤러를 선택해야 한다.
<br>



<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128622025-524874dd-38f4-422a-8f9b-f0979431a768.png" width="70%" height="70%">
</div>

<br>
<a href="https://kubernetes.io/ko/docs/concepts/architecture/controller/">컨트롤러</a>
<a href="https://kubernetes.io/ko/docs/concepts/workloads/">워크로드</a>
<br>

## 목차

- [워크로드 타입 - Frontend 처리](#1)
- [워크로드 타입 - Backend 처리](#2)
- [워크로드 타입 - 배치 처리](#3)
- [워크로드 타입 - 시스템 운영 처리](#4)
- [컨트롤러 타입 - 디플로이먼트(Deployment)](#5)
- [컨트롤러 타입 - 스테이트풀셋(StatefulSet)](#6)
- [컨트롤러 타입 - 잡(Job)](#7)
- [컨트롤러 타입 - 크론잡(CronJob)](#8)
- [컨트롤러 타입 - 데몬셋(DaemonSet)](#9)
- [컨트롤러 타입 - 레플리카셋(ReplicaSet)](#10)
- [컨트롤러 타입 - 레플리케이션 컨트롤러(Replication Controller)](#11)
- [워크로드-컨트롤러 대응표](#12)

<br><br>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="1"></a>

# 워크로드 타입 - Frontend 처리

<br>

스마트폰, IoT기기, 컴퓨터 등의 클라이언트로부터 요처을 직접 받아들이는 워크로드를 총칭한다. 이 타입의 워크로드는 대량의 클라이언트 요청에 대해 짧은 시간에 응답을 반환하는 것이 중요한다.(실시간, 끊임없는 반영)
요청에 대응하는 처리를 복수의 파드에서 분담하도록 설계해야 하며 24시간 무정지로 서비스를 제공하면서도 빠르게 신기능을 배포할 수 있어야 한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a>

# 워크로드 타입 - Backend 처리
<br>

백엔드 처리는 프론트엔드의 뒤에서 업무 특성에 맞게 대응할 수 있는 유연성이 있어야 한다. 요청량이 변하더라도 일정한 응담 속도를 유지해야 하며 요구 사항에 맞게 단기간에 기능을 추가하고 변경하는 것이 가능해야 한다.
<br>

백엔드 처리에는 보통 MySQL이나 Redis와 같은 미들웨어를 사용하거나 클라우드에서 제공하는 DB관리 서비스를 활용한다.
<br>

1. 데이터 스토어 : 데이터의 보존과 조회 기능(SQL/NoSQL 데이터베이스 등)
2. 캐시 : 복수 파드에서 데이터 공유(세션 정보 공유 등)
3. 메시징 : 비동기 시스템 간 연계 기능(메시지 브로커 등)
4. 마이크로 서비스 : 저문적 업무 기능 구현(결제, 배송, 결제 승인 등)
5. 배치 처리 : 긴 처리 시간을 요하는 업무 기능(ML, Data Analytics)

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a>

# 워크로드 타입 - 배치 처리
<br>

배치 처리는 어떤 트리거에 의해 실행이 된다. (Frontend로 부터의 요청, 정해진 시간, 단말로부터의 수시 요청 등)
배치 처리 내용은 다양하다.(동영상 포맷 변환, 대량 메일 송수신, 과학적 연산, GPU 사용)

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# 워크로드 타입 - 시스템 운영 처리
<br>

시스템 운영을 돕기 위해 쿠버네티스 API를 사용해서 노드에서 발생하는 에러나 하드웨어 이상을 감지하고 자동으로 대책을 실행하는 파드를 만드는 경우가 있음.
SRE(Site Reliability Engineering)는 소프트웨어 기술자가 시스템 운영의 자동화에 집중하여 효율적인 시스템 운영을 실현한다.(구글에서 강조하고 있으며 쿠버네티스 또한 구글에서 시작됐고, API를 통해 시스템 운영을 자동화할 수 있는 플랫폼이다.)

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 컨트롤러 타입 - 디플로이먼트(Deployment)
<br>

대등한 관계에 있는 여러 개의 파드로 수평한 클러스터를 구성할 때 사용.
정해진 개수 만큼 파드가 기동하도록 관리하며, 가동 중인 파드를 차례대로 교체하거나 규모를 조절할 수 있는 기능을 갖추고 있다.
<br>

<a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/">디플로이먼트 - 쿠버네티스 문서</a>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a>

# 컨트롤러 타입 - 스테이트풀셋(StatefulSet)
<br>

파드와 퍼시스턴트 볼륨을 조합하여 데이터의 보관에 초점을 둔 컨트롤러다. 파드와 퍼시스턴트 볼륨에 번호를 매겨 관리함으로써 본질적으로 일시적인 존재인 파드가 상태를 가지는 워크로드를 처리할 수 있도록 해준다.<br>

스테이트풀셋은 다음 중 하나 또는 이상이 필요한 애플리케이션에 유용하다.<br>


* 안정된, 고유한 네트워크 식별자.
* 안정된, 지속성을 갖는 스토리지.
* 순차적인, 정상 배포(graceful deployment)와 스케일링.
* 순차적인, 자동 롤링 업데이트.

<br>

<a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/statefulset/">스테이트풀셋 - 쿠버네티스 문서</a>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"> </a>

# 컨트롤러 타입 - 잡(Job)
<br>

배치 처리를 하는 컨테이너가 정상 종료할 때까지 재실행을 반복하는 컨트롤러다. 파드 실행 횟수, 동시 실행 수, 실행 횟수의 상한을 설정할 수 있으며, 지워질 때까지 로그를 보존한다. 데이터 처리나 과학 분야의 계산 작업 등에 사용된다. <br>

<a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/job/">잡 - 쿠버네티스 문서</a>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="8"> </a>

# 컨트롤러 타입 - 크론잡(CronJob)
<br>

UNIX에서 사용되는 cron과 같은 형식으로 잡의 생성 시각을 설정할 수 있고 실행 완ㄴ료한 파드를 몇 개 까지 보관할 수 있는지 설정할 수 있다. 정기적으로 실행되어야 하는 배치 처리에 적합하다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="9"> </a>

# 컨트롤러 타입 - 데몬셋(DaemonSet)
<br>

k8s 클러스터의 모든 노드에서 같은 파드를 실행하기 위해 존재한다. 클러스터 네트워크를 구성하는 파드는 데몬셋에 의해 모든 노드에서 실행되며, 새로운 노드가 추가되면 해당노드에서 자동으로 실행된다. 시스템 운영의 자동화에 적합하다.
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="10"> </a>

# 컨트롤러 타입 - 레플리카셋(ReplicaSet)
<br>

디플로이먼트 컨트롤러와 연동해서 파드가 기동외어야 하는 수를 관리. 레플리카셋은 직접 다루기보다는 디플로이먼트를 통해 이용하는 것이 기본이다.
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="11"> </a>

# 컨트롤러 타입 - 레플리케이션 컨트롤러(Replication Controller)
<br>

쿠버네티스 예전 튜토리얼에서 언급 되나 지금은 차세대 컨트롤러인 디플로이먼트로 대체

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="12"> </a>

# 워크로드-컨트롤러 대응표
<br>

<div align="center">

<table>
    <thead>
        <tr>
            <th colspan="1">워크로드 타입</th>
            <th colspan="1">컨트롤러 타입</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>프론트엔드 처리</td>
            <td>디플로이먼트</td>
        </tr>
        <tr>
            <td>백엔드 처리</td>
            <td>스테이트풀셋</td>
        </tr>
        <tr>
            <td>배치 처리</td>
            <td>잡, 크론잡</td>
        </tr>
        <tr>
            <td>시스템 운영</td>
            <td>데몬셋</td>
        </tr>
    </thead>
</table>

</div>
<br>

### 출처: 15단계로 배우는 도커와 쿠버네티스
### 출처: kubernetes document