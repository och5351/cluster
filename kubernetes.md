# Kubernetes 란?

쿠버네티스는 컨테이너화된 워크로드와 서비스를 관리하기 위한 이식할 수 있고, 확장 가능한 오픈소스 플랫폼으로, 선언적 구성과 자동화를 모두 지원한다. 쿠버네티스는 크고 빠르게 성장하는 생태계를 가지고 있다. 쿠버네티스 서비스, 지원 그리고 도구들은 광범위하게 제공된다.

# Kubernetes 의 장점


애플리케이션의 잦고, 빠른 출시 시기
---

쿠버네티스의 롤아웃, 롤백 기능을 통하여 새로운 기능을 자주 출시하고 버그 수정을 긴급하게 투입하는 것과 같이 민감한 작업을 안전하게 자동화 해 준다.
정식 운영 중인 서비스의 앱 컨테이너를 무정지로 교체할 수 있고 교체 중 발생하는 성능 저하와 프로그램 충돌로 인한 서비스 정지를 막기 위해 컨테이너 교체 정책을 설정할 수 있다.


무정지 서비스
---

쿠버네티스의 자기 회복 기능을 통하여 무정지 서비스 운영을 도와준다. 응답이 없어진 컨테이너를 재기동하며, 쿠버네티스 클러스터 내에 지정한 수만큼 컨테이너가 돌도록 관리 가능


초기 비용을 낮추고 비지니스 상황에 맞게 규모를 조정
---

컨테이너 기술은 앱과 실행 환경을 하나로 묶어서 배포할 수 있게 해준다.그리고 쿠버네티스는 복수의 노드 위에서 컨테이너가 돌아갈 수 있도록 해준다.
이 때 클러스터의 각 노드들이 똑같은 스펙일 필요가 없으며, 비지니스의 초기 단계에서는 스펙이 낮고 저렴한 가상ㅎ서버를 사용하다가, 비지니스가 확대되면 고성능의 가상 서버나 물리 서버를 투입시키는 
전략을 취하여 초기 비용을 낮출 수 있다.

k8s 클러스터 내에서 컨테이너를 다른 노드로 옮기기 위해서는 먼저 해당 노드에 스케줄이 되지 않도록 설정하고, 해당 노드의 모든 컨테이너를 추방시키면 된다.


쿠버네티스와 외부 서비스와의 연동
---

데이터베이스에 대한 컨태이너화는 신중하게 접근할 필요가 있다.(컨테이너는 태생적으로 언제든지 재시작될 수 있는 일시적인 존재로 상태를 포함하지 않는 것을 전제로 하기 때문)
이 때 한이브리드한 시스템을 구축하는 것이 한 가지 대안이 될 수 있다. 예를 들면, 클라우드의 DBaaS(Database as a Service)나 온프레미스에서 관리하는 데이터베이스와 연동하는 것.
이런한 연동을 위해 쿠버네티스는 외부 서비스를 내부 DBS에 등록하는 기능 제공

개발 환경과 운영 환경의 분리
---
컨테이너의 개발이 완료되어 테스트까지 끝났다면 정식 서비스 때 배포하기 전까지는 이미지를 다시 빌드하지 않는 것이 좋다. (테스트로 검증되지 않은 기능이 포함될 여지가 있기 때문)
운영 환경에서 사용하는 엔드포인트나 인증 정보는 테스트 환경과 다르기 마련이며, 데이터베이스의 접속 주소나 HTTPS를 위한 인증서 등이 다르다.

쿠버네티스에서는 클러스터를 여러 개의 가상 환경으로 분할 하는 것이 가능. 그리고 각각의 가상환경에 설정 파일, 보안이 필요한 인증서나 비밀번호를 저장할 수 있다.
즉, 테스트가 완료된 컨테이너의 이미지를 그대로 정식 운영 환경에 배포할 수 있다.


온프레미스와 클라우드 위에 구축
---

대형 클라우드 업체들은 EKS(AWS), AKS(Azure), IKS(IBM Cloud)와 같은 쿠버네티스 서비스를 제공한다. 쿠버네티스는 인프라의 복잡성을 감추며, 일관된 인터페이스로 다룰 수 있도록 설계되었다.
온프레미스와 클라우드 환경에서 동일한 인터페이스로 조작하며 운영할 수 있다.

애플리케이션 중심의 오케스트레이션
---

애플리케이션 개발자가 YAML 파일을 기술하여 쿠버네티스에 제출하면 로드밸런서, 저장소, 네트워크, 런타임 등의 환경이 구성된다.


특정 기업에 종속되지 않는 표준 기술
---

쿠버네티스 프로젝트는 원래 구글에서 시작되었지만, CNCF에 기증되어 오픈 소스 프로젝트로 운영되고 있다. 170여 개의 회사가 참가하고 있기 때문에 특정 회사에 종속되지 않은 포준 기술로 자리 잡았음.

서버들의 가동률 높이기
---

CPU 사용률이 낮은 서버가 많은 것은 퍼블릭 클라우드에서도 온프레미스에서도 바람직하지 않다. 한편, 쿠버네티스에서 사용되는 컨테이너 기술은 애플리케이션이 정해진 서버에서 돌지 않아도 된다는 자유를 제공한다.
또한, CPU 사용 시간이나 메모리 요구량도 간단히 제어할 수 있다.

이 기술 덕분에 쿠버네티스는 가동률이 적은 서버의 애플리케이션을 한 곳에 모을 수 있다. 그래서 서버의 CPU 가동률을 높게 유지하면서도 안정적으로 서비스를 제공한다는 상반되는 요구사항을 충족시킬 수 있는 것.

<a id="home1"></a>
# kubernetes 마스터 구성 요소 

- [etcd](#etcd)
- [kube-apiserver](#kube-apiserver)
- [kube-scheduler](#kube-scheduler)
- [kube-controller-manager](#kube-contoroller-manager)
- [cloud-controller-manager](#cloud-controller-manager)
- ...

<br>

<a id="etcd"></a> 

etcd
---
etcd 는 고가용성을 제공하는 key - value 저장소. kubernetes에서 필요한 모든 데이터를 저장하는 실질적인 데이터 베이스. 원래 kubernetes는 처음에 구글 내부의 borg라는 컨테이너 오케스트레이션 도구의 오픈 소스화 도중 나온 것. borg 는 chubby라는 분산 저장 솔루션을 사용. kubernetes를 오픈 소스화 할 때 etcd를 사용하게 되었다고 함. etcd는 프로세스 1개만으로 사용 가능하지만 데이터의 안정성을 위해서는 여러개의 장비에 분산해서 etcd 자체를 클러스터링을 구성해서 동작시키는게 일반적 방법. 안정적으로 운영하려면 etcd에 있는 데이터를 주기적으로 백업해 줘야 한다. 
<div align="right"> 

[목차로](#home1) 
</div><br>
<a id="kube-apiserver"></a>

kube-apiserver 
---
kubernetes는 MSA 구조로 되어 있음. 그래서 여러 개의 분리된 프로세스로 구성 되어 있으며, 그 중에서 kube-apiserver 는 kubernetes cluster의 api 를 사용할 수 있게 해주는 프로세스. cluster로 요청이 왔을 때 그 요청이 유효한지 검증하는 역할을 함. kubernetes로의 모든 요청은 kube-apiserver는 수평적으로 확장이 가능하게 설계가 되어 있어서, 여러대의 장비에 여러개를 띄워놓고 사용할 수 있음.
<div align="right"> 

[목차로](#home1) 
</div><br>

<table>
    <thead>
        <tr>
            <th colspan="1">이름</th>
            <th colspan="1">타입</th>
            <th colspan="1">기본값</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>--admission-control</td>
            <td>stringSlice</td>
            <td>[Alwaysadmit]</td>
            <td>
A변경에 관한 플러그인들이 실행되고나서 유효성 검사에 대한 플러그인들이 실행<br>
- 사용 가능한 값 : AlwaysAdmit, AlwaysDeny, AlwaysPullImages, DefaultStorageClass, DefaultTolerationSeconds, DenyEscalatingExec, DenyExecOnPrivileged, EventRateLimit, ExtendedResourceToleration, ImagePolicyWebhook, InitialResources, Initializers, LimitPodHardAntiAffinityTopology, LimitRanger, MutatingAdmissionWebhook, NamespaceAutoProvision, NamespaceExists, NamespaceLifecycle, NodeRestriction, OwnerReferencesPermissionEnforcement, PVCProtection, PersistentVolumeClaimResize, PersistentVolumeLabel, PodNodeSelector, PodPreset, PodSecurityPolicy, PodTolerationRestriction, Priority, ResourceQuota, SecurityContextDeny, ServiceAccount, ValidatingAdmissionWebhook
            </td>
        </tr>
        <tr>
            <td>--admission-control-config-file</td>
            <td>string</td>
            <td>없음</td>
            <td>
어드미션 컨트롤에 대한 설정 파일
            </td>
        </tr>
        <tr>
            <td>--advertise-address</td>
            <td>ip</td>
            <td>없음</td>
            <td>
Cluster의 다른 멤버들이 apiserver에 접근하기 위한 ip 주소. 이 값을 명시하지 않으면 --bind-address에 설정된 값이 사용. --bind-address도 설정 되어 있지 않으면 호스트의 기본 ip 주소가 사용.
            </td>
        </tr>
        <tr>
            <td>--allow-privileged</td>
            <td></td>
            <td>false</td>
            <td>컨테이너가 privileged 모드로 실행될 수 있게 함</td>
        </tr>
        <tr>
            <td>--anonymous-auth</td>
            <td></td>
            <td>true</td>
            <td>apiserver의 보안 포트에 사용자 명시 없이 익명으로 요청이 가능하게 한다.들어온 요청은 익명 요청으로 처리되고 사용자명은 system:anonymous, 그룹명은 system:unauthenticated 을 사용하게 된다.</td>
        </tr>
        <tr>
            <td>--apiserver-count</td>
            <td>int</td>
            <td>1</td>
            <td>클러스터내에서 실행되는 apiserver의 개수.</td>
        </tr>
        <tr>
            <td>--audit-log-format</td>
            <td>string</td>
            <td>json</td>
            <td>Cluster내에서 어떤일이 일어났는지 기록해두는 audit값을 어떤 형식으로 기록해 둘지 지정. json이 기본값이고, legacy로 설정하면 별다른 포맷없이 1줄의 로그 형식으로 기록.</td>
        </tr>
        <tr>
            <td>--audit-log-maxage</td>
            <td>int</td>
            <td></td>
            <td>audit 로그를 최대 며칠까지 유지하고 있을지 설정</td>
        </tr>
        <tr>
            <td>--audit-log-maxbackup</td>
            <td>int</td>
            <td></td>
            <td>오래된 audit 로그 파일을 최대 몇개까지 보관하고 있을지 설정</td>
        </tr>
        <tr>
            <td>--audit-log-path</td>
            <td>string</td>
            <td></td>
            <td>apiserver로 들어오는 모든 요청을 설정된 파일에 저장합니다. "-" 로 두면 표준출력으로 내보냄.</td>
        </tr>
        <tr>
            <td>--audit-policy-file</td>
            <td>string</td>
            <td></td>
            <td>audit 정책을 설정해둔 파일. </td>
        </tr>
        <tr>
            <td>--audit-webhook-batch-buffer-size</td>
            <td>int</td>
            <td>10000</td>
            <td>이벤트를 배치로 웹훅으로 보내기전에 보관해둘 버퍼의 크기. 배치모드에서만 사용됨.</td>
        </tr>
        <tr>
            <td>--audit-webhook-batch-initial-backoff</td>
            <td>duration</td>
            <td>10s</td>
            <td>요청이 실패하면 지정한 시간뒤에 재시도. 배치모드에서만 사용</td>
        </tr>
        <tr>
            <td>--audit-webhook-batch-max-size</td>
            <td>int</td>
            <td>400</td>
            <td>웹훅으로 보내는 배치의 최대 크기. 배치 모드에서만 사용.</td>
        </tr>
        <tr>
            <td>--audit-webhook-batch-max-wait</td>
            <td>duration</td>
            <td>30s</td>
            <td>배치가 최대크기 만큼이 안 됐어도 지정된 시간이 되면 보냄. 배치모드에서만 사용.</td>
        </tr>
        <tr>
            <td>--audit-webhook-barch-throttle-burst</td>
            <td>int</td>
            <td>15</td>
            <td>ThrottleQPS가 사용되지 않았을 때 동시에 보낼 수 있는 요청의 최대 개수. 배치모드에서만 사용</td>
        </tr>
        <tr>
            <td>--audit-webhook-batch-throttle-qps</td>
            <td>float332</td>
            <td>10</td>
            <td>요청을 초당 최대 얼마나 보낼 수 있는지 설정하는 값. 배치 모드에서만 사용.</td>
        </tr>
        <tr>
            <td>--audit-webhook-config-file</td>
            <td>string</td>
            <td></td>
            <td>audit 웹 훅 설정을 kubeconfig 형식으로 저장해 둔 파일의 위치.</td>
        </tr>
        <tr>
            <td>--audit-webhook-mode</td>
            <td>string</td>
            <td>batch</td>
            <td>audit 이벤트를 어떻게 처리할지에 대한 값. batch는 이벤트를 모았다가 한번에 보내는 비동기 처리를 함. Blocking은 이벤트를 보낼 때 서버 응답을 막도록 함.</td>
        </tr>
        <tr>
            <td>--authentication-token-webhook-cache-ttl</td>
            <td>duration</td>
            <td>2m0s</td>
            <td>웹 훅 토큰 인증기의 응답을 캐시하는 시간</td>
        </tr>
        <tr>
            <td>--authentication-token-webhook-config-file</td>
            <td>string</td>
            <td></td>
            <td>
            토큰 인증에 대한 설정을 kubeconfig 형식으로 가지고 있는 파일. apiserver가 여기 설정된 외부 서비스에 토큰의 인증 여부를 문의.
            </td>
        </tr>
        <tr>
            <td>--authorization-mode</td>
            <td>string</td>
            <td>AllwaysAllow</td>
            <td>
            인증에 사용할 플러그인들을 순서대로 입력합니다. 사용가능한 값 : AlwaysAllow, AlwaysDeny, ABAC, Webhook, RBAC, Node
            </td>
        </tr>
        <tr>
            <td>--authorization-policy-file</td>
            <td>string</td>
            <td></td>
            <td>인증 정책을 csv 형식으로 가지고 있는 파일.</td>
        </tr>
        <tr>
            <td>--authorization-webhook-cache-authorized-ttl</td>
            <td>duration</td>
            <td>5m0s</td>
            <td>
            웹훅 인증 서비스로부터 받은 "인증되었다"는 응답을 얼마나 캐시하고 있을지에 대한 값.
            </td>
        </tr>
        <tr>
            <td>--authorization-webhook-cache-unauthorized-ttl</td>
            <td>duration</td>
            <td>30s</td>
            <td>웹훅 인증 서비스로부터 받은 "인증불가"라는 응답을 얼마나 캐시하고 있을지에 대한 값.</td>
        </tr>
        <tr>
            <td>--authorization-webhook-config-file</td>
            <td>string</td>
            <td></td>
            <td>--authorization-mode=Webhook 옵션을 사용했을때 웹훅 설정을 kubeconfig 형식으로 가지고 있는 파일.</td>
        </tr>
        <tr>
            <td>--azure-container-registry-config</td>
            <td>string</td>
            <td></td>
            <td>애저(Azure) 컨테이너 저장소의 설정을 가지고 있는 파일.</td>
        </tr>
        <tr>
            <td>--basic-auth-file</td>
            <td>string</td>
            <td></td>
            <td>이 옵션이 설정되면 apiserver의 보안포트로 들어온 요청들에 대해서 http 기본인증을 하는데 사용됨.</td>
        </tr>
        <tr>
            <td>--bind-address</td>
            <td>ip</td>
            <td>0.0.0.0</td>
            <td>--secure-port port로 설정된 포트로 들어오는 요청을 받을 ip 주소. 클러스터의 다른 멤버들이나 kubectl같은 클라이언트가 접근가능한 주소여야 함.</td>
        </tr>
        <tr>
            <td>--cert-dir</td>
            <td>string</td>
            <td>/var/run.kubernetes</td>
            <td>TLS인증서들이 있는 경로. --tls-cert-file이나 --tls-private-key-file 옵션이 제공되면 이 옵션은 무시.</td>
        </tr>
        <tr>
            <td>--client-ca-file</td>
            <td>string</td>
            <td></td>
            <td>이 파일에 있는 클라이언트 인증서를 사용하는 요청은 클라이언트 인증서에 명시된 CommonName에 있는 값으로 인증.</td>
        </tr>
        <tr>
            <td>--cloud-config</td>
            <td>string</td>
            <td></td>
            <td>클라우드 제공자의 설정파일</td>
        </tr>
        <tr>
            <td>--cloud-provider</td>
            <td>string</td>
            <td></td>
            <td>클라우드 서비스 제공자</td>
        </tr>
        <tr>
            <td>--contention-profiling</td>
            <td></td>
            <td></td>
            <td>프로파일링이 활성화 되어 있을경우 락(lock) 경합에 대한 프로파일링을 활성화</td>
        <tr>
        <tr>
            <td>--cors-allowed-origins</td>
            <td>stringSlice</td>
            <td></td>
            <td>CORS를 허용하는 목록입니다. 하위 도메인을 표현하기 위해 정규표현식을 사용할수 있음. 이 옵션이 없는 경우 CORS를 사용할 수 없음.</td>
        </tr>
    </tbody>
</table>
<br>

<div style="text-align: right"> 
<div align="right"> 

[목차로](#home1) 
</div><br>
<a id="kube-scheduler"></a>

kube-scheduler 
---
kuber-scheduler는 이름에서 알 수 있듯이 새로운 포드들이 만들어질때 현재 클러스터내에서 자원할당이 가능한 노드들 중에서 알맞은 노드를 선택해서 그곳에 포드를 띄우는 역할을. Pod는 처음 실행될때 여러가지 조건을 지정해서 실행하는데, kube-scheduler가 그 조건에 맞는 노드를 찾아주는 역할. 필요한 하드웨어 요구사항이라던가, 어피니티/안티어피니티(affinity/anti-affinity) 조건을 만족하는지, 특정 데이터가 있는 노드에 할당한다던가 하는 다양한 설정을 할 수 있음.
<div align="right"> 

[목차로](#home1) 
</div><br>
<a id="kube-contoroller-manager"></a>

kube-controller-manager 
---
Kubernetes는 각각의 컨트롤러(controller)들이 Pod들을 관리하는 역할. kube-controller-manager는 이런 각각의 컨트롤러들을 실행하는 역할. 각 컨트롤러들은 논리적으로는 개별 프로세스이지만 복잡도를 줄이기 위해서 하나의 바이너리 파일로 컴파일되어 있고, 하나의 단일 프로세스로 실행. Kubernetes는 golang언어로 개발되어 있는데, 클러스터내에서 새로운 컨트롤러가 사용될때는 그 컨트롤러에 해당하는 구조체가 만들어진 다음에 그걸 kube-controller-manager가 관리하는 큐에 넣어서 실행하는 방식으로 작동.
<div align="right"> 

[목차로](#home1) 
</div><br>
<a id="cloud-controller-manager"></a>

cloud-controller-manager 
---
cloud-controller-manager 는 클라우드 서비스를 제공해 주는 곳들에서 쿠버네티스의 컨트롤러들을 자신들의 서비스와 연계해서 사용하기 위해서 사용. 관련된 코드는 각 클라우드 서비스 제공사들에서 직접 관리함. 다음 4가지 컨트롤러들이 관련있는 컨트롤러이 있음.

- 노드 컨트롤러(Node Controller) : 클라우드 서비스 내에서 노드를 관리하기 위해서 사용.
- 라우트 컨트롤러(Route Controller) : 각 클라우드 인프라내에서의 네트워크 라우팅을 관리하는데 사용.
- 서비스 컨트롤러(Service Controller) : 각 클라우드 서비스에서 제공하는 로드밸런서를 생성/갱신/삭제하는데 사용.
- 볼륨 컨트롤러(Volume Controller) : 클라우드 서비스에서 볼륨을 생성해서 노드에 붙이고 마운트해서 볼륨을 관리하는데 사용.


<div align="right"> 

[목차로](#home1) 
</div><br>

<a id="home2"></a>
# kubernetes 구성 요소

- [Kubeadm](#kubeadm)
- [kubelete](#kubelet)
- [kube-proxy](#kube-proxy)
- [컨테이너 런타임(Container Runtime)](#Container_Runtime)
- [애드온(Addons)](#Addons)
- [네트워킹 애드온](#networkingAddon)
- [DNS Addon](#dnsAddons)
- [DashBoard Addon](#dashboardAddon)
- [컨테이너 자원 모니터링](#monitoring)
= [클러스터 로깅](#clustering)

<a id="kubeadm"></a>

Kubeadm
--- 
kubeadm 이란, kubernetes에서 제공하는 기본적인 도구, kubernetes 클러스터를 빠르게 구축하기 위한 다양한 기능을 제공

- kubeadm init : Kubernetes 컨트롤 플레인 노드를 초기화한다.(마스터 초기화)
- kubeadm join : Kubernetes 워커 노드를 초기화하고 클러스터에 연결한다.
- kubeadm upgrade : kubernetes 업그레이드
- kubeadm config : 
- kubeadm token : 부트 스트랩 토큰을 사용한 인증에 설명되어 있는 대로 부트 스트랩 토큰은 클러스터에 참여하는 노드와 제어 평면 노드 사이에 양방향 신뢰를 설정하는 데 사용된다.
- kubeadm reset : kubeadm init 혹은 kubeadm join의 변경사항을 최대한 복구한다.
- kubeadm version : kubeadm 버전을 보여준다.
- kubeadm alpha : 정식으로 배포된 기능은 아니지만 kubernetes 측에서 사용자 피드백을 얻기 위해 인증서 갱신, 인증서 만료 확인, 사용자 생성, kubelet 설정 등 다양항 기능을 제공하고 있다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="kubelet"></a>

kubelet
---
kubelet 이란? 클러스터내의 모든 모드에서 실행되는 에이전트. 포드내의 컨테이너들이 실행되는 걸 직접적으로 관리하는 역할. kubelet은 PodSpecs 라는 설정을 받아서 그 조건에 맞게 컨테이너를 실행하고 컨테이너가 정상적으로 실행되고 있는지 상태 체크를 진행. 노드안에 있는 컨테이너라고 하더라도 쿠버네티스가 만들지 않은 컨테이너는 관리하지 않음.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="kube-proxy"></a>

kube-proxy
---
쿠버네티스는 클러스터 내부에 별도의 가상 네트워크를 설정하고 관리. kube-proxy는 이런 가상 네트워크가 동작할 수 있게 하는 실질적인 역할을 하는 프로세스. 호스트의 네트워크 규칙을 관리하거나 커넥션 포워딩을 함.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="Container_Runtime"></a>

컨테이너 런타임(Container Runtime)
---
컨테이너 런타임은 실제로 컨테이너를 실행시키는 역할을 합니다. 가장 많이 알려진 런타임으로는 도커(Docner)가 있고, 그외 rkt, runc같은 런타임도 지원합니다. 그외에도 컨테이너에 관한 표준을 제정하는 역할을 하는 OCI(Open Container Initiative, https://www.opencontainers.org/)의 런타임 규격(runtime-spec)을 구현하고 있는 컨테이너 런타임이라면 쿠버네티스에서 사용할 수 있습니다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="Addons"></a>

애드온(Addons)
---
애드온은 클러스터 내부에서 필요한 기능들을 위해 실행되는 포드들. 애드온에 사용되는 포드들은 디플로이먼트, 리플리케이션컨트롤러 등에 의해 관리. 애드온이 사용하는 네임스페이스는 kube-system. 애드온에는 몇가지 종류들이 있음.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="networkingAddon"></a>

네트워킹 애드온
---
쿠버네티스는 클러스터 내부에 가상네트워크를 구성해서 사용하는데, 이때 kuby-proxy이외에 네트워킹 관련한 애드온을 사용합니다. AWS, 애저, 구글클라우드같은 클라우드 서비스에서 제공하는 쿠버네티스를 사용한다면 별도의 네트워킹 애드온을 사용하지 않더라도 각 클라우드 벤더에서 구현하고 있으니 신경쓰지 않아도 됩니다. 하지만 쿠버네티스를 직접 보유중인 서버들에 설치해서 구성을 하려고 하려면 네트워킹 관련 애드온을 설치해서 사용해야합니다. 이 부분이 쿠버네티스를 직접 설치할때 가장 까다로운 부분이기도 합니다. 네트워킹 애드온의 종류는 10개가 넘을 정도로 다양하게 있습니다. ACI, Calico, Canal, Cilium, CNI-Genie, Contiv, Falannel, Multus, NSX-T, Nuage, Romana, Weave Net등이 있고, OCI의 CNI(Container Network, Interface) 를 구현하고 있다면 다른 애드온들도 사용할 수 있습니다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="dnsAddons"></a>

DNS 애드온
---
DNS 애드온의 경우 실제로 클러스터 내에서 작동하는 DNS 서버입니다. 쿠버네티스 서비스들에 DNS 레코드를 제공하는 역할을 합니다. 쿠버네티스 내부에서 실행된 컨테이너들은 자동으로 DNS서버에 등록됩니다. dns 서비스로는 kube-dns와 core-dns가 있습니다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="dashboardAddon"></a>

대시보드 애드온
---
쿠버네티스를 소개할때 대부분의 경우 kubectl이라는 CLI(Command Line Interface)를 많이 사용합니다. 하지만 웹 UI가 필요한 경우가 있을수도 있는데, 이런경우에 사용할수 있는 대시보드가 있습니다. 보이는 것 처럼 클러스터 현황을 한눈에 쉽게 파악할 수 있습니다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="monitoring"></a>

컨테이너 자원 모니터링
---
클러스터 내부에서 실행중인 컨테이너의 상태를 모니터링하기 위해서는 cpu, 메모리 같은 필요한 메트릭 데이터들을 시계열형식으로 저장하고 볼수 있는 방법을 제공하는데 필요한 애드온입니다. kubelet안에 cAdvisor라는 컨테이너 모니터링 도구가 포함되어 있는데요. 이 데이터들을 수집해서 사용할 수 있는 힙스터(heapster)를 이용하면 손쉽게 필요한 데이터들을 수집해서 모니터링에 이용할 수 있습니다.

<div align="right"> 

[목차로](#home2) 
</div><br>

<a id="clustering"></a>

클러스터 로깅
---
클러스터내의 각 개별 컨테이너에서 나오는 로그와 쿠버네티스 구성요소들에서 나오는 로그들을 중앙화된 로그 수집 시스템에 모아서 볼 수 있습니다. 이것 역시 클라우드 서비스를 이용중이라면 각 클라우드 벤더에서 제공해주는 로깅 서비스들과 잘 연동이 되어 있겠지만, 직접 쿠베네티스를 설치해서 사용할때는 고려해야 하는 요소입니다. 클러스터 내의 각 노드에 로그를 수집하는 포드를 실행해서 한곳으로 로그를 수집하도록 합니다. 로그를 수집해서 제공해주는 역할로는 ELK(ElasticSearch, Logstash, Kibana)를 많이 사용합니다.

<div align="right"> 

[목차로](#home2) 
</div><br>
