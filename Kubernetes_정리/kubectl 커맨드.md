
<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# kubectl commad
<br>

kubectl command 인자의 기본 형식

```bash
kubectl # <(1) 커맨드> <(2)리소스 타입> [이름] [(3) 옵션]
```

<br><br>

## 목차

- [kubectl 첫 번째 파라미터](#1)
- [파드 관련 리소스 타입](#2)
- [서비스 관련 리소스 타입](#3)
- [컨트롤러 관련 리소스 타입](#4)
- [볼륨 관련 리소스 타입](#5)
- [k8s 클러스터의 구성 관련 리소스 타입](#6)
- [컨피그맵과 시크릿 관련 리소스 타입](#7)
- [네임스페이스 관련 리소스 타입](#8)
- [역할 기반 액세스 제어(RBAC) 관련 리소스 타입](#9)
- [보안 관련 리소스 타입](#10)
- [자원 관리 관련 리소스 타입](#11)
- [옵션](#12)

<br><br>
<a id="1"></a>

##### 1. kubectl 첫 번째 파라미터
<br>
<table>
    <thead>
        <tr>
        <th colspan="1">커맨드</th>
            <th colspan="1">사용 예</th>
            <th colspan="1">개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>get</td>
            <td>kubectl get -f &lt 매니페스트 | 디렉터리 &gt<br>
            kubectl get &lt 리소스_타입 &gt<br>
            kubectl get &lt 리소스_타입 &gt &lt 이름 &gt<br>
            kubectl get &lt 리소스_타입 &gt &lt 이름 &gt &lt 옵션 &gt
            </td>
            <td>get은 지정한 오브젝트의 목록을 한 줄에 하나씩 출력</td>
        </tr>
        <tr>
            <td>describe</td>
            <td>kubectl describe -f &lt 매니페스트 | 디렉터리 &gt<br>
            kubectl describe &lt 리소스_타입 &gt<br>
            kubectl describe &lt 리소스_타입 &gt &lt 이름 &gt<br>
            kubectl describe &lt 리소스_타입 &gt &lt 이름 &gt &lt 옵션 &gt
            </td>
            <td>describe의 경우 get보다도 자세한 정보를 출력</td>
        </tr>
        <tr>
            <td>apply</td>
            <td>kubectl apply -f &lt 매니페스트 | 디렉터리 &gt</td>
            <td>docker-compose 관리하의 컨테이너를 정지</td>
        </tr>
        <tr>
            <td>create</td>
            <td>kubectl create -f &lt 파일명 &gt</td>
            <td>docker-compose 관리하의 컨테이너를 정지</td>
        </tr>
        <tr>
            <td>delete</td>
            <td>kubectl delete -f &lt 매니페스트 | 디렉터리 &gt<br>
            kubectl delete &lt 리소스_타입 &gt &lt 이름 &gt
            </td>
            <td>매니페스트에 기술된 오브젝트를 삭제</td>
        </tr>
        <tr>
            <td>config</td>
            <td>kubectl config get-contexts<br>
            kubectl config use-context &lt 콘텍스트명 &gt
            </td>
            <td>접속 대상인 되는 콘텍스트(k8s 클러스터, 네임스페이스, 유저)의 목록을 출력하거나 선택</td>
        </tr>
        <tr>
            <td>exec</td>
            <td>kubectl exec -it &lt 파드명 &gt [-c 컨테이너명] &lt 커맨드 &gt</td>
            <td>컨테이너에 대화형으로 커맨드를 실행, 파드 내에 컨테이너가 여러 개 있는 경우 [-c]로 컨테이너명을 지정. 컨테이너명은 'kubectl get describe &lt 파드명 &gt' 으로 확인 가능</td>
        </tr>
        <tr>
            <td>run</td>
            <td>kubectl run &lt 이름 &gt --image=&lt 이미지명 &gt</td>
            <td>파드를 실행</td>
        </tr>
        <tr>
            <td>logs</td>
            <td>kubectl logs &lt 파드명 &gt [-c 컨테이너명]</td>
            <td>컨테이너 로그 표시</td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

# 리소스 타입
<br>

kubectl api-resource 로 얻은 리소스의 목록

<br>
<a href="2"></a>

##### 2-1. 파드 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
        <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>pod(po)</td>
            <td>파드</td>
            <td>컨테이너의 최소 기동  단위로, 기동 시 파드 네트워크상의 IP주소를 할당받으며, 한 개 이상의 컨테이너를 내포<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/">파드</a>
            </td>
        </tr>
        <tr>
            <td>poddisruptionbudget(pdb)</td>
            <td>파드 정지 허용 수</td>
            <td>파드의 개수가 지정한 개수 이하가 되지 않도록 디플로이먼트, 스테이트풀셋, 레플리카셋, 레플리케이션 컨트롤러의 동작을 제어<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/pods/disruptions/">중단(disruption)</a>
            </td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="3"></a>

##### 2-2. 서비스 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>service(svc)</td>
            <td>서비스</td>
            <td>파드를 클라이언트에 공개<br>
            <a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/">서비스</a></td>
        </tr>
        <tr>
            <td>endpoints(ep)</td>
            <td>엔드포인트</td>
            <td>서비스를 제공하는 파드의 IP 주소와 포트를 관리.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%A0%95%EC%9D%98">엔드포인트</a></td>
        </tr>
        <tr>
            <td>ingress(ing)</td>
            <td>인그레스</td>
            <td>서비스 공개, TLS 암호, 세션 유지, URL 매핑 기능을 제공<br>
            <a href="https://kubernetes.io/ko/docs/concepts/services-networking/ingress/">인그레스</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="4"></a>

##### 2-3. 컨트롤러 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>deployment(deploy)</td>
            <td>디플로이먼트</td>
            <td>파드의 레플리카 수, 자기 회복, 롤아웃, 롤백, 등을 제어하는 컨트롤러.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/deployment/">디플로이먼트</a></td>
        <tr>
            <td>replicaset(rs)</td>
            <td>레플리카셋</td>
            <td>파드의 레플리카 수를 제어하는 컨트롤러로, 디플로이먼트와 연계하여 동작.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicaset/">레플리카셋</a></td>
        </tr>
        <tr>
            <td>statefulset(sts)</td>
            <td>스테이트풀셋</td>
            <td>퍼시스턴트 데이터를 보유하는 파드를 제어한느 컨트롤러, 퍼시스턴트 볼륨과 파드를 하나씩 쌍으로 묶어 각 이름에 동일한 일련 번호를 부여하여 관리.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/statefulset/">스테이트풀셋</a></td>
        </tr>
        <tr>
            <td>job</td>
            <td>잡</td>
            <td>배치 처리를 수행하는 파드를 관리하는 컨트롤러.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/job/">잡</a></td>
        </tr>
        <tr>
            <td>cronjob</td>
            <td>크론잡</td>
            <td>정기적으로 실행되는 배치 처리를 관리하는 컨트롤러.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/cron-jobs/">잡</a></td>
        </tr>
        <tr>
            <td>daemonset(ds)</td>
            <td>데몬셋</td>
            <td>모든 노드에 파드를 배치하는 컨트롤러.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/daemonset/">데몬셋</a></td>
        </tr>
        <tr>
            <td>replicationcontroller(rc)</td>
            <td>레플리케이션 컨트롤러</td>
            <td>파드의 레플리카 수를 제어하는 컨트롤러. 레플리카셋의 이전 버전.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/workloads/controllers/replicationcontroller/">레플리케이션 컨트롤러</a></td>
        </tr>
        <tr>
            <td>horizontalpodautoscaler(hpa)</td>
            <td>Horizontal Pod Autoscaler</td>
            <td>워크로드에 따라 파드 수를 조절하는 컨트롤러.<br>
            <a href="https://kubernetes.io/ko/docs/tasks/run-application/horizontal-pod-autoscale/">Horizontal Pod Autoscaler</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="5"></a>

##### 2-4. 볼륨 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>persistentvolume(pv)</td>
            <td>퍼시스턴트 볼륨</td>
            <td>로우 레벨 스토리지 관리.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/storage/persistent-volumes/">퍼시스턴트 볼륨</a></td>
        </tr>
        <tr>
            <td>persistentvolumeclaim(pvc)</td>
            <td>퍼시스턴트 볼륨 클레임</td>
            <td>서비스를 제공하는 파드의 IP 주소와 포트를 관리.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/services-networking/service/#%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%A0%95%EC%9D%98">엔드포인트</a></td>
        </tr>
        <tr>
            <td>storageclass(sc)</td>
            <td>스토리지 클래스</td>
            <td>스토리지의 종류<br>
            <a href="https://kubernetes.io/ko/docs/concepts/storage/storage-classes/">스토리지 클래스</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="6"></a>

##### 2-5. k8s 클러스터의 구성 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>node(no)</td>
            <td>노드</td>
            <td>k8s 클러스터의 워크로드를 실행하는 서버.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/architecture/nodes/">노드</a></td>
        </tr>
        <tr>
            <td>apiservice</td>
            <td>API 서비스</td>
            <td>마스터가 지원하는 API 서비스를 관리.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/overview/kubernetes-api/">API 서비스</a></td>
        </tr>
        <tr>
            <td>componentstatuses(cs)</td>
            <td>컴포넌트 상태</td>
            <td>scheduler, controller-manager, etcd-0 에 헬스 체크 결과를 보고<br>
            <a href="https://kubernetes.io/ko/docs/concepts/overview/components/">컴포넌트 상태</a></td>
        </tr>
        <tr>
            <td>controllerrevision</td>
            <td>컨트롤러 리비전</td>
            <td>컨트롤러의 리비전 관리<br>
            <a href="https://kubernetes.io/ko/docs/tasks/manage-daemon/rollback-daemon-set/">컨트롤러 리비전</a></td>
        </tr>
        <tr>
            <td>event</td>
            <td>이벤트</td>
            <td>k8s 클러스터에서 발생한 이벤트를 기록하고 표시하기 위한 컨트롤러<br>
            <a href="https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application-introspection/#example-debugging-pending-pods">이벤트</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="7"></a>

##### 2-6. 컨피그맵과 시크릿 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>configmap(cm)</td>
            <td>컨피그맵</td>
            <td>설정 파일을 저장.<br>
            <a href="https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/">컨피그맵</a><br>
            <a href="https://kubernetes.io/ko/docs/concepts/configuration/configmap/">컨피그맵2</a></td>
        </tr>
        <tr>
            <td>secret</td>
            <td>시크릿</td>
            <td>패스워드 등 비밀성이 필요한 정보를 저장.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/configuration/secret/">시크릿</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="8"></a>

##### 2-7. 네임스페이스 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>namespace(ns)</td>
            <td>네임스페이스</td>
            <td>k8s 클러스터를 놀리적으로 분할해서 사용.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/namespaces/">네임스페이스</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="9"></a>

##### 2-8. 역할 기반 액세스 제어(RBAC) 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>serviceaccount(sa)</td>
            <td>서비스 어카운트</td>
            <td>서비스 어카운트는 파드에서 실행되는 프로세스를 위한 어카운트. 접근 권한을 식별하기 위해 사용.<br>
            <a href="https://kubernetes.io/ko/docs/reference/access-authn-authz/service-accounts-admin/">서비스 어카운트</a></td>
        </tr>
        <tr>
            <td>role</td>
            <td>롤</td>
            <td>일련의 권한을 기술하여 롤을 정의. 롤의 유효 범위는 네임스페이스로 한정됨.<br>
            <a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/">롤</a></td>
        </tr>
        <tr>
            <td>rolebindig</td>
            <td>롤 바인딩</td>
            <td>서비스 어카운트와 롤을 바인딩<br>
            <a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings">롤 바인딩</a></td>
        </tr>
        <tr>
            <td>clusterrole</td>
            <td>클러스터 롤</td>
            <td>k8s 클러스터 전체에 유효한 롤.<br>
            <a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview">클러스터 롤</a></td>
        </tr>
        <tr>
            <td>clusterrolebinding</td>
            <td>클러스터 롤 바인딩</td>
            <td>k8s 클러스터 전체에 유효한 클러스터 롤과 서비스 어카운트를 매핑.<br>
            <a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview">클러스터 롤 바인딩</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="10"></a>

##### 2-9. 보안 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>certificatesigningrequest</td>
            <td>루트 인증서 서명 요구</td>
            <td>인증 기관(CA)에 인증서 서명 요구 작성<br>
            <a href="https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview">루트 인증서 서명 요구</a></td>
        </tr>
        <tr>
            <td>networkpoliceies(netpol)</td>
            <td>네트워크 정책</td>
            <td>네임스페이스 사이의 네트워크 접근 제어.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/services-networking/network-policies/">네트워크 정책</a></td>
        </tr>
        <tr>
            <td>podsecuritypolicies(psp)</td>
            <td>파드 보안 정책</td>
            <td>파드 보안 관련 항목의 기본값 설정.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/policy/pod-security-policy/">파드 보안 정책</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>
<a id="11"></a>

##### 2-10. 자원 관리 관련 리소스 타입
<br>
<table>
    <thead>
        <tr>
            <th colspan="1">리소스 타입(생략형)</th>
            <th colspan="1">이름</th>
            <th colspan="1">오브젝트 개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>limitrange(limits)</td>
            <td>limit range</td>
            <td>네임스페이스 내 컨테이너의 CPU 와 메모리 요구값과 상한값의 기본값 설정.<br>
            <a href="https://kubernetes.io/ko/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/">limit range</a></td>
        </tr>
        <tr>
            <td>resourcequota(quota)</td>
            <td>resource quota</td>
            <td>네임스페이스별 CPU와 메모리 요구량, 상한값 설정.<br>
            <a href="https://kubernetes.io/ko/docs/concepts/policy/resource-quotas/">resource quota</a></td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

[목차로](#home1) 
</div><br><br>
<a id="12"></a>

# 옵션

<br>
<table>
    <thead>
        <tr>
            <th colspan="1">옵션</th>
            <th colspan="1">개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-n 네임스페이스명</td>
            <td>조작 대상을 지정된 네임스페이스로 한정</td>
        </tr>
        <tr>
            <td>--all-namespaces -A(v1.14부터)</td>
            <td>모든 네임스페이스의 오브젝트를 대상으로 함</td>
        </tr>
        <tr>
            <td>-o=yaml</td>
            <td>YAML 포맷으로 API 오브젝트를 표시</td>
        </tr>
        <tr>
            <td>-o=wide</td>
            <td>추가 정보 표시(파드의 IP 주소, 배치된 노드 이름 등)</td>
        </tr>
        <tr>
            <td>-o=json</td>
            <td>JSON 형식으로 API 오브젝트를 표시</td>
        </tr>
        <tr>
            <td>-o=custom-columns=&lt spec &gt</td>
            <td>항목을 지정해서 목록 표시</td>
        </tr>
        <tr>
            <td>-o=custom-columns-file=&lt file &gt</td>
            <td>템플릿 파일로 출력할 컬럼을 지정</td>
        </tr>
        <tr>
            <td>-o=jsonpath=&lt template &gt</td>
            <td>jsonpath에 일치하는 목록 표시<br>
            <a href="https://kubernetes.io/ko/docs/reference/kubectl/jsonpath/">JsonPath</a></td>
        </tr>
        <tr>
            <td>-o=jsonpath-file=&lt filename &gt</td>
            <td>jsonpath 형식의 템플릿 파일로 출력할 내용 지정</td>
        </tr>
    </tbody>
</table>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 출처: 15단계로 배우는 도커와 쿠버네티스