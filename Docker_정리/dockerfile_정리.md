<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Dockerfile
<br>

설치 스크립트를 기재하여 베이스 이미지 위에 소프트웨어 패키지를 설치하게 하는 Docker 스크립트 파일
<br>

아래와 같은 내용을 담고 있다.

1. 베이스 이미지의 리포지터리
2. 설치할 패키지
3. 애플리케이션 코드와 설정 파일
4. 컨테이너 기동 시실행될 명령어

# Dockerfile 자주 쓰이는 명령어
<br>

<table>
    <thead>
        <tr>
            <th colspan="1">커맨드</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>FROM &lt 이미지 &gt[:태그]</td>
            <td>컨테이너의 베이스 이미지를 지정</td>
        </tr>
        <tr>
            <td>RUN &lt 커맨드 &gt<br>
            RUN ["커맨드", "파라미터1", "파라미터2"]</td>
            <td>FROM의 베이스 이미지에서 커맨드를 실행</td>
        </tr>
        <tr>
            <td>ADD &lt 소스 &gt &lt 컨테이너_내_경로 &gt<br>
            ADD ["소스", ..." &lt 컨테이너_내_경로 &gt "]</td>
            <td>소스(파일, 디렉터리, tar 파일, URL)를 컨테이너 내 경로에 복사</td>
        </tr>
        <tr>
            <td>COPY &lt 소스 &gt &lt 컨테이너_내_경로 &gt<br>
            COPY ["소스",... " &lt 컨테이너_내_경로 &gt "]</td>
            <td>소스(파일, 디렉터리)를 컨테이너 내 경로에 복사</td>
        </tr>
        <tr>
            <td>ENTRYPOINT &lt 소스 &gt &lt 컨테이너_내_경로 &gt<br>
            COPY ["소스",... " &lt 컨테이너_내_경로 &gt "]</td>
            <td>소스(파일, 디렉터리)를 컨테이너 내 경로에 복사</td>
        </tr>
        <tr>
            <td>CMD ["실행가능한_것", "파라미터1", "파라미터2"]<br>
            CMD &lt 커맨드 &gt(셸 형식)<br>
            CMD ["파라미터1", "파라미터2"](ENTRYPOINT의 파라미터)</td>
            <td>컨테이너가 기동시 실행될 커맨드를 지정</td>
        </tr>
        <tr>
            <td>ENV &lt key &gt &lt value &gt<br>
            ENV %lt key &gt &lt 형식 &gt</td>
            <td>환경 변수 설정</td>
        </tr>
        <tr>
            <td>EXPOSE &lt port &gt[&lt port &gt ..]</td>
            <td>공개 포트 설정</td>
        </tr>
        <tr>
            <td>USER &lt 유저명 &gt | &lt UID &gt</td>
            <td>RUN, CMD, ENTRYPOINT 실행 유저 지정</td>
        </tr>
        <tr>
            <td>VOLUME ["/path"]</td>
            <td>공유 가능한 볼륨을 마운트</td>
        </tr>
        <tr>
            <td>WORKDIR /path</td>
            <td>RUN, CMD, ENTRYPOINT, COPY, ADD 의 작업 디렉터리 지정</td>
        </tr>
        <tr>
            <td>ARG &lt 이름 &gt[= &lt 디폴트 값 &gt]</td>
            <td>빌드할 때 넘길 인자를 정의 --build-arg &lt 변수명 &gt = &lt 값 &gt</td>
        </tr>
        <tr>
            <td>LABEL &lt key &gt = &lt value <span> </span>&gt &lt key &gt = &lt value &gt</td>
            <td>이미지의 메타데이터에 라벨을 추가</td>
        </tr>
        <tr>
            <td>MAINTAINER &lt 이름 &gt</td>
            <td>이미지의 메타데이터에 저작권을 추가</td>
        </tr>
    </tbody>
</table>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

* 참고 사항 : <a href="https://docs.docker.com/engine/reference/builder"> Docker Doc </a>

