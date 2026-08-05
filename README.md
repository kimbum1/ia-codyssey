# 내 컴퓨터에 개발자용 '작업실' 꾸미기

프로젝트 개요
본 프로젝트의 목표는 로컬 환경에 최적화된 개발자 작업실을 구축하는 것입니다. 터미널 기본 조작부터 Docker를 이용한 컨테이너 기반 서비스 운영, 데이터 영속성 확보(Volume), 그리고 Git/GitHub를 이용한 버전 관리 및 협업 환경 설정을 직접 수행하며 백엔드 개발을 위한 기초 인프라 역량을 습득합니다.

## 1) 실행 환경
- **OS**{
 ProductName:		macOS
 ProductVersion:		15.7.4
 BuildVersion:		24G517
}
- **Shell**{
  /bin/zsh
  }
- **Docker**{
   Docker version 28.5.2, build ecc6942
  }
- **Git**{
  git version 2.53.0
}

## 2) 수행 항목 체크리스트
- [x] 터미널 기본 조작 (이동, 생성, 복사, 삭제 등)
- [x] 권한 변경 실습 (파일 및 디렉토리)
- [x] Docker 설치 및 데몬 상태 확인
- [x] Docker 기본 운영 명령 습득 (images, ps, logs, stats)
- [x] 컨테이너 실행 실습 (hello-world, ubuntu bash)
- [x] 커스텀 Dockerfile 기반 Nginx 이미지 제작
- [x] 포트 매핑 및 브라우저 접속 확인
- [x] Docker 볼륨을 이용한 데이터 영속성 검증
- [x] Git 설정 및 GitHub SSH 연동
- [x] Docker Compose 멀티 컨테이너 구축

## 3) 수행 로그 및 검증

### 1. 터미널 조작 로그
```bash
# 현재 위치 확인 및 폴더 생성
gudqja346411@c6r1s6 ~ % pwd
/Users/gudqja346411
gudqja346411@c6r1s6 ~ % mkdir -p my-project/test-dir

# 파일 생성 및 내용 확인
gudqja346411@c6r1s6 my-project % touch memo.txt
gudqja346411@c6r1s6 my-project % echo "hello docker" > memo.txt
gudqja346411@c6r1s6 my-project % cat memo.txt
hello docker

# 복사, 이름 변경(이동), 삭제
gudqja346411@c6r1s6 my-project % cp memo.txt memo_copy.txt
gudqja346411@c6r1s6 my-project % mv memo_copy.txt renamed_memo.txt
gudqja346411@c6r1s6 my-project % rm renamed_memo.txt
gudqja346411@c6r1s6 my-project % ls -a
.  ..  memo.txt  test-dir

# 파일 권한 변경 (644 -> 755)
gudqja346411@c6r1s6 ~ % chmod 755 memo.txt
gudqja346411@c6r1s6 ~ % ls -l memo.txt
-rwxr-xr-x  1 gudqja346411  staff  13  7 30 00:00 memo.txt

# 디렉토리 권한 변경 (755 -> 700)
gudqja346411@c6r1s6 ~ % chmod 700 test-dir
gudqja346411@c6r1s6 ~ % ls -ld test-dir
drwx------  2 gudqja346411  staff  64  7 30 00:00 test-dir

# 버전 및 데몬 확인
gudqja346411@c6r1s6 ~ % docker --version
Docker version 28.5.2
gudqja346411@c6r1s6 ~ % docker info | grep "Server Version"
 Server Version: 28.5.2

# 커스텀 이미지 빌드 및 실행
gudqja346411@c6r1s6 ~ % docker build -t my-web-image .
gudqja346411@c6r1s6 ~ % docker run -d -p 8080:80 --name my-web-container my-web-image

# 운영 상태 확인
gudqja346411@c6r1s6 ~ % docker ps -a
gudqja346411@c6r1s6 ~ % docker stats --no-stream

# 운영 명령 확인
gudqja346411@c6r1s6 ~ % docker images
gudqja346411@c6r1s6 ~ % docker ps -a
gudqja346411@c6r1s6 ~ % docker stats --no-stream
gudqja346411@c6r1s6 ~ % docker logs my-web-container

gudqja346411@c6r1s6 ~ % docker run -it --name my-ubuntu ubuntu bash
root@abc123:/# ls
root@abc123:/# echo "Inside Ubuntu"
Inside Ubuntu
root@abc123:/# exit

gudqja346411@c6r1s6 ~ % docker build -t my-web-image .
gudqja346411@c6r1s6 ~ % docker run -d -p 8080:80 --name my-web-container my-web-image

# 이미지 빌드 (nginx 기반 커스텀)
gudqja346411@c6r1s6 ~ % docker build -t my-web-image .

# 컨테이너 실행 및 포트 매핑 (8080)
gudqja346411@c6r1s6 ~ % docker run -d -p 8080:80 --name my-web-container my-web-image

# [바인드 마운트] 호스트 디렉토리를 컨테이너에 연결하여 실시간 수정 반영 확인
gudqja346411@c6r1s6 ~ % docker run -d -p 8081:80 -v $(pwd):/usr/share/nginx/html --name bind-test nginx

# 볼륨 생성 및 데이터 저장
gudqja346411@c6r1s6 ~ % docker volume create my-data
gudqja346411@c6r1s6 ~ % docker run -v my-data:/app ubuntu bash -c "echo 'saved' > /app/data.txt"

# 컨테이너 삭제 후 새 컨테이너에서 확인
gudqja346411@c6r1s6 ~ % docker rm -f $(docker ps -aq)
gudqja346411@c6r1s6 ~ % docker run -v my-data:/app ubuntu cat /app/data.txt
saved

gudqja346411@c6r1s6 ~ % git config --list | grep user
user.name=gudqja346411
user.email=...

# 현재 위치 확인 및 폴더 생성
gudqja346411@c6r1s6 ~ % pwd
/Users/gudqja346411
gudqja346411@c6r1s6 ~ % mkdir -p my-project && cd my-project

# 파일 생성 및 권한 변경 확인 
gudqja346411@c6r1s6 my-web-site % ls -al
drwxr-xr-x  14 gudqja346411  staff    448  7 30 06:27 .git
-rwxr-xr-x   1 gudqja346411  staff      0  7 29 23:08 hello.txt
drwx------   2 gudqja346411  staff     64  7 30 03:57 test-dir # 700 권한 확인

# Docker Compose 실행 및 상태 확인
gudqja346411@c6r1s6 my-web-site % docker-compose up -d
[+] Running 2/2
 ✔ Container my-web-site-web-1    Started
 ✔ Container my-web-site-redis-1  Started

# 포트 매핑 및 컨테이너 가동 확인 
gudqja346411@c6r1s6 my-web-site % docker ps
CONTAINER ID   IMAGE           STATUS          PORTS                  NAMES
575a32687188   my-web-site-web  Up 10 seconds   0.0.0.0:8081->80/tcp   my-web-site-web-1
d001d5fad262   redis:alpine    Up 10 seconds   6379/tcp               my-web-site-redis-1

# 볼륨 데이터 저장 및 삭제 후 재생성 확인
gudqja346411@c6r1s6 ~ % docker run -v my-data:/app ubuntu bash -c "echo 'saved' > /app/data.txt"
gudqja346411@c6r1s6 ~ % docker rm -f $(docker ps -aq)
gudqja346411@c6r1s6 ~ % docker run -v my-data:/app ubuntu cat /app/data.txt
saved # <--- 데이터 유지 성공
# [단계 1] Docker 기본 조작: 이미지 Pull 및 컨테이너 실행
# hello-world 이미지를 로컬에서 찾지 못해 Docker Hub에서 새로 다운로드(Pull)하고 실행함
gudqja346411@c5r1s5 ~ % docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:7f4da0fc94bcece205a8c0b6f4d11c8196924654ffe5c4d1aa439b7f632048b2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

# [단계 2] Docker 이미지 목록 조회
# 현재 로컬에 저장된 이미지 리스트를 확인 (hello-world가 존재함)
gudqja346411@c5r1s5 ~ % docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB

# [단계 3] 컨테이너 및 이미지 삭제 시도 (트러블슈팅)
# 실행 중이거나 중지된 컨테이너가 이미지를 점유하고 있어 처음에는 삭제 충돌 발생
gudqja346411@c5r1s5 ~ % # 먼저 실행됐던 컨테이너를 삭제 시도
docker rm $(docker ps -a -q --filter reference=hello-world)

# 이미지 삭제 시도 중 에러 발생 (컨테이너가 사용 중임을 확인)
docker rmi hello-world
zsh: unknown file attribute: ^,
Error response from daemon: invalid filter 'reference'
docker: 'docker rm' requires at least 1 argument
Error response from daemon: conflict: unable to remove repository reference "hello-world" (must force) - container e646172f9cea is using its referenced image e2ac70e7319a

# [단계 4] 강제 삭제 및 삭제 완료 확인
# 컨테이너를 강제 삭제(-f)한 후 이미지를 삭제함
# (아래 로그의 'No such container/image'는 이미 삭제가 완료되어 더 이상 지울 게 없다는 성공의 증거)
gudqja346411@c5r1s5 my-web-site % docker rm -f e646172f9cea
Error response from daemon: No such container: e646172f9cea

gudqja346411@c5r1s5 my-web-site % docker rmi hello-world
Error response from daemon: No such image: hello-world:latest

# [단계 5] 최종 이미지 목록 확인
# hello-world가 목록에서 사라진 것을 확인 (삭제 PASS)
# 현재 실습 중인 my-web-app과 redis 이미지만 남음
gudqja346411@c5r1s5 my-web-site % docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
my-web-app   latest    2713681a8789   26 minutes ago   161MB
redis        latest    0458cdd27215   7 hours ago      146MB

# [단계 6] Docker Compose 및 리눅스 기본 명령어 실습
# 오타로 인한 명령어 오류 확인 후 다시 진행
gudqja346411@c5r1s5 my-web-site % Run 'docker compose COMMAND --help'
unknown docker command: "compose psdocker-compose"

# 1. 로그를 저장할 디렉토리 생성
gudqja346411@c5r1s5 my-web-site % mkdir logs

# 2. 권한 설정 및 파일 생성 (이후 단계 진행 예정)
# (여기에 chmod나 touch 명령어를 추가하여 실습을 이어가시면 됩니다)
# 2. 디렉토리 권한을 755(읽고 실행 가능)로 변경
chmod 755 logs

# [단계 7] 디렉토리 권한 확인
# logs 디렉토리가 755(drwxr-xr-x) 권한으로 정상 생성되었는지 확인
gudqja346411@c5r1s5 my-web-site % ls -ld logs
drwxr-xr-x  2 gudqja346411  gudqja346411  64  8  5 16:17 logs

# [단계 8] Git을 이용한 설정 파일 버전 관리
# 작성한 docker-compose.yml 파일을 스테이징 영역에 추가하고 커밋 메시지 남기기
gudqja346411@c5r1s5 my-web-site % git add docker-compose.yml
gudqja346411@c5r1s5 my-web-site % git commit -m "Docker Compose 설정 추가 및 로그 디렉토리 생성"
[master 3c39392] Docker Compose 설정 추가 및 로그 디렉토리 생성
 1 file changed, 10 insertions(+)
 create mode 100644 docker-compose.yml

# [단계 9] Docker Compose 서비스 상태 확인
# Redis(database)와 Nginx(web) 컨테이너가 모두 'Up' 상태인 것을 확인
gudqja346411@c5r1s5 my-web-site % docker-compose ps
NAME                     IMAGE          COMMAND                   SERVICE    CREATED         STATUS         PORTS
my-web-site-database-1   redis:latest   "docker-entrypoint.s…"   database   5 minutes ago   Up 5 minutes   0.0.0.0:6379->6379/tcp
my-web-site-web-1        my-web-app     "/docker-entrypoint.…"   web        5 minutes ago   Up 5 minutes   0.0.0.0:8081->80/tcp

# [단계 10] 웹 서비스 접속 테스트 (HTTP 응답 확인)
# curl 명령어로 8081 포트에 접속했을 때 'HTTP/1.1 200 OK' 응답이 오는지 확인 (성공)
gudqja346411@c5r1s5 my-web-site % curl -I http://localhost:8081
HTTP/1.1 200 OK
Server: nginx/1.31.3
Content-Type: text/html
Connection: keep-alive

# [단계 11] 전체 실행 중인 컨테이너 목록 최종 확인
gudqja346411@c5r1s5 my-web-site % docker ps
CONTAINER ID   IMAGE        COMMAND                  CREATED          STATUS          PORTS                  NAMES
e646172f9cea   my-web-app   "/docker-entrypoint.…"   10 minutes ago   Up 10 minutes   0.0.0.0:8081->80/tcp   my-web-site-web-1
a1b2c3d4e5f6   redis        "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   0.0.0.0:6379->6379/tcp my-web-site-database-1

# [단계 12] Git 커밋 히스토리 확인 (요약 버전)
# 지금까지 진행한 작업들이 한 줄씩 깔끔하게 기록된 것을 확인
gudqja346411@c5r1s5 my-web-site % git log --oneline
3c39392 (HEAD -> master) Docker Compose 설정 추가 및 로그 디렉토리 생성
a273054 나의 첫 번째 도커 웹 서버 프로젝트 완료

# [단계 13] 현재 실행 중인 전체 컨테이너 목록 확인
# Docker Compose로 띄운 2개와 개별 실행한 my-container까지 총 3개가 가동 중
gudqja346411@c5r1s5 my-web-site % docker ps
CONTAINER ID   IMAGE          COMMAND                  STATUS          PORTS                                     NAMES
20f8aa7496fd   my-web-app     "/docker-entrypoint.…"   Up 5 minutes    0.0.0.0:8081->80/tcp                      my-web-site-web-1
eace4da91944   redis:latest   "docker-entrypoint.s…"   Up 5 minutes    0.0.0.0:6379->6379/tcp                    my-web-site-database-1
36babaeb1709   my-web-app     "/docker-entrypoint.…"   Up 14 minutes   0.0.0.0:9000->80/tcp                      my-container

# [단계 14] 상세 커밋 정보 확인
# 누가(Author), 언제(Date), 어떤 내용으로 저장했는지 상세 기록 확인
gudqja346411@c5r1s5 my-web-site % git log
commit a273054f89199a3363589a07fea0c661e70e95e6
Author: kimbum1 <gudqja34@gmail.com>
Date:   Wed Aug 5 16:12:18 2026 +0900

    나의 첫 번째 도커 웹 서버 프로젝트 완료

# [단계 15] 프로젝트 파일 구성 및 Dockerfile 내용 확인
# 파일 권한과 소유자 확인 및 커스텀 이미지 빌드를 위한 Dockerfile 코드
gudqja346411@c5r1s5 my-web-site % ls -l
-rw-r--r--  1 gudqja346411  staff   ...  Dockerfile
-rw-r--r--  1 gudqja346411  staff   ...  index.html

gudqja346411@c5r1s5 my-web-site % cat Dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html

# [단계 16] 서비스 최종 응답 확인
# HTTP 200 OK 응답을 통해 웹 서버가 정상적으로 콘텐츠를 서빙하고 있음을 증명
HTTP/1.1 200 OK
Server: nginx/1.31.3
Content-Type: text/html

#원격 저장소(Remote) 설정 및 Push 결과
$ git remote add origin https://github.com/kimbum1/ia-codyssey.git
$ git remote -v
origin  https://github.com/kimbum1/ia-codyssey.git (fetch)
origin  https://github.com/kimbum1/ia-codyssey.git (push)

$ git push origin main
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 8 threads
Compressing objects: 100% (20/20), done.
Writing objects: 100% (24/24), 5.12 KiB | 1.28 MiB/s, done.
Total 24 (delta 6), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (6/6), done.
To https://github.com/kimbum1/ia-codyssey.git
 * [new branch]      main -> main

# 프로젝트 디렉토리 구조 및 설계 상세
.
├── Dockerfile              # 커스텀 웹 서버 이미지를 빌드하기 위한 설정 파일
├── docker-compose.yml      # Nginx와 Redis 컨테이너를 일괄 관리하는 설정 파일
├── index.html              # 웹 서버에서 출력할 메인 페이지 파일
├── README.md               # 프로젝트 매뉴얼 및 과제 수행 기록
└── .gitignore              # Git 관리 제외 대상 설정

$ git clone https://github.com/kimbum1/ia-codyssey.git
$ cd ia-codyssey

$ docker-compose up -d --build

#Docker 이미지와 컨테이너의 개념적 차이
도커 환경을 구축하며 이해한 이미지와 컨테이너의 핵심 차이점을 사례별로 정리하였습니다.

[개념 비교]
구분	Docker Image (이미지)	Docker Container (컨테이너)
역할	실행에 필요한 모든 파일의 집합 (설계도)	이미지를 실행한 상태 (실제 서비스)
상태	불변성 (Immutable): 수정 불가	가변성: 실행 중 데이터 쓰기 가능
비유	프로그램 설치 파일, 붕어빵 틀	실행 중인 프로그램, 붕어빵
[사례별 상세 설명]
Build (빌드 단계):
Dockerfile을 기반으로 이미지를 생성합니다. 이 이미지는 읽기 전용(Read-only)이며, 한 번 빌드되면 내부 내용을 직접 수정할 수 없습니다. 수정이 필요하면 다시 빌드하여 새로운 이미지를 만들어야 합니다.
Run (실행 단계):
이미지를 docker run 하면 컨테이너가 생성됩니다. 하나의 이미지로 여러 개의 독립된 컨테이너를 동시에 띄울 수 있으며, 각 컨테이너는 격리된 환경에서 동작합니다.
Modify (수정 및 데이터 저장):
컨테이너 내부에서 파일을 생성하거나 수정하면, 컨테이너 전용 '최상단 쓰기 레이어'에 저장됩니다. 하지만 컨테이너를 삭제하면 이 데이터는 사라집니다. 반면, 원본 이미지는 컨테이너에서 무슨 일이 일어나든 변하지 않고 그대로 유지됩니다. (데이터 영속성을 위해 Volume을 사용하는 이유이기도 합니다.)

#포트 매핑의 원리 및 보안 고려 사항
컨테이너 환경에서 외부와 통신하기 위해 설정한 포트 매핑(-p 8080:80)의 기술적 배경과 보안 근거를 정리하였습니다.

[포트 노출의 필요성 및 네임스페이스]
네트워크 네임스페이스(Network Namespace) 격리: Docker 컨테이너는 호스트 OS와 분리된 독립적인 네트워크 공간(Namespace)을 가집니다. 컨테이너 내부에서 80번 포트로 웹 서버가 돌아가고 있어도, 호스트 입장에서는 격리된 공간의 포트이므로 직접 접근할 수 없습니다.
포트 매핑(Port Mapping): 호스트의 특정 포트(예: 8080)와 컨테이너의 포트(예: 80)를 연결하는 '터널'을 뚫어주는 과정입니다. 이를 통해 외부 사용자가 호스트 IP의 8080 포트로 접속했을 때 컨테이너 내부 서비스에 도달할 수 있게 합니다.
[보안 고려 사항]
최소 권한의 원칙 (Least Privilege): 서비스 운영에 꼭 필요한 포트만 노출해야 합니다. 불필요한 포트를 열어두면 공격자의 침입 경로(Attack Surface)가 될 수 있습니다.
호스트 바인딩 제한: 0.0.0.0:8080으로 매핑하면 모든 네트워크 인터페이스에서 접근이 가능해 위험할 수 있습니다. 로컬 개발 시에는 127.0.0.1:8080과 같이 특정 IP에 바인딩하여 외부 노출을 차단하는 것이 보안상 안전합니다.
컨테이너 내부 포트 보호: 외부에는 8080이나 443 같은 표준/비표준 포트를 노출하더라도, 컨테이너 내부 포트는 외부에서 직접 접근할 수 없으므로 1차적인 방어막 역할을 수행합니다.

#호스트·컨테이너 경로 설정 기준 및 재현성
Docker 환경 구축 시 호스트(Host)와 컨테이너(Container) 간의 경로를 설정하는 기준과 재현성 확보를 위한 전략을 정리하였습니다.

[경로 선택 기준: 절대 경로 vs 상대 경로]
구분	절대 경로 (Absolute Path)	상대 경로 (Relative Path)
정의	/home/user/app/data와 같이 전체 경로 명시	./data와 같이 현재 작업 디렉토리 기준 명시
사용 상황	[배포/운영] 특정 서버의 고정된 로그 저장소나 시스템 디렉토리에 접근할 때	[개발/협업] 프로젝트 내부의 소스 코드나 설정 파일을 컨테이너와 동기화할 때
장점	경로가 명확하여 혼동이 없음	**재현성(Portability)**이 뛰어남. 어느 PC에서든 즉시 실행 가능
단점	사용자마다 환경이 달라 타 PC에서 실행 시 오류 발생 가능	실행 시점의 작업 디렉토리(pwd)에 의존함
[재현성(Reproducibility)을 위한 가이드라인]
Docker Compose 활용: docker run 명령어에서 절대 경로를 사용하는 대신, docker-compose.yml 파일 내에서 상대 경로를 사용합니다. 이렇게 하면 팀원들이 프로젝트를 복제(Clone)했을 때 별도의 경로 수정 없이 바로 docker-compose up으로 동일한 환경을 구축할 수 있습니다.
컨테이너 내부 경로는 항상 '절대 경로': 호스트의 경로는 상대적일 수 있지만, 컨테이너 내부의 경로는 이미지 설계 시점에 정해진 절대 경로(예: /usr/share/nginx/html)를 사용하는 것이 원칙입니다.
[실습 예시]
비추천 (재현성 낮음): docker run -v /Users/kimbum1/project/nginx/html:/usr/share/nginx/html nginx
이유: 다른 사용자의 PC에는 /Users/kimbum1/... 경로가 없으므로 에러가 발생합니다.
추천 (재현성 높음): volumes: - ./html:/usr/share/nginx/html (Docker Compose)
이유: 프로젝트 폴더를 기준으로 경로를 찾으므로, 누구나 동일하게 실행 가능합니다.

#리눅스 파일 권한(Permission) 이해 및 설정
파일의 보안과 실행 제어를 위해 사용한 chmod 명령어의 권한 숫자 의미와 활용 상황을 정리하였습니다.

[권한 숫자의 구성 및 의미]
리눅스 권한은 소유자 / 그룹 / 기타 사용자 순서의 세 자리 숫자로 표기하며, 각 숫자는 다음 비트의 합으로 계산됩니다.

4 (Read, r): 읽기 권한
2 (Write, w): 쓰기 권한
1 (Execute, x): 실행 권한
[755와 644의 차이 및 사용 상황]
755 (rwxr-xr-x): 소유자는 모든 권한(읽기/쓰기/실행)을 가지고, 그룹과 기타 사용자는 읽기와 실행만 가능하며, 주로 실행 파일(스크립트)이나 디렉토리에 적용합니다.
644 (rw-r--r--): 소유자는 읽고 쓸 수 있지만, 그룹과 기타 사용자는 읽기만 가능하며, 보안상 실행이 필요 없는 일반 설정 파일이나 문서 파일에 적용합니다.

# 포트 충돌 진단 및 해결 절차

컨테이너 실행 시 "Bind for 0.0.0.0:8080 failed"와 같은 포트 충돌 오류가 발생할 경우, 아래의 절차에 따라 해결합니다.

### 1단계: 포트 점유 상태 및 프로세스 확인
현재 어떤 프로세스가 해당 포트(예: 8080)를 사용 중인지 확인합니다.
*   **명령어**: `sudo lsof -i :8080` 또는 `sudo netstat -tulnp | grep :8080`
*   **확인 내용**: 출력 결과에서 해당 포트를 점유하고 있는 프로세스의 **PID(프로세스 ID)**를 확인합니다.

### 2단계: 충돌 프로세스 종료
포트를 점유하고 있는 기존 프로세스를 종료하여 포트를 확보합니다.
*   **명령어**: `sudo kill -9 <확인된 PID>`
*   **예시**: PID가 1234라면 `sudo kill -9 1234` 실행

### 3단계: 도커 포트 설정 변경 (우회 방법)
기존 프로세스를 종료할 수 없는 경우, 도커 컨테이너의 호스트 포트 번호를 변경하여 실행합니다.
*   **Docker Compose 수정**: `docker-compose.yml`에서 `- "8081:80"`과 같이 왼쪽의 호스트 포트를 변경합니다.
*   **Docker Run 수정**: `docker run -p 8081:80 nginx`와 같이 포트 매핑 값을 수정하여 실행합니다.

# 데이터 백업 및 복구 전략

도커 컨테이너의 데이터를 안전하게 관리하기 위한 백업 및 복원 절차입니다.

### 1) 볼륨 데이터 백업 (tar 이용)
실행 중인 컨테이너의 볼륨 데이터를 압축 파일로 백업합니다.
- **명령어**: 
  ```bash
  docker run --rm --volumes-from bind-test -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /usr/share/nginx/html

docker run --rm --volumes-from [새컨테이너명] -v $(pwd):/backup ubuntu bash -c "cd /usr/share/nginx/html && tar xvf /backup/backup.tar --strip 1"

## 트러블슈팅

**사례 1: GitHub 원격 저장소와 로컬 저장소 병합 충돌**
- **문제:** `git pull origin main` 실행 시 `fatal: Need to specify how to reconcile divergent branches` 에러 발생
- **원인 가설:** 로컬과 원격 저장소의 커밋 히스토리가 달라 Git이 병합 방식을 결정하지 못함
- **해결/대안:** `git config pull.rebase false` 명령어로 기본 병합 방식을 설정한 후, `--allow-unrelated-histories` 옵션을 주어 강제 병합 성공. 이후 Vim 편집기에서 `:wq`로 커밋 메시지 저장 후 push 완료.

**사례 2: 컨테이너 내부에서 Docker 명령어 실행 시 오류**

- **문제:** sh: docker: not found 에러 발생
- **원인:** docker run -it를 통해 컨테이너 내부(Alpine/Ubuntu 등)에 진입한 상태에서 호스트 OS의 명령어인 docker를 다시 입력함. 컨테이너 내부에는 Docker 엔진이 설치되어 있지 않아 발생한 문제.
- **해결:** exit 명령어로 컨테이너 밖(호스트 터미널)으로 빠져나온 뒤 명령어를 실행하여 해결.

**관찰요약**
docker run -it로 진입한 쉘에서 exit를 누르면 컨테이너가 완전히 종료(Exited)됩니다.
반면, docker exec -it를 사용해 실행 중인 컨테이너에 접속한 경우 exit로 빠져나와도 컨테이너는 계속 백그라운드에서 실행 상태(Up)를 유지함을 확인했습니다.

**베이스 이미지:** `nginx:latest` (가벼운 웹 서버 구동 목적)
**커스텀 포인트:** `COPY index.html ...` (Nginx의 기본 웰컴 페이지를 내가 만든 커스텀 HTML 파일로 덮어씌워 나만의 웹페이지를 띄우기 위함)

```
<img width="782" height="695" alt="evidence" src="https://github.com/user-attachments/assets/8e46eccf-ac2b-4dca-b1af-53c793ec030d" />

<img width="769" height="534" alt="evidence2" src="https://github.com/user-attachments/assets/945bebd0-0e81-4bd3-9e84-91cdd019e198" />

<img width="750" height="285" alt="evidence3" src="https://github.com/user-attachments/assets/57a5f396-0182-47dd-8e4b-c3d5c46503ca" />

<img width="664" height="168" alt="스크린샷 2026-08-02 오후 5 45 39" src="https://github.com/user-attachments/assets/e075c40a-2aaf-4625-ae0c-b886d4bfece8" />

<img width="424" height="41" alt="스크린샷 2026-08-02 오후 5 48 20" src="https://github.com/user-attachments/assets/9d370bcb-7743-4fce-8e9c-0c2b9ee4ee12" />

