<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Helm 이란?

<br>

Helm(헬름)이란 쿠버네티스 차트를 관리하기 위한 도구(Package managing tool) <br>
실무에서 여러 환경에 배포해야 하는 애플리케이션은 모두 차트로 패키징해 kubectl 대신 Helm으로 배포 및 업데이트를 수행한다.

<br><br>

## 목차

- [헬름 상세](#1)
- [헬름 3 설치](#2)

<br><br>
<a id="1"></a> 

# 헬름 상세

<br><br>

쿠버네티스를 사용하다 보면 수 많은 템플릿을 관리해야 하며, 그 템플릿 파일의 집합 들을 "차트" 라고 한다.
헬름은 차트와 차트 압축파일(tgz)를 만들 수 있다. <br>
차트가 저장된 차트 저장소와 연결해 쿠버네티스 클러스터에 차트를 설치하거나 삭제할 수 있으며, 헬름 차트들의 배포 주기를 관리할 수 도 있다.

<br>

### 1. 용어

<br>

 * Chart : 쿠버네티스에서 실행할 애플리케이션을 만드는 데 필요한 정보 묶음. Helm package 이며, App을 실행 시키기 위한 모든 리소스가 정의 되어 있다.
 * Config : 패키지 한 차트에 넣어서 배포 가능한 오브젝트를 만들 때 사용할 수 있는 설정
 * Release : 특정 Config를 이용해 실행 중인 차트의 인스턴스

<br>

### 2. Helm client 와 Tiller server

<br>

헬름은 커맨드라인 인터페이스인 Helm Client와 쿠버네티스 클러스터 안에 Helm client 명령을 받아 쿠버네티스 API와 통신하는 Tiller server로 구성 되어 있다.

Helm client를 사용하여 로컬 서버에서 차트를 만들고 차트 저장소들을 관리한다. Tiller server 와 통신하여 쿠버네티스 클러스터에 설치하려는 차트를 보낼 수 있고 현재 클러스터에 실행 중인 애플리케이션 릴리즈 정보를 요청하거나 실행 중인 릴리즈를 업그레이드 또는 삭제하는 요청을 할 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a> 

# 헬름 3 설치

<br><br>

쿠버네티스 클러스터에서 kubectl 명령어를 사용할 수 있는 곳에 설치

<br>

### 1. 설치

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

### 2. 버전 확인

```bash
helm version
```

<br>

### 3. Helm chart repository 추가

```bash
helm repo add prometheus https://prometheus-community.github.io/helm-charts
# https://kubernetes-charts.storage.googleapis.com/ 왼쪽 url은 add가 되지 않음
# https://charts.helm.sh/stable 왼쪽 url은 prometheus를 더이상 지원하지 않음
```

<br>

### 4. Chart list 출력

```bash
helm search repo prometheus
```

<br>

### 5. Chart update
```bash
helm repo update
```

<br>


git clone https://github.com/prometheus-community/helm-charts
### 6. Prometheus 배포
```bash
helm install monitor stable/promethues
```

<br>

### 7. kubernetes pod 상태 확인
```bash
kubectl get pod
```

<br>

### 8. 문제 chart 확인
```bash
helm inspect values stable/prometheus
```

<br>

### 9. (1번째 방법) 수정할 yaml 제작

``` yaml
# vi volumeF.yaml

alertmanager:
    persistentVolume:
        enabled: false
server:  
    persistentVolume:
        enabled: false
pushgateway: 
    persistentVolume:
        enabled: false
```

### 9-1. (1번째 방법) 수정 yaml로 upgrade
```bash
helm upgrade -f volumeF.yaml monitor stable/prometheus
```

<br>

### 10. (2번째 방법) 커맨드 라인으로 설정 추가
```bash
helm install monitor stable/prometheus --set alertmanager.persistentVolume.enabled=false --set server.persistentVolume.enabled=false --set pushgateway.persistentVolume.enabled=false 
```

<br>

### 11. kubectl