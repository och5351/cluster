<a href="https://github.com/och5351/cluster#readme">메인으로</a>

<a id="home1"></a>

# Gitlab 과 Jenkins CI/CD
<br>

1. Jenkins Plugin 설정

<br>

    1-1. Dashboard -> Jenkins 관리 -> plugin 관리
    1-2. Git, GitLab pluging 설치

<br>

2. Jenkins Credentials 설정

<br>

    2.1. Dashboard -> Jenkins 관리 -> Manage Credentials 
    2.2. Stores scoped to Jenkins 에 Domains (global) 클릭
    2.3. Add Credentials 클릭
    2.4. Gitlab 유저 정보 입력
    
    
     * Username : gitalb 의 사용자 id (필수)
     * Password : gitlab의 사용자 password (필수)
     * ID : Credentials를 구분하는 ID (필수)
     * Description : 이 Credentials의 대한 부연설명 (선택)

<br>

3. Item 생성 및 Item 구성 설정

<br>

    3.1. Dashboard -> 새로운 Item
    3.2. 소스코드관리 -> Git 선택
    3.3. Repository URL -> gitlab url
    3.4. Credentials 설정 [2번에 작성한 Credentials]
    3.5. Jenkins build가 돌아갈 gitlab branch 지정
    3.6. 빌드 유발 지정 (Build when a change is pushged to Gitlab. Gitlab.webhook.URL;[jenkins url]) 선택
    3.7. Accepted Merge Request Events 와 Closed Request Events 를 체크
    3.8. Secret token 목록에서 Generate 로 토큰 생성

<br>

4. Gitlab 설정

<br>

    4.1. Admin 계정 접속
    4.2. 좌측 상단 Menu -> Admin
    4.3. 좌측 Dashboard Setting -> Network
    4.4. Outbound request 에 whilte list 추가(Allow requests 부분을 체크하고 본인이 접속하는 url을 입력) 후 Save Change
    4.5. Setting -> Webhooks
    4.6. 아래와 같이

     * URL : Jenkins의 URL로 Jenkins 설정 중 빌드 유발 부분에 나오는 Jenkins url 기입
     * Secret token : 빌드 유발 부분에서 Generate로 생성한 Secret token 기입
     * Trigger : 이벤트를 발생시키는 조건

