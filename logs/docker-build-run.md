# Dockerfile 기반 커스텀 이미지 빌드/실행 로그

## 선택한 베이스 이미지
nginx:alpine 이미지를 베이스 이미지로 사용하였다.

## 커스텀 포인트
1. app/index.html을 NGINX 기본 웹 루트에 복사했다.
2. 컨테이너 내부 80번 포트를 사용했다.
3. Alpine 기반 NGINX 이미지를 사용했다.

## 이미지 빌드 명령
docker build -t workstation-web:1.0 .

## 이미지 빌드 결과
#0 building with "orbstack" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile:
#1 transferring dockerfile: 122B done
#1 DONE 0.2s

#2 [internal] load metadata for docker.io/library/nginx:alpine
#2 DONE 2.4s

#3 [internal] load .dockerignore
#3 transferring context: 2B done
#3 DONE 0.1s

#4 [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752
#4 resolve docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752
#4 ...

#5 [internal] load build context
#5 transferring context: 297B done
#5 DONE 0.2s

#4 [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752
#4 resolve docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752 0.2s done
#4 sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752 10.33kB / 10.33kB done
#4 sha256:1d40e3eb3bf4f138de1d67193f2aa5309fcaf343eb5ffadbf5e9439de1eb1ebb 2.50kB / 2.50kB done
#4 sha256:f0ba77f796e57c6fa89ae7f4fdad1665d6fcbd8e3f211535120542b337f9959e 12.32kB / 12.32kB done
#4 sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4 0B / 3.85MB 0.1s
#4 sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da 0B / 1.89MB 0.1s
#4 sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc2810d59 0B / 627B 0.1s
#4 sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4 1.05MB / 3.85MB 0.3s
#4 sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4 3.85MB / 3.85MB 0.3s done
#4 extracting sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4 0.1s done
#4 sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c924b142 0B / 957B 0.3s
#4 sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da 1.89MB / 1.89MB 0.5s done
#4 extracting sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da
#4 sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80 0B / 404B 0.5s
#4 sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc2810d59 627B / 627B 0.6s done
#4 sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c924b142 957B / 957B 0.7s done
#4 extracting sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da 0.1s done
#4 sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f871726f38 0B / 1.21kB 0.7s
#4 sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80 404B / 404B 0.7s done
#4 extracting sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc2810d59 done
#4 sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 0B / 20.31MB 0.8s
#4 sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f871726f38 1.21kB / 1.21kB 0.9s done
#4 extracting sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c924b142 done
#4 sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9 0B / 1.40kB 0.9s
#4 extracting sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80
#4 sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 3.15MB / 20.31MB 1.0s
#4 extracting sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80 done
#4 extracting sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f871726f38 done
#4 extracting sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9
#4 sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 20.31MB / 20.31MB 1.2s
#4 sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9 1.40kB / 1.40kB 1.1s done
#4 extracting sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9 done
#4 sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 20.31MB / 20.31MB 1.3s done
#4 extracting sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 0.1s
#4 extracting sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 0.4s done
#4 DONE 2.9s

#6 [2/2] COPY ./app/index.html /usr/share/nginx/html/index.html
#6 DONE 0.2s

#7 exporting to image
#7 exporting layers
#7 exporting layers 0.2s done
#7 writing image sha256:e1e8dfaebfb8f0394bc874fefc8372ab2a345b7c85492b3a0b003880571284f6 done
#7 naming to docker.io/library/workstation-web:1.0 0.0s done
#7 DONE 0.2s

## 이미지 목록 확인
REPOSITORY        TAG       IMAGE ID       CREATED                  SIZE
workstation-web   1.0       e1e8dfaebfb8   Less than a second ago   62.4MB
ubuntu            latest    86a1a31fdd84   2 weeks ago              100MB
hello-world       latest    e2ac70e7319a   4 months ago             10.1kB

## 컨테이너 실행 명령
docker run -d --name workstation-web -p 18080:80 workstation-web:1.0

## 컨테이너 실행 결과
ddc11d5c489a55f1740da7c33eb1a17126f73615a184912a9b70b5c2c5691383

## 실행 중인 컨테이너 확인
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS                  PORTS                                       NAMES
ddc11d5c489a   workstation-web:1.0   "/docker-entrypoint.…"   1 second ago   Up Less than a second   0.0.0.0:18080->80/tcp, [::]:18080->80/tcp   workstation-web

## curl 접속 확인
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0100   227  100   227    0     0  85338      0 --:--:-- --:--:-- --:--:--  110k
HTTP/1.1 200 OK
Server: nginx/1.31.3
Date: Fri, 14 Aug 2026 06:12:16 GMT
Content-Type: text/html
Content-Length: 227
Last-Modified: Fri, 14 Aug 2026 06:02:39 GMT
Connection: keep-alive
ETag: "6a7eaf7f-e3"
Accept-Ranges: bytes

<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Dev Workstation Mission</title>
</head>
<body>
  <h1>Hello Docker Workstation</h1>
  <p>This page is served by a custom NGINX image.</p>
</body>
</html>
