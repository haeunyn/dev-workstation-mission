## 바인드 마운트 검증 로그

### 1. 바인드 마운트 컨테이너 실행 명령

호스트의 `app` 디렉토리를 컨테이너 내부의 NGINX 웹 루트 디렉토리에 연결하기 위해 다음 명령어를 실행하였다.

```bash
docker run -d --name workstation-web-bind -p 18081:80 -v "$(pwd)/app:/usr/share/nginx/html:ro" nginx:alpine
```

위 명령어에서 `-v "$(pwd)/app:/usr/share/nginx/html:ro"` 옵션은 현재 작업 디렉토리의 `app` 폴더를 컨테이너의 `/usr/share/nginx/html` 경로에 바인드 마운트한다는 의미이다.  
마지막의 `ro` 옵션은 컨테이너에서 해당 디렉토리를 읽기 전용으로 사용한다는 의미이다.

---

### 2. 바인드 마운트 컨테이너 실행 결과

처음 실행 시 로컬에 `nginx:alpine` 이미지가 없어 Docker Hub에서 이미지를 내려받았다. 이후 컨테이너가 정상적으로 실행되었다.

```text
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
55afa1ecc21d: Already exists
3cd534fe98c6: Already exists
1223f016b4e4: Already exists
62bec68d7c31: Already exists
46f977ee452f: Already exists
d0008c891db4: Already exists
390dc935348d: Already exists
46519e7231d2: Already exists
Digest: sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752
Status: Downloaded newer image for nginx:alpine
8947457355a0c231cd209cb57cb4e7e6ede622f3598a44a838e93f16fe831512
```

---

### 3. 호스트 파일 수정 후 접속 확인

호스트의 `app/index.html` 파일을 수정한 뒤, 다음 명령어로 다시 접속을 확인하였다.

```bash
curl http://localhost:18081
```

실행 결과, 호스트에서 수정한 HTML 내용이 컨테이너의 NGINX 웹 서버 응답에 반영되어 출력되었다.

```html
<title>Bind Mount Updated</title>
Bind Mount Updated

This change was made on the host machine.
```

---

### 4. 결과 정리

바인드 마운트를 사용하여 호스트의 `app/index.html` 파일을 컨테이너 내부 NGINX 웹 루트 디렉토리인 `/usr/share/nginx/html`에 연결하였다.

이후 호스트에서 `index.html` 파일을 수정하자, Docker 이미지를 다시 빌드하거나 컨테이너를 새로 생성하지 않아도 변경 내용이 웹 서버 응답에 즉시 반영되었다.

이를 통해 바인드 마운트는 호스트 파일 시스템의 특정 디렉토리와 컨테이너 내부 디렉토리를 직접 연결하며, 호스트에서 발생한 파일 변경 사항이 컨테이너 내부에도 실시간으로 반영된다는 것을 확인하였다.
