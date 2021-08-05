<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# 파드란

<br>

파드는 쿠버네티스에서 컨테이너를 실행하는 최소 단위로 한 개 혹은 여러 개의 컨테이너를 포함한다.
하나의 파드에 속하는 모든 컨테이너들은 같은 노드에서 동작한다.

<br><br>

## 목차

- [Pod의 목적](#1)
- [Pod에 관하여](#2)
- [Pod life cycle](#3)
- [Pod의 종료 처리 흐름](#4)
- [클러스터 네트워크](#5)


<br><br>
<a id="1"></a> 

# 파드의 목적

<br>

파드는 하나의 목적을 위해 만들어진 컨테이너를 부품처럼 조합할 수 있도록 설계.

1. 파드 내부의 컨테이너들은 파드의 IP 주소와 포트번호를 공유한다.
2. 파드의 내부 컨테이너들은 localhost로 서로 통신할 수 있다.
3. 파드의 내부 컨테이너들은 <a href="https://wiki.kldp.org/HOWTO/html/Secure-Programs-HOWTO/sysv-ipc.html">System V 프로세스 통신</a>이나 POSIX 공유 메모리를 사용하여 서로 통신할 수 있다.
4. 파드의 내부 컨테이너들은 파등의 볼륨을 마운트하여 파일 시스템을 공유할 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a>

# Pod 에 관하여

<br>

1. 파드는 일시적인 존재

일시적 존재로 설계되어 파드 내의 컨테이너는 이미지로 부터 매번 생성 된다. 같은 오브젝트 이름으로 몇 번이고 파드를 기동해도 이전 컨테이너 수행&변경 이력은 남지 않고 이미지 초기 상태로 시작한다.

파드의 IP 주소도 고정적이지 않고 기동 시 부여되며 종료 시 회수 된다. 그렇기에 파드에 요청을 보내고 싶은 경우 반드시 서비스를 사용해야 한다.

<br>

2. 파드는 컨테이너의 실행 상태를 관리

파드가 정지한 경우에는 담당 컨트롤러가 재기동 등의 정해진 처리를 수행하는 한편, 파드 내부의 컨테이너가 정지한 경우에는 파드가 해당 컨테이너를 재시작한다. 설정에 따라 정지한 컨테이너를 그대로 유지한 채 로그를 참조할 수 있도록 하는 것도 가능.

파드는 활성 프로브(Liveness Probe)와 준비 상태 프로브(Readiness Probe)를 설정하여 내부 애플리케이션의 상태를 감시할 수 있다.

활성 프로브 설정 -> 애플리케이션이 멈춰 있는 상태 감지 시 컨테이너 강제 종료.
준비 상태 프로브 설정 -> 파드가 요청을 받을 준비가 될 때 까지 서비스 오브젝트가 요청을 전송하지 않음.

<a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/pod-lifecycle/">파드 라이프 사이클 참조</a>

<br>

3. 파드는 초기화 전용 컨테이너를 실행

파드에 초기화만을 담당하는 컨테이너를 설정할 수 있다. 

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a>

# Pod life cycle

<br>

<table>
    <thead>
        <tr>
            <th colspan="1">STATUS</th>
            <th colspan="1">의미와 대책</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ContainerCreating</td>
            <td>이미지를 다운로드 중이거나 컨테이너를 생성하는 중에 있음을 의미. ConfigMap 과 Secret이 마운트되지 않아 컨테이너 생성이 보류된 경우일 수 있다.</td>
        </tr>
        <tr>
            <td>CrashLoopBackOff</td>
            <td>파드 내의 컨테이너가 종료되어 다음 기동 시까지 대기 상태에 있음을 의미. 2회 이상 컨테이너가 종료되면, CrashLoopBackOff 시간 동안 대기하게 된다. 이 상황에서는 컨테이너 내의 프로세스를 재검토할 필요가 있음.</td>
        </tr>
        <tr>
            <td>Pending</td>
            <td>파드 생성 요구를 받았지만 하나 이상의 컨테이너가 생성되지 않은 상태를 의미. 리소스 부족 등의 이유로 스케줄이 되지 않은 경우에 해당한다.</td>
        </tr>
        <tr>
            <td>Running</td>
            <td>파드의 모든 컨테이너가 생성되어 실행 중임을 의미.</td>
        </tr>
        <tr>
            <td>Terminating</td>
            <td>컨테이너에 종료 요청 시그널을 보낸 후 컨테이너가 종료할 때까지 대기 중임을 의미. 유예 시간을 넘겨도 컨테이너가 종료할 수 없는 경우에 컨테이너를 강제로 종료한다.</td>
        </tr>
        <tr>
            <td>Succeeded</td>
            <td>파드 내 모든 컨테이너가 정상적으로 종료했음을 의미.</td>
        </tr>
        <tr>
            <td>Completed</td>
            <td>파드 내 컨테이너가 정상적으로 종료되었음을 의미. 파드 내에 복수의 컨테이너가 있는 경우, 첫 번째 컨테이너가 정상 종료(Exit 코드 = 0) 하면 Completed가 표시.</td>
        </tr>
        <tr>
            <td>Error</td>
            <td>컨테이너가 이상 종료된 경우. Exit 코드 != 0 인 경우에 이상 종료로 간주. 파드 내에 복수의 컨테이너가 있는 경우, 첫 번째 컨테이너가 이상 종료하면, Error로 표시된다.</td>
        </tr>
        <tr>
            <td>Failed</td>
            <td>파드 내에 적어도 하나의 컨테이너가 이상 종료했음을 의미.</td>
        </tr>
        <tr>
            <td>Unknown</td>
            <td>파드의 상태를 얻을 수 없는 상황을 의미.</td>
        </tr>
    </thead>
</table>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# Pod의 종료 처리 흐름

<br>

PreStop 은 컨테이너가 종료되기 직전에 호출되며, 컨테이너 내의 프로세스가 종료 요청 시그널을 받을 수 없는 제약이 있는 경우에 종료 요청을 받는 수단이 된다. kubectl delete를 실행할 때 옵션 --grace-period=초 를 통해 유예 시간을 별도로 지정할 수 있다. (0일 경우 즉시 제거)

<br>

1. 사용자가 kubectl delete pod를 실행 시 Pod 종료 처리가 시작 됨.(기본 유예 시간 30초)
2. kubectl get pod 의 status 는 Terminating 이라고 표시됨.
3. 다음 3가지 작업이 동시에 진행 된다.
   * Pod의 PreStop hook이 정의되어 있으면, Pod 내에서 호출 됨. 유예 시간을 넘어서면 PreStop hook이 실행되고 있어도, 파드 내의 메인 프로세스에 SIGTERM이 보내지며, 2초 후에 SIGKILL로 강제 종료된다.
   * PreStop hook이 정의되어 있지 않다면, 곧 바로 Pod 내의 메인 프로세스에 SIGTERM 신호가 송신되며 종료 처리가 개시된다.
   * Pod가 서비스의 엔드 포인트 목록에서 제거되며, 로드밸런서(kube-proxy 등)의 목록에서도 제거된다.
4. 유예 시간을 넘어서서 Pod 내의 프로세스가 살아 있다면, 파드의 메인 프로세스에 SIGKILL을 보내서 강제로 종료한다.
5. 제거 대상인 Pod가 표시되지 않게 된다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 클러스터 네트워크

<br>

클러스터 네트워크는 k8s 클러스터 내부에서 사용되는 네트워크이다. pod는 이 네트워크상의 IP주소를 부여받아 다른 노드에 있는 Pod와 서로 통신한다.

서비스의 IP 주소나 포트번호를 k8s 클러스터 외부 네트워크에서 접근할 수 있게 하려면, 서비스 타입을 NodePort 혹은 LoadBalancer를 설정하거나 <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">인그레스</a>를 함께 사용하면 된다.
아래는 대표적인 오픈 소스 네트워크 애드온 2가지 이다.
<br>


### Flannel

<br>

간단한 L3 네트워크를 노드 간에 구축한다. 각 노드 위에 서브넷을 구성해서 한 노드 위의 파드가 다른 노드 위의 파드랑 통신할 수 있도록 한다.
Flannel의 파드는 데몬셋 컨트롤러에 의해 배포된다. 그래서 k8s 클러스터에 새로운 노드가 추가되면 자동적으로 파드 네트워크가 확장된다. Flannel은 네트워킹 기능만 갖추고 있으며, 접근 제어 기능은 제공하지 않는다.

<br>

### Cailco

<br>

노드 간 파드 통신에 더해 네트워크 접근 제어 기능을 제공한다. 예를 들면, 2개의 네임스페이스간에 통신을 금지하는 접근 제어 기능을 설정할 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 출처: 15단계로 배우는 도커와 쿠버네티스
### 출처: kubernetes document