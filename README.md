
cat << 'EOF' > README.md
# 1. 환경 정보
- **OS** {
  macOS Sequoia 15.7.4
  ProductName: macOS
  ProductVersion: 15.7.4
  BuildVersion: 24G517
}

- **Shell** {
  zsh (/bin/zsh)
}

- **Docker** {
  Docker version 28.5.2
}

- **Git** {
  git version 2.53.0
}

## 2. 터미널 기초 실습 (chmod 권한 변경)
파일의 권한을 확인하고 변경하는 실습을 진행하였습니다.
```text
gudqja346411@c6r1s6 my-web-site % ls -l hello.txt
-rw-r--r--  1 gudqja346411  gudqja346411  0  7 29 23:08 hello.txt
gudqja346411@c6r1s6 my-web-site % chmod 755 hello.txt
gudqja346411@c6r1s6 my-web-site % ls -l hello.txt
-rwxr-xr-x  1 gudqja346411  gudqja346411  0  7 29 23:08 hello.txt

## 2. 터미널 기초 실습 (chmod 권한 변경)
파일의 권한을 확인하고 변경하는 실습을 진행하였습니다.
```text
gudqja346411@c6r1s6 my-web-site % ls -l hello.txt
-rw-r--r--  1 gudqja346411  gudqja346411  0  7 29 23:08 hello.txt
gudqja346411@c6r1s6 my-web-site % chmod 755 hello.txt
gudqja346411@c6r1s6 my-web-site % ls -l hello.txt
-rwxr-xr-x  1 gudqja346411  gudqja346411  0  7 29 23:08 hello.txt

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

gudqja346411@c6r1s6 my-web-site % docker build -t my-web-image .
[+] Building 1.8s (7/7) FINISHED                                                               docker:orbstack
 => [internal] load build definition from Dockerfile                                                      0.1s
 => => transferring dockerfile: 104B                                                                      0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                           0.8s
 => [internal] load .dockerignore                                                                         0.1s
 => => transferring context: 2B                                                                           0.0s
 => [internal] load build context                                                                         0.1s
 => => transferring context: 283B                                                                         0.0s
 => CACHED [1/2] FROM docker.io/library/nginx:latest@sha256:5a88c9c45479443d7be2eadc894b4ed0a9801bae03d9  0.0s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                0.2s
 => exporting to image                                                                                    0.2s
 => => exporting layers                                                                                   0.1s
 => => writing image sha256:8036ba174c6645bf69e62a8bed1c3d60b87af2b417bb8c9a6aec0858127e7aa8              0.0s
 => => naming to docker.io/library/my-web-image                                                           0.0s

gudqja346411@c6r1s6 my-web-site % docker run -d -p 8080:80 --name my-web-container my-web-image
30363b6d5f53b0f09ae11e7936f4c5d45e81b37562d6e59f5e422b2c580f18e2
gudqja346411@c6r1s6 my-web-site % docker ps -a
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS                   PORTS                                     NAMES
30363b6d5f53   my-web-image   "/docker-entrypoint.…"   13 seconds ago   Up 12 seconds            0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-container
44b636675124   hello-world    "/hello"                  3 hours ago      Exited (0) 3 hours ago                                             naughty_germain
29996046c0a8   hello-world    "/hello"                  5 hours ago      Exited (0) 5 hours ago                                             gracious_volhard


---


git add README.md
git commit -m "최종 제출"
git push origin main


