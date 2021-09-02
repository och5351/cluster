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
- [플레이북(Playbook) & 플레이](#7)
- [설치](#8)
- [YAML 문법](#9)
- [변수와 팩트](#10)
- [Vagrant를 활용한 제어노드 만들기](#11)

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

  * 동작 인벤토리 매개변수

<div align="center">
<table>
  <thead>
    <tr>
      <th colspan="1">이름</th>
      <th colspan="1">기본값</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>ansible_host</td>
      <td>호스트 이름</td>
      <td>SSH로 연결할 호스트 이름 또는 IP 주소</td>
    </tr>
    <tr>
      <td>ansible_port</td>
      <td>22</td>
      <td>SSH로 연결할 포트</td>
    </tr>
    <tr>
      <td>ansible_user</td>
      <td>root</td>
      <td>SSH로 연결할 사용자</td>
    </tr>
    <tr>
      <td>ansible_password</td>
      <td>(없음)</td>
      <td>SSH 인증에 사용될 패스워드</td>
    </tr>
    <tr>
      <td>ansible_connection</td>
      <td>smart</td>
      <td>앤서블이 호스트에 연결하는 방법</td>
    </tr>
    <tr>
      <td>ansible_private_key_file</td>
      <td>(없음)</td>
      <td>SSH 인증에 사용될 SSH 비밀 키</td>
    </tr>
    <tr>
      <td>ansible_shell_type</td>
      <td>sh</td>
      <td>커맨드가 사용될 셸</td>
    </tr>
    <tr>
      <td>ansible_python_interpreter</td>
      <td>/usr/bin/python</td>
      <td>호스트의 파이썬 인터프리터 경로</td>
    </tr>
    <tr>
      <td>ansible_*_interpreter</td>
      <td>root</td>
      <td>ansible_python_interpreter 처럼 다른 언어의 인터프리터 경로</td>
    </tr>
  </tbody>
</table>
</div>

<br>

* ansible_connection

  앤서블은 여러 전송 방식을 지원하며, 기본 전송 방식인 smart는 로컬에 설치된 SSH 클라이언트가 Control Persist라고 하는 기능의 지원 여부를 확인한다. Control Persist를 지원하는 경우 앤서블은 로컬 SSH 클라이언트를 사용하지만 지원하지 않는 경우는 smart 전송 방식은 Paramiko라는 파이썬 기반 SSH 클라이언트 라이브러리를 사용한다.
<br>

* ansible_shell_type
  원격 머신에 SSH 연결을 생성한 후 스크립트를 호출한다. 기본적으로 앤서블은 원격 셸이 bin/sh에 있는 Bourne 셸이라고 가정하고 Bourne 셸에서 동작하는 적절한 커맨드라인 매개변수를 생성한다.

  앤서블은 csh, fish, powershell을 해당 매개변수의 유효한 값으로 받는다. 따라서 셸 타입을 변경할 필요가 없다.

<br>

* ansible_python_interpreter
  앤서블과 함께 제공되는 모듈은 파이썬 2로 구현돼 있으므로 앤서블은 원격 시스템의 파이썬 인터프리터의 경로를 알아야 한다. 원격 호스트에 /usr/bin/python에 파이썬 2 인터프리터가 없는 경우에 이 값을 변경해야 한다. /usr/bin/python 에 파이썬 3를 설치했다면, 파이썬 3와 호환성이 없기 때문에 /usr/bin/python2로 변경해줘야 한다.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a> 

# 모듈(Module)
<br>

앤서블이 실행하는 코드 단위. 액션을 수행하는 스크립트.
각 모듈은 데이터베이스 처리, 사용자 관리, 네트워크 장치 관리 등 다양한 용도로 사용. 
단일 모듈을 호출하거나 플레이북에서 여러 모듈을 호출 할 수도 있음.

<table>
  <thead>
    <tr>
      <th colspan="1">명칭</th>
      <th colspan="1">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>apt</td>
      <td>apt 패키지 관리자를 사용해 패키지를 설치하거나 제거함</td>
    </tr>
    <tr>
      <td>copy</td>
      <td>로컬 머신의 파일을 호스트에 복사함</td>
    </tr>
    <tr>
      <td>file</td>
      <td>파일, 심볼릭 링크, 디렉터리의 속성을 설정함</td>
    </tr>
    <tr>
      <td>template</td>
      <td>템플릿에서 파일을 생성하고 호스트에 복사</td>
    </tr>
  </tbody>
</table>

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a> 

# 태스크(Task)
<br>

앤서블의 작업 단위. 애드훅(ad-hoc)명령을 사용하여 단일 작업을 한 번 실행할 수 있음.

플레이북에는 다섯 가지 태스크로 구성된 하나의 플레이가 포함된다.

ex) 태스크 예시

```yaml
- name: install nginx
  apt: >
    name=nginx
    update_cache=yes

```

ex) 태스크 이전 문법 예시

```yaml
- name: install nginx
  action: apt name=nginx update_cache=yes

```

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"></a> 

# 플레이북(Playbook) & 플레이(Play)
<br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/131465903-04b3b609-fc30-48f4-9dad-b3101279e82a.png" width="70%" height="70%" />
</div>

<div align="right"><p>플레이북 엔티티 관계</p></div>
<br><br>

* 플레이북(Playbook)

순서가 지정된 태스크 목록이 저장되어 지정된 작업을 해당 순서로 반복적으로 실행할 수 있음.
플레이 북에는 변수와 작업이 포함될 수 있고 YAML로 작성.

<br>

* 플레이(Play)

단일 플레이북을 포함하는 리스트.
모든 플레이는 1) 설정해야 할 호스트 집합, 2) 호스트에서 실행돼야 할 태스크 목록 들이 포함되어야 한다.

<br>

* name

플레이가 무엇인지 설명하는 주석. 플레이가 플레이를 시작하면 앤서블을 출력

<br>

* become

true인 경우 앤서블은 기본적으로 루트 사용자가 돼 모든 작업을 실행한다. 이는 우분투 서버를 관리할 때 유용한다. 기본적으로 루트 사용자로 SSH를 사용할 수 없기 때문

<br>

* vars

변수 및 값 리스트.

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

# YAML 문법
<br>

* 파일 시작 : ---
* 주석 : #
* 문자열 : 공백이 있을 시에는 따옴표를 사용하지 않아도 되지만 없을 시 "문자열"
* 불린 
  * 진실 : true, True, TRUE, yes, YES, on, On ,ON, y, Y
  * 거짓 : false, False, FALSE, no, No, NO, off, Off, OFF, n, N
* 리스트
  * ex) YAML 은 아래와 같이 리스트 작성
  ``` YAML
    - list[0]
    - list[1]
    - list[2]

    or

    ["list[0]", "list[1]", "list[2]"]
  ```  
  아래는 JSON
  ``` JSON
    [
      "list[0]",
      "list[1]",
      "list[2]"
    ]
  ```
* 딕셔너리 : JSON객체, 파이썬 딕셔너리, 루비의 해시와 비슷함. YAML에서는 매핑이라고 부르지만 ansible 문서는 딕셔너리라고 부름.
  * ex) YAML 은 아래와 같이 딕셔너리 작성
  ```YAML
  mapping_1: 1
  mapping_2: 2
  mapping_3: 3 

  or

  {mapping_1: 1, mapping_2: 2, mapping_3: 3}
  ```
  아래는 JSON
  
  ```JSON
  {
    "mapping_1": 1,
    "mapping_2": 2,
    "mapping_3": 3 
  }
  ```
* 라인 폴딩 : 앤서블은 여러 라인의 문자열을 한 라인으로 처리할 수 있다.
  * ex) YAML 
  ```YAML
  mapping_1: 1,
  2,
  3
  mapping_2: 2
  mapping_3: 3 

  or

  {mapping_1: 1, mapping_2: 2, mapping_3: 3}
  ```
  아래는 JSON
  
  ```JSON
  {
    "mapping_1": "1, 2, 3",
    "mapping_2": "2",
    "mapping_3": "3" 
  }
  ```
<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="10"></a>

# 변수와 팩트

### 변수

ex) 변수를 정의 하는 방법(플레이북에 vars 섹션을 추가한다.)
```yaml
vars:
  key_file: /etc/nginx/ssl/nginx.key
  cert_fil: /etc/nginx/ssl/nginx.crt
  conf_file: /etc/nmginx/sites-available/default
  server_name: localhost
```

vars_files 라는 센션을 사용하여 하나 이상의 파일에 변수를 저장할 수 있다.

ex)

```yaml
vars_files:
 - nginx.yml
```
ex) nginx.yml
```yaml
key_file: /etc/nginx/ssl/nginx.key
cert_fil: /etc/nginx/ssl/nginx.crt
conf_file: /etc/nmginx/sites-available/default
server_name: localhost
```

* 변수 등록하기

태스크 결과에 따라 변수의 값을 설정해야 하는 경우

ex) whoami

```yaml
- name: capture output of whoami coomand
  command: whoami
  register: login
```

debug 모듈을 만들어서 return 값을 확인하는 방법

```yaml
- name: show return value of command module
  hosts: server1
  tasks:
    - name: capture output of whoami coomand
      command: whoami
      register: login
    - debug: var=login
```

* Task에서 커맨드 출력 사용하기

```yaml
- name: capture output of id command
  command: id -un
  register: login
- debug: msg="Logged in as user {{ login.stdout }}"
```

* 모듈이 에러를 리턴하면 무시하기

```yaml
- name: Run myprog
  command: /opt/myprog
  register: result
  ignore_errors: True
- debug: var=result
```


<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="11"></a>

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