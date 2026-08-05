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


##4 트러블슈팅

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

