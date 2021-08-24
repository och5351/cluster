<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Vagrant란?
<br>

* Vagrant의 사전적인 의미는 “부랑자”/“정처없는 사람”을 뜻함. 가상 환경을 효과적으로 만들고 테스트하는 유연한 환경에서 지어진 이름이라고 명명.
* Vagrant를 통해 코드를 작성해두면, 가상화 환경을 만들고 실행하는데 있어서 빠르게 진행할 수 있다는 장점
* Vagrant는 2010년 3월에 “Mitchell Hashimoto”가 시작한 오픈소스 프로젝트

IaC (Infrastructure as Code)

인프라스트럭처 시스템은 서버, 네트워크, 보안, 스토리지 등 다양한 서비스를 안정적으로 운영하기 위해 발전했으며, 이를 관리하기 위한 복잡도 역시 증가하였다. 하나의 애플리케이션을 구성하고 이를 배포하기에 수 많은 시스템 간의 연계 및 구성이 필요하고 빠르게 변화하는 IT 산업에서 민첩성이 중요하게 대두되었다. 하지만 상대적으로 개발에 비해 인프라는 한번 구성된 이상 시스템 변경이 어렵고 느리기 때문에 프로덕션 환경에서의 기존 인프라 환경의 유지는 효율성과 속도에 영향을 끼친다.

IaC는 이를 개선하기 위해서 수정적인 관리 시스템을 탈피하고 스크립트, 코드, 자동화 툴을 사용하여 전체 인프라스트럭처 시스템을 말한다. 코드를 통해 인프라시스템을 운영하기 때문에 반복적인 작업에 있어서 자동화가 가능하고 인적 실수를 줄일 수 있다. 특히나 퍼블릭 클라우드가 대중화되면서 서버라는 자원을 누구나 쉽게 접근할 수 있고, 친숙한 코드 형태로 작성하기 때문에 인프라에 대한 자세한 내용을 신경 쓰지 않고 빠르게 애플리케이션을 개발하고 배포할 수 있다.

* <a href="https://judo0179.tistory.com/120">Vagant 설명 참조</a>

<br><br>

## 목차

- [Vagrant 설치 방법](#1)
- [Vagrant 명령어](#2)

<br><br>

<a id="1"></a> 

# Vagrant 설치 방법
<br>

* Windows

1. <a href="https://www.virtualbox.org/">버추얼박스 다운</a> 
2. <a href="https://www.vagrantup.com/downloads">Vagrant 다운</a> 
3. 다운받은 파일 설치

* Mac

```bash
$ brew install --cask virtualbox virtualbox-extension-pack vagrant

```

* Ubuntu

```bash
$ sudo apt update
$ sudo apt install virtualbox
$ curl -O https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.deb
$ sudo apt install ./vagrant_2.2.6_x86_64.deb

```

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="2"></a> 

# Vagrant 명령어
<br>

<table>
    <thead>
        <tr>
            <th colspan="1">명령어</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>vagrant init</td>
            <td>베이그런트를 프로비저닝 하기 위한 Vagrantfile을 생성한다.</td>
        </tr>
        <tr>
            <td>vagrant status</td>
            <td>베이그런트에서 관리하는 가상머신들의 상태를 확인한다.</td>
        </tr>
        <tr>
            <td>vagrant suspend</td>
            <td>베이그런트에서 관리하는 가상머신을 휴면 상태로 만든다.</td>
        </tr>
        <tr>
            <td>vagrant resume</td>
            <td>베이그런트에서 관리하는 가상머신을 복원 상태로 만든다.</td>
        </tr>
        <tr>
            <td>vagrant reload</td>
            <td>베이그런트에서 관리하는 가상머신을 재시동 시킨다.</td>
        </tr>
        <tr>
            <td>vagrant up</td>
            <td>작성된 Vagrantfile을 바탕으로 프로비저닝을 진행한다.</td>
        </tr>
        <tr>
            <td>vagrant halt</td>
            <td>베이그런트에서 관리하는 가상 머신을 종료한다.</td>
        </tr>
        <tr>
            <td>vagrant destroy</td>
            <td>베이그런트에서 관리하는 가상 머신을 삭제한다.</td>
        </tr>
        <tr>
            <td>vagrant ssh</td>
            <td>베이그런트에서 생성된 가상 머신에 ssh로 접속한다.</td>
        </tr>
        <tr>
            <td>vagrant provision</td>
            <td>베이그런트에서 관리하는 가상 머신의 설정을 변경하고 적용한다.</td>
        </tr>
    </table>
</thead>

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a> 

# Vagrant 사용법
<br>

1. 작업 directory 생성
<br>

2. vagrant init 후 파일 확인

``` bash
PS C:\Users\vm> vagrant init
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

```
<br>

3. vagrantfile 확인

``` ruby
Vagrant.configure("2") do |config|
  config.vm.box = "base"
end

```
<br>

4. vagrant 이미지 확인

<a href="https://app.vagrantup.com/boxes/search?provider=virtualbox">varant 이미지 cloud</a>
<br>

``` ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
end

```
<br>

- 초기화와 동시에 지정하는 법

``` bash
$> vagrant init ubuntu/focal64

```
<br>

5. 가상화 도구 지정

``` ruby
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config| 
  config.vm.box = "ubuntu/focal64"
  config.vm.box_version = "20210819.0.0" # 이미지 버전을 명시 하여준다.

  config.vm.provider "virtualbox" do |machine| # 가상화 도구를 지정하여 준다.
    machine.memory = 4096 # 머신의 메모리 설정 (예: 4GB)
    machine.cpus = 4 # CPU 코어 설정 (예: 4개)
  end
end

```
<br>

6. Hostname과 가상머신 이름 지정
```ruby
Vagrant.configure("2") do |config|
    config.vm.define "master" do |master|
        master.vm.box = "ubuntu/focal64"
        master.vm.host_name = "master"
        master.vm.provider :virtualbox do |spec|
            spec.cpus = 1 # cpu
            spec.memory = 2048 # memory
        end
    end
end

```
<br>

7. Multi VM 설정 법

``` ruby
Vagrant.configure("2") do |config|

config.vm.define "frontend" do |frontend| # 가상머신 이름
    frontend.vm.box = "ubuntu/bionic64" # 가상머신 이미지
    frontend.vm.host_name = "front" # 가상머신 호스트 네임
    frontend.vm.provider :virtualbox do |spec|
        spec.cpus = 1 # cpu
        spec.memory = 1024
        end
    end

    config.vm.define "backend" do |backend|
        backend.vm.box = "ubuntu/focal64"
        backend.vm.host_name = "api"
        backend.vm.provider :virtualbox do |spec|
            spec.cpus = 2
            spec.memory = 2048
        end
    end

    config.vm.define "db" do |db|
        db.vm.box = "generic/fedora33"
        db.vm.host_name = "db"
        db.vm.provider :virtualbox do |spec|
            spec.cpus = 4
            spec.memory = 4096
        end
    end

    config.vm.define "file" do |file|
        file.vm.box = "generic/centos8"
        file.vm.host_name = "file"
        file.vm.provider :virtualbox do |spec| 
            spec.cpus = 2
            spec.memory = 2048
        end
    end


    # 따로 cpu 갯수와 memory를 적지 않았다면, 기본으로 설정될 값
    config.vm.provider "virtualbox" do |spec|
        spec.cpus = 2
        spec.memory = 1024
    end
end

```
<br>

8. 가상머신 Bridge를 통한 Public IP 만들기 (공유기에서 포트포워딩이 필요)
<br>

브릿지를 만들 시 랜카드 선택하는 항목이 나오므로 반드시 입력.

```ruby
bridge_if = "eth0"

Vagrant.configure("2") do |config|

  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.box_version = "20210819.0.0"
    master.vm.host_name = "master"
    master.vm.network :private_network, ip: "172.16.20.11" # Private IP
    master.vm.network :public_network,ip:  "192.168.0.9", bridge: bridge_if # 대역에 맞춰서 Public IP 지정
    master.vm.provider :virtualbox do |spec|
      spec.gui = false
      spec.cpus = 2
      spec.memory = 2048 

    end
  end
end

```
<br>

- rsa private key 

```bash
$> vagrant ssh-config
Host master
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/dhcks/kubecluster/vm/.vagrant/machines/master/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL

$> cat C:/Users/dhcks/kubecluster/vm/.vagrant/machines/master/virtualbox/private_key 
# 이하 내용 복사 후 메모장이나 vi 로 00.pem 파일 만들어주기
# 반드시 400권한 지정 
# chmod 400 00.pem
$> ssh -i 00.pem vagrant@192.168.0.9 
```

9. Storage 지정

```ruby
bridge_if = "eth0"

Vagrant.configure("2") do |config|

  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.box_version = "20210819.0.0"
    master.vm.host_name = "master"
    master.vm.network :private_network, ip: "172.16.20.11"
    master.vm.network :public_network,ip:  "192.168.0.9", bridge: bridge_if
    master.vm.provider :virtualbox do |spec|
      spec.gui = false
      spec.cpus = 2
      spec.memory = 2048 

      vdisk = "vdisk/sdb-1.vdi" 
      # CREATE DISK
      if not File.exist?(vdisk) then
        spec.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 20480 ]
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
  end
end

```
<br>

> Vagrant was unable to mount VirtualBox shared folders. This is usually
> because the filesystem "vboxsf" is not available. This filesystem is
> made available via the VirtualBox Guest Additions and kernel module.
> Please verify that these guest additions are properly installed in the
> guest. This is not a bug in Vagrant and is usually caused by a faulty
> Vagrant box. For context, the command attempted was:
>
> mount -t vboxsf -o uid=1000,gid=1000,_netdev vagrant /vagrant
>
> The error output from the command was:
>
> /sbin/mount.vboxsf: mounting failed with the error: Invalid argument


10.  위 에러 발생 시 플러그인 설치
``` bash
$> vagrant plugin install vagrant-vbguest

```
<br>

11. ssh 접속 관련 설정 shell script 추가 (vagrant/vagrant)

```ruby
bridge_if = "eth0"

Vagrant.configure("2") do |config|

  config.vm.define "master" do |master|
    master.vm.box = "ubuntu/focal64"
    master.vm.box_version = "20210819.0.0"
    master.vm.host_name = "master"
    master.vm.network :private_network, ip: "172.16.20.11"
    master.vm.network :public_network,ip:  "192.168.0.9", bridge: bridge_if
    master.vm.provider :virtualbox do |spec|
      spec.gui = false
      spec.cpus = 2
      spec.memory = 2048 

      vdisk = "vdisk/sdb-1.vdi" 
      # CREATE DISK
      if not File.exist?(vdisk) then
        spec.customize [
           'createmedium', 'disk',
           '--filename', vdisk,
           '--format', 'VDI',
           '--size', 20480 ]
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
    master.vm.provision "shell", inline: <<-SHELL
      sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config 
      sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
    SHELL
  end
end

```
<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>


