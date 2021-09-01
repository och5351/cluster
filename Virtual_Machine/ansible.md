<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Ansible이란?
<br>

IT 자동화 도구(IT automation tool).

시스템 구성, 소프트웨어 배포, 지속적 배포 또는 제로 다운 타임 롤링 업데이트와 같은 고급 IT 작업 조율 가능

[앤서블 주요 특징] 
- 단순성
- 편의성
- 최소한 변경
- OpenSSH
- yaml 이용
- 멱등성

앤서블의 단순성은 개발자, 시스템 관리자, 릴리스 엔진니어, IT 관리자 등 많은 영역의 사람들이 작업할 수 있도록 지원.
앤서블은 작은 인스턴스의 설정 부터 수천 개의 인스턴스가 있는 엔터프라이즈 환경까지 관리 가능.

<br><br>

## 목차

- [Ansible 개념](#1)
- [제어 노드(Control node)](#2)
- [매니지드 노드(Managed node)](#3)
- [인벤토리(Inventory)](#4)
- [모듈(Module)](#5)
- [태스크(Task)](#6)
- [플레이북(Playbook)](#7)
- [설치](#8)
- [Vagrant를 활용한 제어노드 만들기](#9)

<br><br>

<a id="1"></a> 

# Ansible 개념
<br>

<img src="https://user-images.githubusercontent.com/45858414/130410447-6a53a51d-d990-422e-ac4f-592ac475d442.png" width="70%" height="70%"/>

<br>

#### [구성]

* 제어 노드(Control node)
* 매니지드 노드(Managed node)
* 인벤토리(Inventory)
* 모듈(Module)
* 태스크(Task)
* 플레이북(Playbook)

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a> 

# 제어 노드(Control node)
<br>

앤서블을 실행하는 노드. 
/usr/bin/ansible이나 /usr/bin/ansible-playbook 명령을 이용하여 제어 노드에서 관리 노드들을 관리. 
앤서블이 설치 되어 있으면 노트북이나, 서버급 컴퓨터를 제어 노드로 이용할 수 있음.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

# 매니지드 노드(Managed node)
<br>

앤서블로 관리하는 서버. 
매니즈드 노드는 호스트라고도 하며 앤서블은 설치 되지 않음.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a> 

# 인벤토리(Inventory)
<br>

매니지드 노드 목록. 
인벤토리 파일은 호스트 파일이라고도 하며 각 매니지드 노드에 대한 IP 주소, 호스트 정보, 변수와 같은 정보를 지정할 수 있음.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a> 

# 모듈(Module)
<br>

앤서블이 실행하는 코드 단위. 
각 모듈은 데이터베이스 처리, 사용자 관리, 네트워크 장치 관리 등 다양한 용도로 사용. 
단일 모듈을 호출하거나 플레이북에서 여러 모듈을 호출 할 수도 있음.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a> 

# 태스크(Task)
<br>

앤서블의 작업 단위. 애드훅(ad-hoc)명령을 사용하여 단일 작업을 한 번 실행할 수 있음.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"></a> 

# 플레이북(Playbook)
<br>

순서가 지정된 태스크 목록이 저장되어 지정된 작업을 해당 순서로 반복적으로 실행할 수 있음.
플레이 북에는 변수와 작업이 포함될 수 있고 YAML로 작성.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="8"></a>

# 설치
<br>

1. Python

```bash
$> pip install ansible

```

<br>

2. CentOS

```bash
$> yum install -y ansible 

```
<br>

3. Ubuntu

```bash
$> apt install ansible

```
<br>

4. MacOS

```bash
$> brew install ansible

```
<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="9"></a>

# Vagrant를 활용한 제어노드 만들기
<br>

```ruby

storage_size = 10240
bridge_if = "eth0"
vagrant_API_version = "2"
cpu_core = 2
memory_size = 2048
vm_name = "ansible"
os = "ubuntu/focal64"
os_version = "20210819.0.0"

Vagrant.configure(vagrant_API_version) do |config|

  config.vm.define vm_name do |vm1|
    vm1.vm.box = os
    vm1.vm.box_version = os_version
    vm1.vm.host_name = vm_name
    vm1.vm.network :private_network, ip: "172.16.20.11"
    vm1.vm.network :public_network,ip:  "192.168.0.5", bridge: bridge_if
    vm1.vm.provider :virtualbox do |spec|
      spec.gui = false
      spec.cpus = cpu_core
      spec.memory = memory_size 

      vdisk = "vdisk/sdb-1.vdi" 
      # CREATE DISK
      if not File.exist?(vdisk) then
        spec.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', storage_capa ]
      end
      # ATTACH DISK
      spec.customize [
        'storageattach', :id,
        '--storagectl', 'SCSI',
        '--port', 2,
        '--device', 0,
        '--type', 'hdd',
        '--medium', vdisk]
    end
    vm1.vm.provision "shell", inline: <<-SHELL
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
      apt install -y
    SHELL
  end
end

```