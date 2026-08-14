

## Docker 운영 명령 로그

### 1. 컨테이너 로그 확인

실행 중인 `workstation-web` 컨테이너의 로그를 확인하기 위해 다음 명령어를 실행하였다.

```bash
docker logs workstation-web
```

실행 결과는 다음과 같다.

```text
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/08/14 06:12:16 [notice] 1#1: using the "epoll" event method
2026/08/14 06:12:16 [notice] 1#1: nginx/1.31.3
2026/08/14 06:12:16 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/08/14 06:12:16 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/08/14 06:12:16 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/08/14 06:12:16 [notice] 1#1: start worker processes
2026/08/14 06:12:16 [notice] 1#1: start worker process 30
2026/08/14 06:12:16 [notice] 1#1: start worker process 31
2026/08/14 06:12:16 [notice] 1#1: start worker process 32
2026/08/14 06:12:16 [notice] 1#1: start worker process 33
2026/08/14 06:12:16 [notice] 1#1: start worker process 34
2026/08/14 06:12:16 [notice] 1#1: start worker process 35
192.168.215.1 - - [14/Aug/2026:06:12:16 +0000] "GET / HTTP/1.1" 200 227 "-" "curl/8.7.1" "-"
192.168.215.1 - - [14/Aug/2026:06:12:22 +0000] "GET / HTTP/1.1" 200 227 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
192.168.215.1 - - [14/Aug/2026:06:12:22 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://localhost:18080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.6 Safari/605.1.15" "-"
2026/08/14 06:12:22 [error] 30#30: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 192.168.215.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:18080", referrer: "http://localhost:18080/"
```

로그를 통해 NGINX가 정상적으로 시작되었고, `curl`과 웹 브라우저 요청에 대해 `/` 경로가 `200` 상태 코드로 응답한 것을 확인하였다.

```text
"GET / HTTP/1.1" 200 227
```

이는 커스텀 HTML 페이지가 정상적으로 제공되었다는 의미이다.

또한 다음 로그가 확인되었다.

```text
"GET /favicon.ico HTTP/1.1" 404
open() "/usr/share/nginx/html/favicon.ico" failed
```

이는 브라우저가 자동으로 `favicon.ico` 파일을 요청했지만 해당 파일이 없어서 발생한 로그이다.  
웹 페이지 본문 요청은 `200 OK`로 정상 처리되었으므로 실습 결과에는 문제가 없다.

---

### 2. 컨테이너 리소스 사용량 확인

실행 중인 컨테이너의 CPU, 메모리, 네트워크 사용량 등을 확인하기 위해 다음 명령어를 실행하였다.

```bash
docker stats --no-stream workstation-web
```

실행 결과는 다음과 같다.

```text
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT     MEM %     NET I/O          BLOCK I/O        PIDS
ddc11d5c489a   workstation-web   0.00%     5.641MiB / 15.67GiB   0.04%     3.59kB / 2.48kB  496kB / 8.19kB   7
```

확인 결과 `workstation-web` 컨테이너는 CPU 사용률이 `0.00%`, 메모리 사용량이 약 `5.641MiB`로 매우 적은 리소스를 사용하고 있었다.

이를 통해 NGINX 컨테이너가 정상적으로 실행 중이며, 과도한 리소스를 사용하지 않는 상태임을 확인하였다.

---

### 3. 전체 컨테이너 목록 확인

실행 중인 컨테이너뿐만 아니라 종료된 컨테이너까지 포함하여 전체 컨테이너 목록을 확인하였다.

```bash
docker ps -a
```

실행 결과는 다음과 같다.

```text
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                    PORTS                                           NAMES
ddc11d5c489a   workstation-web:1.0   "/docker-entrypoint.…"   3 minutes ago    Up 3 minutes              0.0.0.0:18080->80/tcp, [::]:18080->80/tcp       workstation-web
cef0aca93c1e   hello-world           "/hello"                 5 minutes ago    Exited (0) 5 minutes ago                                                  eloquent_chatterjee
```

조회 결과 `workstation-web` 컨테이너는 `Up` 상태로 실행 중이며, `18080:80` 포트 매핑이 정상적으로 적용되어 있음을 확인하였다.

또한 이전 실습에서 실행한 `hello-world` 컨테이너는 `Exited (0)` 상태로 종료되어 있었다.  
`Exited (0)`은 오류 없이 정상 종료되었다는 의미이다.

---

### 4. 결과 정리

Docker 운영 명령을 사용하여 실행 중인 컨테이너의 상태를 점검하였다.

- `docker logs workstation-web` 명령으로 NGINX 시작 로그와 HTTP 요청 로그를 확인하였다.
- `/` 경로 요청이 `200 OK`로 처리되어 웹 서버가 정상 동작함을 확인하였다.
- `favicon.ico` 요청은 `404`가 발생했지만, 브라우저의 자동 요청이며 실습 결과에는 문제가 없었다.
- `docker stats --no-stream workstation-web` 명령으로 CPU와 메모리 사용량을 확인하였다.
- `docker ps -a` 명령으로 실행 중인 컨테이너와 종료된 컨테이너 목록을 확인하였다.

이를 통해 커스텀 NGINX 컨테이너가 정상 실행 중이며, 로그 확인, 리소스 확인, 컨테이너 목록 조회 등 기본적인 Docker 운영 명령을 실습하였다.
