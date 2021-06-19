<h2> kubernetes 마스터 구성 요소 </h2>

- etcd 
- kube-apiserver
- ube-scheduler
- kube-controller-manager
- cloud-controller-manager 
- ...

<h3> etcd </h3><hr>

#### etcd 는 고가용성을 제공하는 key - value 저장소. kubernetes에서 필요한 모든 데이터를 저장하는 실질적인 데이터 베이스. 원래 kubernetes는 처음에 구글 내부의 borg라는 컨테이너 오케스트레이션 도구의 오픈 소스화 도중 나온 것. borg 는 chubby라는 분산 저장 솔루션을 사용. kubernetes를 오픈 소스화 할 때 etcd를 사용하게 되었다고 함. etcd는 프로세스 1개만으로 사용 가능하지만 데이터의 안정성을 위해서는 여러개의 장비에 분산해서 etcd 자체를 클러스터링을 구성해서 동작시키는게 일반적 방법. 안정적으로 운영하려면 etcd에 있는 데이터를 주기적으로 백업해 줘야 한다. 

<br><h3> kube-apiserver </h3><hr>

#### kubernetes는 MSA 구조로 되어 있음. 그래서 여러 개의 분리된 프로세스로 구성 되어 있으며, 그 중에서 kube-apiserver 는 kubernetes cluster의 api 를 사용할 수 있게 해주는 프로세스. cluster로 요청이 왔을 때 그 요청이 유효한지 검증하는 역할을 함. kubernetes로의 모든 요청은 kube-apiserver는 수평적으로 확장이 가능하게 설계가 되어 있어서, 여러대의 장비에 여러개를 띄워놓고 사용할 수 있음.

<table>
    <thead>
        <tr>
            <th colspan="2">이름</th>
            <th colspan="2">타입</th>
            <th colspan="2">기본값</th>
            <th colspan="2">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>--admission-control</td>
            <td>stringSlice</td>
            <td>[Alwaysadmit]</td>
            <td>
Admission Controller는 쿠버네티스의 API를 호출했을 때, 해당 요청의 내용을 변형하거나 검증하는 플러그인의 집합이다. 예를 들어, 어떤 사용자가 쿠버네티스 API를 통해 포드의 생성을 요청했을 때 해당 포드의 설정이 적절한지 검증함으로써 포드 템플릿을 변경할 수도 있고, 부적절하다고 판단되면 아예 포드 생성 요청을 거부할 수도 있다. Admission Controller를 사용해 본 적이 없더라도, 쿠버네티스를 사용하면 의외로 알게 모르게 많이 쓰게 된다. 
            </td>
        </tr>
    </tbody>
</table>

<h2> kubernetes 설치 구성 요소 </h2>

<br><h3> Kubeadm </h3><hr>

#### kubeadm 이란, kubernetes에서 제공하는 기본적인 도구, kubernetes 클러스터를 빠르게 구축하기 위한 다양한 기능을 제공

- kubeadm init : Kubernetes 컨트롤 플레인 노드를 초기화한다.(마스터 초기화)
- kubeadm join : Kubernetes 워커 노드를 초기화하고 클러스터에 연결한다.
- kubeadm upgrade : kubernetes 업그레이드
- kubeadm config : 
- kubeadm token : 부트 스트랩 토큰을 사용한 인증에 설명되어 있는 대로 부트 스트랩 토큰은 클러스터에 참여하는 노드와 제어 평면 노드 사이에 양방향 신뢰를 설정하는 데 사용된다.
- kubeadm reset : kubeadm init 혹은 kubeadm join의 변경사항을 최대한 복구한다.
- kubeadm version : kubeadm 버전을 보여준다.
- kubeadm alpha : 정식으로 배포된 기능은 아니지만 kubernetes 측에서 사용자 피드백을 얻기 위해 인증서 갱신, 인증서 만료 확인, 사용자 생성, kubelet 설정 등 다양항 기능을 제공하고 있다.

<br><h3>kubelet</h3><hr>

#### kubelet 이란? 클러스터내의 모든 모드에서 실행되는 에이전트. 포드내의 컨테이너들이 실행되는 걸 직접적으로 관리하는 역할. kubelet은 PodSpecs 라는 설정을 받아서 그 조건에 맞게 컨테이너를 실행하고 컨테이너가 정상적으로 실행되고 있는지 상태 체크를 진행. 노드안에 있는 컨테이너라고 하더라도 쿠버네티스가 만들지 않은 컨테이너는 관리하지 않읍.
