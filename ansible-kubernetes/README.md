# vagrant-kubernetes

이 Vagrant와 Ansible 코드는 학습용 멀티 노드 kubernetes 환경을 자동으로 구축하는 스크립트다. 
다음 유튜브 영상을 참고하기 바란다. 
https://www.youtube.com/watch?v=ajU5iwy4-eg


vagrant로 클러스터를 기동하면, 컴퓨터상에 가상 서버 3대가 기동하여 쿠버네티스 환경을 자동으로 구축한다. 그러면 kubectl 명령어를 사용할 수 있게 된다. 

1. master 172.16.20.11
1. node1  172.16.20.12
1. node2  172.16.20.13


## 클러스터를 기동하기 위해 필요한 소프트웨어

다음 소프트웨어가 필요하다. 

* Vagrant (https://www.vagrantup.com/)
* VirtualBox (https://www.virtualbox.org/)
* kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* git (https://kubernetes.io/docs/tasks/tools/install-kubectl/)


## 가상 머신의 호스트 환경

Vagrant와 VirtualBox가 동작하는 OS가 필요하다. 

* Windows10　
* MacOS
* Linux

이 코드를 테스트한 저자의 환경은 다음과 같다. 

* RAM: 8GB 
* 남은 저장 공간: 5GB 이상
* CPU: Intel Core i5 


## Kubernetes 기동 방법

어떤 OS에서든 깃헙에서 다음 코드를 클론해서 vagrant up을 하면 된다. 
이 명령어가 실행 중에는 가상 서버의 이미지, 컨테이너 이미지 등 큰 용량의 다운로드가 발생한다. 

~~~
git clone -b 1.14 https://github.com/takara9/vagrant-kubernetes
cd vagrant-Kubernetes
vagrant up
~~~

위 명령어를 실행하면 20분 정도 뒤에 master, node1, node2 3대의 가상 서버로 구성된 쿠버네티스 클러스터가 기동된다. 

~~~
$ vagrant ssh master
<생략>

$ kubectl get node
NAME     STATUS   ROLES    AGE    VERSION
master   Ready    master   103s   v1.14.0
node1    Ready    <none>   67s    v1.14.0
node2    Ready    <none>   80s    v1.14.0

$ kubectl version --short
Client Version: v1.14.0
Server Version: v1.14.0

$ kubectl get componentstatus
NAME                 STATUS    MESSAGE             ERROR
scheduler            Healthy   ok                  
controller-manager   Healthy   ok                  
etcd-0               Healthy   {"health":"true"}   

$ kubectl cluster-info
Kubernetes master is running at https://172.16.20.11:6443
KubeDNS is running at https://172.16.20.11:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
~~~


## kubectl 설정 방법

PC의 OS에서 kubectl 명령어를 사용하여 master상의 apiserver와 연동하기 위해서는 환경 변수 KUBECONFIG에 config 파일의 경로를 설정해야 한다. 

이 config 파일은 Ansible의 플레이북에 의해 자동 생성되며, git clone한 디렉터리의 kubeconfig에 생성된다. 

Windows10의 경우 다음과 같이 환경 변수를 설정하여 kubectl 명령어를 사용할 수 있다. 

~~~
C:\Users\Maho\tmp\vagrant-kubernetes>set KUBECONFIG=%CD%\kubeconfig\config
C:\Users\Maho\tmp\vagrant-kubernetes>kubectl get node
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   35m   v1.14.3
node1    Ready    <none>   35m   v1.14.3
node2    Ready    <none>   35m   v1.14.3
~~~

Linux/macOS에서는 git clone으로 만들어진 디렉터리에서 다음 명령어를 실행한다. 
~~~
imac:k8s_v1.14 maho$ export KUBECONFIG=`pwd`/kubeconfig/config
imac:k8s_v1.14 maho$ kubectl get node
NAME     STATUS   ROLES    AGE   VERSION
master   Ready    master   98s   v1.14.3
node1    Ready    <none>   76s   v1.14.3
node2    Ready    <none>   63s   v1.14.3
~~~

홈 디렉터리의 .kube에 config를 복사하면 환경 변수 KUBECONFIG를 설정하지 않아도 kubectl을 사용할 수 있게 된다. 

~~~
C:\Users\Maho\tmp\vagrant-kubernetes\kubeconfig>copy config C:\Users\Maho\.kube\
        1 개 파일 복사
~~~

Linux/macOS에서는 cp 명령어를 사용한다. 한번 kubectl 명령어를 실행하면 홈 디렉터리에 .kube/config가 작성되므로 복사만 하면 된다. 


## 쿠버네티스 클러스터 정지

클러스터의 가상 서버를 종료하기 위해서는 다음 명령어를 실행한다. 
다시 기동하기 위해서는 vagrant up을 실행한다. 
~~~
vagrant halt
~~~


## 쿠버네티스 클러스터 삭제 

다음 명령어를 통해 가상 서버를 삭제한다. 
가상 서버에 가한 변경 사항이 전부 삭제된다. 

~~~
vagrant destroy -f
~~~


## Kuberntes DashBoard UI 기동 방법 

Metrics server와 Dashboard가 자동으로 기동된다. 
Dashboard에 접속하기 위해서는 PC에서 접속할 수 있도록 프록시를 기동하도록 한다. 
이 프록시를 멈추기 위해서는 Ctrl-C를 입력한다. 

~~~
$ export KUBECONFIG=`pwd`/kubeconfig/config
$ kubectl proxy
~~~

그리고 브라우저에서 다음 URL로 접속한다. 

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

브라우저에는 Kubernetes Dashboard 확인 화면이 표시된다. 여기서 Kubeconfig를 선택하여 kubeconfig의 파일로써 $KUBECONFIG에 설정한 경로의 파일을 선택한다. 그리고 로그인 버튼을 클릭한다. 
이로써 Kubernetes Dashboard의 화면이 표시된다. 
