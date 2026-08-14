# Docker 운영 명령 로그

## 컨테이너 로그 확인 명령
docker logs workstation-web

## 컨테이너 로그 확인 결과
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

## 컨테이너 리소스 사용량 확인 명령
docker stats --no-stream workstation-web

## 컨테이너 리소스 사용량 확인 결과
CONTAINER ID   NAME              CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O        PIDS
ddc11d5c489a   workstation-web   0.00%     5.641MiB / 15.67GiB   0.04%     3.59kB / 2.48kB   496kB / 8.19kB   7

## 전체 컨테이너 목록 확인 명령
docker ps -a

## 전체 컨테이너 목록 확인 결과
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS                     PORTS                                       NAMES
ddc11d5c489a   workstation-web:1.0   "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes               0.0.0.0:18080->80/tcp, [::]:18080->80/tcp   workstation-web
cef0aca93c1e   hello-world           "/hello"                 5 minutes ago   Exited (0) 5 minutes ago                                               eloquent_chatterjee
