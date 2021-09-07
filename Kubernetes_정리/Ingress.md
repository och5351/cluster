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

<img src="https://user-images.githubusercontent.com/45858414/132308805-37ca0e2b-bf8e-484d-af97-a44ddbd4402f.png" />

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
        rules:
        - host: abc.sample.com
          http:
            paths:
            - path: /
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
