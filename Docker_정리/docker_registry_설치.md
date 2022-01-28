<a href="https://github.com/och5351/cluster#readme">메인으로</a>
<a id="home1"></a>

# Docker registry 이란?

<br>

docker hub 에 private image를 올리는 제한 때문에 private resgistry 환경을 구축할 수 있는 컨테이너 이미지


<br>

## 목차

- [Docker registry 설치](#1)
- [Docker registry push](#2)
- [k8s jenkins groovy code](#3)
- [Trouble shooting](#4)

<br><br>
<a id="1"></a>

# Docker registry 설치

<br>

1. Docker registry 이미지 pull

``` bash
docker pull registry
```

<br>

2. Docker registry 이미지 run

``` bash
docker run -it --name docker-registry -p 5000:5000 registry
```

<br>

[목차로](#home1)

<br><br>
<a id="2"></a>

# Docker registry push

<br>

1. (도커 이미지가 준비 된 상태에서) Docker tag
``` bash
docker tag hello-world ${docker registry ip}:5000/${image name}
```

<br>

2. docker push
``` bash
docker push ${docker registry ip}:5000/${image name}
```

<br>

[목차로](#home1)

<br><br>
<a id="3"></a>

# k8s jenkins groovy code

<br>

예제 ms 의 contosoair 이용 <br>
kubernetes v 1.23.2
jenkins v 2.331

<br>

``` groovy
def docker_repo = "${docker registry ip}:5000"
def docker_tag = "${docker tag}"
def docker_image = "${image name}"
def NAMESPACE = "${kubernetes namespace}"
def DATE = new Date().format("yyyy-MM-dd'T'HH:mm:ss.SSSXXX",TimeZone.getTimeZone('Asia/Seoul'));

podTemplate(label: 'jenkins-slave-pod', 
  containers: [
    containerTemplate(
      name: 'git',
      image: 'alpine/git',
      command: 'cat',
      ttyEnabled: true
    ), 
    containerTemplate(
      name: 'node', 
      image: 'node:8.16.2-alpine3.10',
      command: 'cat',
      ttyEnabled: true
    ),
    containerTemplate(
      name: 'docker',
      image: 'docker',
      command: 'cat',
      ttyEnabled: true
    ),
    containerTemplate(
      name: 'kubectl', 
      image: 'lachlanevenson/k8s-kubectl:v1.23.2', 
      command: 'cat', 
      ttyEnabled: true
    ),
  ],
  volumes: [ 
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'), 
  ]
)
{
    node('jenkins-slave-pod') { 
        stage('Clone repository') {
            container('git') {
              checkout scm
            }
        }
        
        stage('build the source code via npm') {
            container('node') {
                sh "npm install"
            }
        }
        stage('Docker build') {
            container('docker') {
              app = docker.build("${docker_tag}/${docker_image}")
            }
        }
      stage('Push image') {   
          container('docker') {
            docker.withRegistry("http://${docker_repo}/${docker_image}") {
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
          }
        }
      stage('Kubernetes deploy') {
          container('kubectl') {
              sh "sed -i.bak 's#DATE_STRING#${DATE}#' ./k8s/k8s-deployment.yaml"
              sh "kubectl get ns ${NAMESPACE} || kubectl create ns ${NAMESPACE}"
              sh "kubectl apply -f ./k8s/"
          }
      }
    }
  }   
```

<br>

[목차로](#home1)

<br><br>
<a id="4"></a>

# Trouble shooting

<br>

1. docker private registry에서 pull할 때 오류 "server gave HTTP response to HTTPS client"
 > push 했던 곳에서 나는 Error (해당 환경은 kubernetes)
<br>

쿠버네티스 환경의 Docker 환경 설정을 바꿔준다. 기본적으로 https 통신을 지원하지만, http로 통신을 하여 발생하는 에러라고 한다. docker registry가 설치된 ip:port를 적어준다.

<br>

``` bash
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
```
