<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# 쿠버네티스 설치

<br>

kubeadm이란, kubernetes에서 제공하는 기본적인 도구이며, kubernetes 클러스터를 빨리 구축하기 위한 다양한 기능을 제공한다.

<br><br>

## 목차

- [kubeadm을 활용한 설치](#1)

<br><br>

<br><br>
<a id="1"></a>

## 1. kubeadm을 활용한 설치

<br>

1. Swap을 끊다

```bash
 # 사용하지 않는 이유에 대한 글은 아래의 링크를 통해 확인 가능
 $ > swapoff -a
 # /etc/fstab 주석 처리
 #/dev/mapper/centos-swap swap                    swap    defaults        0 0
 $ > reboot
```

<a href="https://www.evernote.com/shard/s360/client/snv?noteGuid=caa3d18e-4bda-4516-9ec9-1180999015e2&noteKey=46fa507ba5b78edc&sn=https%3A%2F%2Fwww.evernote.com%2Fshard%2Fs360%2Fsh%2Fcaa3d18e-4bda-4516-9ec9-1180999015e2%2F46fa507ba5b78edc&title=191120%2Bwhy%2Bk8s%2Bdisable%2Bswap%253F">swapoff 이유</a>

<br>

2. yum update 및 도커 설치

```bash
 $ > yum update -y

 # 기존에 설치된 도커는 삭제
 $ > yum remove docker docker-engine docker.io
 $ > sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

 # Docker Repository 등록
 $ > sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

 # 도커 버전 리스트
 $ > yum list docker-ce --showduplicates | sort -r

 # 도커 특정 버전 설치
 $ > yum install docker-ce-<version-string_from_output_of_above_command>
 # ex)  yum install docker-ce-18.06.3.ce-3.el7

 $ > systemctl start docker && systemctl enable docker
```

<br>

2. -1 Docker 데몬 드라이버 교체

```bash
cat > /etc/docker/daemon.json <<EOF
{
    "exec-opts": ["native.cgroupdriver=systemd"],
    "log-driver": "json-file",
    log-opts": {
        "max-size": "100m"
    },
    "storage-driver": "overlay2",
    "insecure-registries" : ["123.123.123.123:5000"]
}
EOF

 $ > sudo mkdir -p /etc/systemd/system/docker.service.d
```

<br>

3. SELinux 설정을 permissive 모드로 변경

```bash
 $ > setenforce 0

 $ > sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

<br>

4. iptable 설정

```bash
$ > cat <<EOF >  /etc/sysctl.d/k8s.conf

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

EOF

$ sysctl --system
```

<br>

5. firewalld 비활성화

```bash
$ > systemctl stop firewalld

$ > systemctl disable firewalld
```

6. kubernetes yum repository 설정

```bash
$ > cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF
```

<br>

7. kubernetes 설치

```bash
 $ > sudo yum install -y kubeadm-1.21.5-0.x86_64 kubectl-1.21.5-0.x86_64 kubelet-1.21.5-0.x86_64 --disableexcludes=kubernetes
 $ > sudo systemctl enable kubelet && sudo systemctl start kubelet
 $ > sudo kubeadm config images pull
```

<br>

8. kubeadm 초기화

```bash
 $ > sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address='private IP'
```

9. Kubernetes 클러스터에 로컬로 액세스 할 수 있도록, 사용할 OS 유저에 설정을 추가

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

<br>

10. network 정책 적용

```bash
 $ > kubectl apply -f "calico.yaml"
```

<a href="https://lifeplan-b.tistory.com/158?category=886551">참고</a>

<br>

11. worker node 조인

```bash
 $ > sudo kubeadm join (마스터 노드 접속 가능한 IP):6443 --token (TOKEN) --discovery-token-ca-cert-hash (DISCOVERY_HASH)
```

<br>

12. token을 잊었을시

```bash
 $ > kubeadm token list
 $ > openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

<br>

13. token 만료시(24시간)

```bash
 $ > kubeadm token create
```

<br>

14. Role 설정

```bash
 $ > kubectl get nodes
 $ > kubectl edit node 'node 이름'

 vi > kubernetes.io/role: '원하는 명 변경'
```

<br>

15. ingress 등록

<a href="https://github.com/och5351/cluster/tree/main/master_yaml/ingress-keepalived">Ingress</a>

<br>

16. jenkins master pod 생성

```bash
 $ > kubectl create namespace ns-jenkins
 $ > mkdir jenkins && cd jenkins
```

<a href="https://github.com/och5351/cluster/tree/main/master_yaml/jenkins">jenkins yaml</a>

<br>
