# 컨테이너란?

하나의 리눅스 프로세스가 마치 전용 서버에서 동작하고 있는 것 같은 분리 상태이며, 리눅스 커널의 네임스페이스와 컨트롤 그룹(cgroup)이라는 기술을 기반으로 한다.


# 컨테이너와 가상 서버의 차이점

<table>
    <thead>
        <tr>
            <th colspan="1">특징</th>
            <th colspan="1">가상 서버</th>
            <th colspan="1">컨테이너</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>이미지 크기(CentOS 7.4의 경우)</td>
            <td>최소 1.54GB</td>
            <td>최소 0.20GB</td>
        </tr>
        <tr>
            <td>메모리 사용량</td>
            <td>기본 640MB</td>
            <td>기본 512MB</td>
        </tr>
        <tr>
            <td>벤치 마크 성능 비교</td>
            <td>65%(Xen HVM 가상 서버)</td>
            <td>90%</td>
        </tr>
        <tr>
            <td>OS 기동 시간</td>
            <td>분 단위</td>
            <td>초 단위</td>
        </tr>
    </table>
</thead>

<div align="center">
    <img src="https://user-images.githubusercontent.com/45858414/128109064-84d5312a-7640-4d85-9e60-9d674785ba56.png" width="70%" height="70%"/>
</div>

<span>Windows나 Mac에서 컨테이너를 돌리려면 리눅스 커널을 위해 가상 서버 필요(Hyper-V[windows10], HyperKit[Mac] 위에서 LinuxKit 기동).</span>
<span>LinuxKit 은 컨테이너를 실행하기 위한 경량의 리눅스 서브 시스템으로 도커(Docker), IBM, 리눅스 파운데이션(Linux Foundation), 마이크로소프트(Microsoft), ARM, 휴렛 패커드(Hewlett Packard), 인텔(Intel)과 같은 회사가 만듬</span>
<span>컨테이너는 하이버바이저상의 가상 서버에서도 사용할 수 있어 퍼블릭 클라우드의 '가상 서버'나 온프레미스의 Openstack 위에서 많이 활용 됨</span>