네, 좋아요. 아래 내용을 **보고서에 바로 붙여넣기** 하면 됩니다.

---

## Dockerfile 기반 커스텀 이미지 실행 및 접속 확인 로그

### 1. 이미지 목록 확인

Dockerfile을 기반으로 빌드한 커스텀 이미지가 정상적으로 생성되었는지 확인하기 위해 이미지 목록을 조회하였다.

```bash
docker images
```

실행 결과는 다음과 같다.

```text
REPOSITORY        TAG       IMAGE ID       CREATED                  SIZE
workstation-web   1.0       e1e8dfaebfb8   Less than a second ago   62.4MB
ubuntu            latest    86a1a31fdd84   2 weeks ago              100MB
hello-world       latest    e2ac70e7319a   4 months ago             10.1kB
```

조회 결과 `workstation-web` 이미지가 `1.0` 태그로 생성되었으며, 이미지 크기는 `62.4MB`임을 확인하였다.

---

### 2. 컨테이너 실행 명령

생성한 `workstation-web:1.0` 이미지를 사용하여 컨테이너를 백그라운드에서 실행하였다.

```bash
docker run -d --name workstation-web -p 18080:80 workstation-web:1.0
```

위 명령어의 의미는 다음과 같다.

- `-d`: 컨테이너를 백그라운드에서 실행
- `--name workstation-web`: 컨테이너 이름을 `workstation-web`으로 지정
- `-p 18080:80`: 호스트의 18080번 포트를 컨테이너 내부의 80번 포트와 연결
- `workstation-web:1.0`: 실행할 Docker 이미지 이름과 태그

---

### 3. 컨테이너 실행 결과

컨테이너 실행 결과 다음과 같이 컨테이너 ID가 출력되었다.

```text
ddc11d5c489a55f1740da7c33eb1a17126f73615a184912a9b70b5c2c5691383
```

컨테이너 ID가 출력되었으므로 컨테이너가 정상적으로 생성 및 실행된 것을 확인하였다.

---

### 4. 실행 중인 컨테이너 확인

실행 중인 컨테이너 상태를 확인하기 위해 다음 명령어를 실행하였다.

```bash
docker ps
```

실행 결과는 다음과 같다.

```text
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                    PORTS                                           NAMES
ddc11d5c489a   workstation-web:1.0   "/docker-entrypoint.…"   1 second ago     Up Less than a second     0.0.0.0:18080->80/tcp, [::]:18080->80/tcp       workstation-web
```

`workstation-web` 컨테이너가 실행 중이며, 호스트의 `18080` 포트가 컨테이너의 `80` 포트로 정상적으로 매핑된 것을 확인하였다.

---

### 5. curl 접속 확인

호스트에서 컨테이너의 NGINX 웹 서버에 접속되는지 확인하기 위해 다음 명령어를 실행하였다.

```bash
curl -i http://localhost:18080
```

실행 결과는 다음과 같다.

```text
HTTP/1.1 200 OK
Server: nginx/1.31.3
Date: Fri, 14 Aug 2026 06:12:16 GMT
Content-Type: text/html
Content-Length: 227
Last-Modified: Fri, 14 Aug 2026 06:02:39 GMT
Connection: keep-alive
ETag: "6a7eaf7f-e3"
Accept-Ranges: bytes

<title>Dev Workstation Mission</title>
Hello Docker Workstation

This page is served by a custom NGINX image.
```

응답 코드가 `HTTP/1.1 200 OK`로 표시되었으므로 웹 서버가 정상적으로 응답하고 있음을 확인하였다.  
또한 응답 본문에 `Hello Docker Workstation` 문구가 출력되어, Dockerfile에서 복사한 `app/index.html` 파일이 NGINX를 통해 정상적으로 제공되고 있음을 확인하였다.

---

### 6. 결과 정리

Dockerfile을 기반으로 생성한 `workstation-web:1.0` 이미지가 Docker 이미지 목록에 정상적으로 표시되었다.  
이후 해당 이미지를 사용하여 `workstation-web` 컨테이너를 실행하였고, `docker ps` 명령어를 통해 컨테이너가 정상 실행 중임을 확인하였다.

또한 포트 매핑 `18080:80` 설정을 통해 호스트의 `localhost:18080` 주소로 접속했을 때 컨테이너 내부 NGINX 웹 서버로 연결되는 것을 확인하였다.  
`curl -i http://localhost:18080` 실행 결과 `HTTP/1.1 200 OK` 응답과 함께 커스텀 HTML 페이지 내용이 출력되었다.

이를 통해 Dockerfile 기반 커스텀 이미지 빌드, 컨테이너 실행, 포트 매핑, 웹 서버 접속 확인이 모두 정상적으로 완료되었음을 확인하였다.
