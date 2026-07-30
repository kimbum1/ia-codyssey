# 내 컴퓨터에 개발자용 '작업실' 꾸미기

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
- [x] (보너스) Docker Compose 멀티 컨테이너 구축

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

##4 트러블슈팅

**사례 1: GitHub 원격 저장소와 로컬 저장소 병합 충돌**
- **문제:** `git pull origin main` 실행 시 `fatal: Need to specify how to reconcile divergent branches` 에러 발생
- **원인 가설:** 로컬과 원격 저장소의 커밋 히스토리가 달라 Git이 병합 방식을 결정하지 못함
- **해결/대안:** `git config pull.rebase false` 명령어로 기본 병합 방식을 설정한 후, `--allow-unrelated-histories` 옵션을 주어 강제 병합 성공. 이후 Vim 편집기에서 `:wq`로 커밋 메시지 저장 후 push 완료.

* **관찰 결과 요약:** `docker run -it`로 진입한 쉘에서 `exit`를 누르면 컨테이너가 완전히 종료(Exited)되지만, `docker exec -it`를 사용해 실행 중인 컨테이너에 접속한 경우 `exit`로 빠져나와도 컨테이너는 계속 백그라운드에서 실행 상태(Up)를 유지함을 확인했습니다.

* **베이스 이미지:** `nginx:latest` (가벼운 웹 서버 구동 목적)
* **커스텀 포인트:** `COPY index.html ...` (Nginx의 기본 웰컴 페이지를 내가 만든 커스텀 HTML 파일로 덮어씌워 나만의 웹페이지를 띄우기 위함)

```
<img width="782" height="695" alt="evidence" src="https://github.com/user-attachments/assets/8e46eccf-ac2b-4dca-b1af-53c793ec030d" />

<img width="769" height="534" alt="evidence2" src="https://github.com/user-attachments/assets/945bebd0-0e81-4bd3-9e84-91cdd019e198" />

<img width="750" height="285" alt="evidence3" src="https://github.com/user-attachments/assets/57a5f396-0182-47dd-8e4b-c3d5c46503ca" />
