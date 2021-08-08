<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# 서비스란?

<br>
쿠버네티스의 서비스는 클라이언트의 요청을 파드에 전달하는 역할을 담당한다. 이러한 서비스가 필요한 이유는 파드의 IP 주소가 기동할 때마다 바뀌기 때문. 그래서 파드에 접속해야하는 클라이언트는 서비스가 가지는 대표 IP를 사용해서 접속해야 한다.

<br>


<br><br>

## 목차

- [서비스 역할](#1)
- [대표 IP 주소](#2)
- [부하 분산](#3)
- [이름 해결](#4)
- [환경 변수](#5)

<br><br>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="1"></a>

# 서비스 역할

<br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128621376-1b33eb03-283a-49ff-9b1a-11dd0c5eb3bb.png" width="70%"  height="70%" />
</div>

<br>

1. 서비스는 로드밸런서의 역할을 가지며, 클라이언트의 요청을 받기 위한 대표 IP 주소를 획득한다.
2. 서비스의 이름은 내부 DNS에 등록되기 때문에 클라이언트는 서비스의 이름만으로 서비스의 IP 주소를 획득할 수 있다.
3. 서비스는 Selector에 지정된 라벨과 일치하는 파드 중 하나에게 요청을 전달한다. (파드는 기동될 때 라벨이나 IP 주소를 포함한 자신의 오브젝트 정보를 마스터 노드의 etcd에 등록, 서비스는 Selector 의 라벨에 일치하는 파드를 etcd에서 조회&취득)
4. 서비스가 만들어지고 나서 기동된 파등의 컨테이너에는 서비스에 대한 정보가 담긴 환경 변수가 자동으로 설정된다.
5. 서비스에는 4 종류의 서비스 타입이 있어, 클라이언트의 범위를 k8s 클러스터 내부로 한정할지, 외부까지 확장할지, 또한 k8s 클러스터 외부의 IP 주소에 전송할지를 설정한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a>

# 대표 IP 주소

<br>

서비스는 파드의 그룹을 대표하여 클라이언트의 요청을 받기 위해 대표 IP 주소(ClusterIP)를 가진다. 헤드리스로 지정한 경우에는 대표 IP 주소를 획득하지 않고, 파드의 IP 주소를 직접 내부 DNS에 설정한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a>

# 부하 분산

<br>

서비스의 대표 IP 주소에 도착한 요청은 Selector의 라벨과 일치하는 파드에 전송된다. 이를 위한 모듈인 kube-proxy는 초창기에는 커널의 유저 공간에서 동작하는 프록시 서버였지만, 지금은 <a href="https://itwiki.kr/w/리눅스_iptables">iptables</a> 나 <a href="https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/">ipvs</a>를 관리하는 프로그램으로 바뀌었다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# 이름 해결

<br>

서비스는 IP 주소와 서비스명을 k8s 클러스터의 내부 DNS에 등록한다. 그래서 k8s 클러스터 내의 파드에서는 서비스 이름으로 접근하는 것이 가능한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 환경 변수

<br>

서비스가 만들어지고 나서 생성되는 파드의 컨테이너에는 환경 변수가 설정되어 있다. 커넽이너 안의 애플리케이션 코드에서는 환경 변수를 이용해서 서비스의 대표 IP 주소를 얻을 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a>

# 서비스 타입 

<br>

서비스를 설정할 때는 해당 서비스를 이용하는 클라이언트를 고려하여 서비스 타입을 지어한다. 예를 들면, k8s 클러스터 내부 파드를 대상으로 하는 경우와 k8s 클러스터 외부에서 접근하는 경우를 고려하여 지정한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"></a>

# 어피니티

<br>

기본적으로 부하분산에 사용되는 알고리즘은 랜덤이다. 클라이언트에 따라 전송되는 파드를 고정하고 싶은 경우에는 sessionAffinity 항목에 ClientIP를 설정한다. 그리고 HTTP 헤드 안의 쿠키(Cookie) 값에 따라 전송되는 파드를 고정하고 싶은 경우에는 뒤에서 설명할 인그레스를 이용해야 한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="8"></a>

# 실릭터와 라벨

<br>

서비스에 도달한 요청 트래픽이 파드에 전송될 때는 Selector와 라벨을 참조한다. 라벨은 파드 등의 오브젝트에 부여하는 키값 쌍이다. 서비스에 도착한 요청은 Selector에 설정된 조건에 일치하는 라벨을 가지는 파드에 전송한다. Selector 의 라벨 조건이나 파드에 부여되는 라벨은 운영 중에도 바꿀 수 있어 유연하게 운영할 수 있다.

<br>

### 출처: 15단계로 배우는 도커와 쿠버네티스
### 출처: kubernetes document
<a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/">서비스</a>
