<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Ingress
<br>

인그레스는 k8s 클러스터 외부에서의 요청을 k8s 클러스터 내부의 Application에 연결 하기 위한 API 오브젝트이다. 디플로이먼트 관리하의 Application을 외부 공개용 URL과 매핑하여 인터넷에 공개하는 데 사용 된다.
인그레스는 SSL/TSL 암호화나 Session Affinity 기능을 갖추고 있어, 기존 웹 Application을 쿠버네티스화하는 데 유용한 오브젝트다.

<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/132303965-0ad0e11d-6e5c-438c-8da2-dfdb0c8cbc79.png" width="70%" height="70%" />

인그레스 개념
</div>
<br><br>

## 목차

- [인그레스 기능과 개요](#1)
- [공개 URL과 Application 매핑](#2)
- [가상 호스트와 서비스를 매핑하는 매니페스트 기술](#3)
- [kube-keepalibed-vip에 의한 VIP 획득과 HA 구성](#4)
- [고가용성 Ingress 환경 구축](#5)

<br><br>
<a id="1"></a>

# 1. 인그레스 기능과 개요
<br>

* 공개 URL과 Application 매핑
* 복수의 도메인 이름을 가지는 가상 호스트 기능
* 클라이언트의 요청을 여러 파드에 분산
* SSL/TLS 암호화 통신 HTTPS
* Session Affinity

인그레스는 기존의 로드밸런서나 리버스 프록시를 대체할 수 있다. 공개용 URL의 경로에 Application을 Mapping 하여 로드밸런싱, HTTPS 통신 그리고 Session Affinity 를 사용할 수 있기 때문이다.

Ingress는 다른 controller와는 달리 마스터 상의 kube-controller-manager의 일부로 실행되지 않는다.
다양한 컨트롤러가 있지만 Nginx 컨트롤러가 대표적이다.

Public Cloud에서의 k8s 관리 서비스는 Cloud의 기능과 Ingress를 연동하여 Public IP 주소를 연결한다.
On-premise 에서의 k8s 클러스터는 Public IP 주소를 노드 간에 공유하는 기능을 추가해야 한다. Github의 kube-keepalibed-vip를 이용.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a>

# 공개 URL과 Application 매핑
<br>

인그레스를 사용하면 공개 URL의 경로 부분에 복수의 Application을 매핑할 수 있다.
예를 들어, http://abc.sample.com이란 URL의 reservation과 order라는 경로에 각각의 전용 Application을 Mapping할 수 있다. 사용자 입장에서는 하나의 URL이지만 내부적으로 Application이 적절히 분리되어 있어 느슨하게 결합된 Application의 집합체로 구현 가능.

<div align="center">

<img src="https://user-images.githubusercontent.com/45858414/132308805-37ca0e2b-bf8e-484d-af97-a44ddbd4402f.png" width="70%" height="70%"/>

인그레스에 의한 가상 호스트 작성 예
</div>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a>

# 3. 가상 호스트와 서비스를 매핑하는 매니페스트 기술
<br>

Ingress의 Manifest 에서 Metadata와 annotation이 중요한 역할을 수행.
Annotation에 key와 value를 기재하여 Ingress controller에 명령을 전달한다고 볼 수 있다.


아래 예제의 주요 어노테이션

```yaml
kubernetes.io/ingress.class: 'nginx' # 여러 인그레스 컨트롤러가 k8s 클러스터에서 동작 중인 경우에는 
# 이 어노테이션을 명시적으로 지정할 필요가 있다.

nginx.ingress.kubernetes.io/rewrite-target: / # URL 경로를 바꾸도록 하는 어노테이션이다.
# 이 설정이 없으면 클라이언트로부터의 요청 경로를 파드에게 그대로 전송하여 File NotFound 에러로 연결될 수 있다.
```

ex) ingress.yaml 

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: hello-ingress
    annotations:
        kubernetes.io/ingress.class: 'nginx'
        nginx.ingress.kubernetes.io/rewrite-target: /
    spec:
        # v1.21 부터 추가 사항
        # defaultBackend:
        #   service:
        #     name: nginx
        #     port:
        #       number: 80
        rules:
        - host: abc.sample.com
          http:
            paths:
            - path: /
              # v1.21 부터 변경 사항
              # path: /
              # pathType: Prefix
              # backend:
              #   service:
              #     name: helloworld-svc
              #     port:
              #       number: 8080
              backend:
                serviceName: helloworld-svc
                servicePort: 8080
            - path: /apl2
              backend:
                serviceName: nginx-svc
                servicePort: 9080
        - host: xyz.sample.com
          http:
            paths:
            - path: /
              backend:
                serviceName: java-svc
                servicePort: 9080
```
<br>

표 1.인그레스 API(Ingress v1beta1 extensions)
<a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/">v.1.22 기준 Ingress reference</a>
<table>
  <thead>
    <tr>
      <th colspan="1">주요 항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>apiVersion</td>
      <td>extensions/v1beta1 설정</td>
    </tr>
    <tr>
      <td>kind</td>
      <td>Ingress 설정</td>
    </tr>
    <tr>
      <td>metadata.name</td>
      <td>인그레스 오브젝트의 이름</td>
    </tr>
    <tr>
      <td>metadata.annotations</td>
      <td>인그레스 컨트롤러 설정에 사용. <br> <a href="https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/">v.1.22 기준 Annotation 참고 자료</a></td>
    </tr>
    <tr>
      <td>spec</td>
      <td>표 2 참고</td>
    </tr>
  </tbody>
</table>
<br>

표 2. 인그레스 사양(IngressSpec v1beta1 extensions)
<a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/#IngressSpec">v.1.22 기준 인그레스 샤앙 참조</a>
<table>
  <thead>
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>rules</td>
      <td>DNS명과 백엔드 서비스를 대응시키는 규치 목록. 자세한 내용은 표3 참고. 이 규칙에 맞지 않는 요청은 디폴트 백엔드라는 파드에 전송됨</td>
    </tr>
    <tr>
      <td>tls</td>
      <td>표 7 참고</td>
    </tr>
  </tbody>
</table>
<br>

표 3. 인그레스 규칙(IngressRule v1beta1 extensions)

<table>
  <thead>
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>host</td>
      <td>FQDN(Fully Qualified Domain Name) 설정</td>
    </tr>
    <tr>
      <td>http</td>
      <td>자세한 내용은 표 4 참고</td>
    </tr>
  </tbody>
</table>

<br>

표 4. URL 경로와 백엔드 서비스의 대응 배열(HTTPIngressRuleValue v1beta1 extensions)
<a href="https://kubernetes.io/docs/reference/kubernetes-api/service-resources/ingress-v1/#IngressBackend">v.1.22 기준 backend 참조</a>
<table>
  <thead
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>paths</td>
      <td>URL의 경로와 백엔드 서비스를 대응시키는 목록을 기술. 표 5 참고</td>
    </tr>
  </tbody>
</table>
<br>

표 5. URL 경로와 백엔드 서비스의 대응(HTTPIngressPath v1beta1 extensions)
<table>
  <thead>
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>path</td>
      <td>URL 주소의 경로 부분을 기재</td>
    </tr>
    <tr>
      <td>backend</td>
      <td>요청이 전달될 서비스와 포트번호 기재. 표 6 참고</td>
    </tr>
  </tbody>
</table>
<br>

표 6. 전송될 서비스의 이름과 포트번호(IngressBackend v1beta1 extensions)
<table>
  <thead>
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>serviceName</td>
      <td>서비스 이름</td>
    </tr>
    <tr>
      <td>servicePort</td>
      <td>서비스의 포트번호</td>
    </tr>
  </tbody>
</table>
<br>

표 7. TLS 인증서와 도메인(IngressTLS v1beta1 extensions)
<table>
  <thead>
    <tr>
      <th colspan="1">주요항목</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>hosts</td>
      <td>도메인명 목록</td>
    </tr>
    <tr>
      <td>serviceName</td>
      <td>서버 인증서 시크릿의 이름. 시크릿은 네임스페이스 내에 보안이 필요한 데이터를 보존하는 오브젝트로 컨테이너에서 볼륨으로 마운트 가능</td>
    </tr>
  </tbody>
</table>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# 4. kube-keepalived-vip에 의한 VIP 획득과 HA 구성
<br>

Nginx Ingress Cotroller 는 Virtual IP(VIP)를 노드 간 공유하는 기능을 가지고 있지 않다.
그래서 복수의 노드로 구성된 학습 관경 2인 멀티 노드 k8s에서는 keepalived를 사용해야 한다.
Public Cloud에서는 Ingress와 VIP를 연결하는 기능이 있어 그대로 사용하면 된다.
하지만 On-premise 환경에서는 Ingress에 VIP를 더하기 위한 작업이 필요하다.
<br>

<div align="center">

<img src="https://user-images.githubusercontent.com/45858414/132448403-ce6b5bae-b14d-496c-81ba-23242f94ef92.png" width="70%" height="70%">

Ingress와 kube-keepalived-vip 구성

<br><br>

<img src="https://user-images.githubusercontent.com/45858414/132450289-2e907b73-2c84-4ca5-b645-9eb28272fa91.png" width="70%" heigh="70%">

요청이 VIP를 통해 파드에 전달되는 개념도
</div>

상세설명)
1. kube-keepalived-vip는 외부에서 접근 가능한 IP 주소를 노드에 할당한다.
2. nginx-ingress-svc와 인그레스 컨트롤러가 외부 네트워크 파드 네트워크를 중개한다.
  인그레스 컨트롤러는 파드이기 때문에 서비스 nginx-ingress-svc를 통해 요청을 받는다.
3. 그림 좌측의 인그레스는 URL과 Application을 Mapping한다.
4. 이러한 동작에 의해 Application 은 k8s cluster 외부로부터 요청을 전달 받는다.

<br>



<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 5. 고가용성 Ingress 환경 구축
<br>

1. 전용 네임스페이스 작성
2. Ingress configmap 작성
3. Ingress service account 작성과 RBAC 설정
4. Ingress default backend 배포
5. Ingress controller qovh
6. kube-keepalived-vip의 configmap 작성
7. kube-keepalived-vip의 service account 작성과 RBAC 설정
8. kube-keepalived-vip의 Daemonset 배포