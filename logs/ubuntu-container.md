# ubuntu 컨테이너 실행 로그

## ubuntu 컨테이너에서 명령 실행

```bash
docker run --rm ubuntu bash -lc 'pwd; ls -la /; echo "hello from ubuntu container"; cat /etc/os-release | head'
```

실행 결과:

```text
/
total 16
drwxr-xr-x   1 root root   6 Aug 14 06:10 .
drwxr-xr-x   1 root root   6 Aug 14 06:10 ..
-rwxr-xr-x   1 root root   0 Aug 14 06:10 .dockerenv
drwxr-xr-x   1 root root  26 Jul 24 12:48 .rock
lrwxrwxrwx   1 root root   7 Apr 20 08:46 bin -> usr/bin
drwxr-xr-x   1 root root   0 Apr 20 08:46 boot
drwxr-xr-x   5 root root 320 Aug 14 06:10 dev
drwxr-xr-x   1 root root  56 Aug 14 06:10 etc
drwxr-xr-x   1 root root  12 Jul 24 12:48 home
lrwxrwxrwx   1 root root   7 Apr 20 08:46 lib -> usr/lib
lrwxrwxrwx   1 root root   9 Apr 20 08:46 lib64 -> usr/lib64
drwxr-xr-x   1 root root   0 Jul 24 12:47 media
drwxr-xr-x   1 root root   0 Jul 24 12:47 mnt
drwxr-xr-x   1 root root   0 Jul 24 12:47 opt
dr-xr-xr-x 233 root root   0 Aug 14 06:10 proc
drwx------   1 root root  30 Jul 24 12:48 root
drwxr-xr-x   1 root root  22 Jul 24 12:48 run
lrwxrwxrwx   1 root root   8 Apr 20 08:46 sbin -> usr/sbin
drwxr-xr-x   1 root root   0 Jul 24 12:47 srv
dr-xr-xr-x  11 root root   0 Aug 14 06:10 sys
drwxrwxrwt   1 root root   0 Jul 24 12:48 tmp
drwxr-xr-x   1 root root  10 Jul 24 12:47 usr
drwxr-xr-x   1 root root  90 Jul 24 12:48 var
hello from ubuntu container
PRETTY_NAME="Ubuntu 26.04 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
```

## exec 실습

```bash
docker run -d --name exec-test ubuntu sleep 300
docker exec exec-test bash -lc 'echo "exec command executed"; ps -ef | head'
docker stop exec-test
docker rm exec-test
```

실행 결과:

```text
56e71535ffa14f3c3d6ac13138f62f75eeb0de1e85a03ce6c28d99cfb7ccf8a2
exec command executed
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  9 06:10 ?        00:00:00 sleep 300
root           7       0 66 06:10 ?        00:00:00 bash -lc echo "exec command executed"; ps -ef | head
root          15       7  0 06:10 ?        00:00:00 ps -ef
root          16       7  0 06:10 ?        00:00:00 head
exec-test
exec-test
```

## run과 exec 차이 정리

docker run은 새 컨테이너를 생성하고 실행하는 명령이다.
docker exec는 이미 실행 중인 컨테이너 안에서 추가 명령을 실행하는 명령이다.

docker run -it ubuntu bash처럼 실행한 경우, 컨테이너의 메인 프로세스가 bash이므로 exit를 입력하면 bash가 종료되고 컨테이너도 종료된다.
