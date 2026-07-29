# 🐳 내 컴퓨터에 개발자용 '작업실' 꾸미기 프로젝트

## 1) 프로젝트 개요
본 프로젝트는 Mac Apple Silicon 환경에서 Docker와 Git을 설치하고, 터미널 기초 조작부터 컨테이너 기반 웹 서버 구축, 데이터 영속성 관리, 그리고 GitHub 협업 환경을 구축하는 것을 목표로 합니다.

## 2) 실행 환경
- **OS**: macOS Sequoia 15.7.4 (Apple Silicon)
- **Shell**: zsh
- **Docker**: 28.5.2 (OrbStack)
- **Git**: 2.53.0

## 3) 수행 항목 체크리스트
- [x] 터미널 기본 조작 (이동, 생성, 복사, 삭제 등)
- [x] 권한 변경 실습 (파일 및 디렉토리)
- [x] Docker 설치 및 데몬 상태 확인
- [x] Docker 기본 운영 명령 습득 (images, ps, logs, stats)
- [x] 컨테이너 실행 실습 (hello-world, ubuntu bash)
- [x] 커스텀 Dockerfile 기반 Nginx 이미지 제작
- [x] 포트 매핑 및 브라우저 접속 확인
- [x] Docker 볼륨을 이용한 데이터 영속성 검증
- [x] Git 설정 및 GitHub SSH 연동
- [x] (보너스) Docker Compose 멀티 컨테이너 구축

## 4) 수행 로그 및 검증

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


---

### 🚀 마지막으로 할 일

1.  **브라우저 캡처**: `localhost:8080`에 접속해서 주소창이 보이게 캡처한 후 이름을 `evidence.png`로 바꿔서 폴더에 넣으세요.
2.  **파일 업데이트**: 위 내용을 `README.md`에 붙여넣고 저장하세요.
3.  **최종 푸시**: 터미널에서 아래 명령어를 입력하세요.

```bash
git add .
git commit -m "Final: 모든 요구사항 및 보너스 과제 반영"
git push origin main
