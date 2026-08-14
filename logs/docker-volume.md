

## Docker 볼륨 영속성 검증 로그

### 1. 볼륨 생성 명령

컨테이너가 삭제되어도 데이터를 유지할 수 있는지 확인하기 위해 Docker 볼륨을 생성하였다.

```bash
docker volume create workstation-volume
docker volume ls
```

---

### 2. 볼륨 생성 결과

실행 결과는 다음과 같다.

```text
workstation-volume

DRIVER    VOLUME NAME
local     workstation-volume
```

`workstation-volume`이라는 이름의 Docker 볼륨이 생성되었고, `docker volume ls` 명령을 통해 목록에 정상적으로 표시되는 것을 확인하였다.

---

### 3. 첫 번째 컨테이너에서 데이터 생성

생성한 볼륨을 첫 번째 Ubuntu 컨테이너의 `/data` 디렉터리에 마운트한 뒤, 파일을 생성하였다.

```bash
docker run --name volume-test -v workstation-volume:/data ubuntu bash -lc 'echo persistent-data > /data/message.txt && cat /data/message.txt'
```

위 명령어의 의미는 다음과 같다.

- `--name volume-test`: 컨테이너 이름을 `volume-test`로 지정
- `-v workstation-volume:/data`: Docker 볼륨 `workstation-volume`을 컨테이너 내부 `/data` 경로에 연결
- `ubuntu`: Ubuntu 이미지를 사용
- `bash -lc '...'`: 컨테이너 안에서 Bash 명령 실행
- `echo persistent-data > /data/message.txt`: `/data/message.txt` 파일에 데이터 저장
- `cat /data/message.txt`: 저장된 파일 내용 확인

실행 결과는 다음과 같다.

```text
persistent-data
```

첫 번째 컨테이너에서 `/data/message.txt` 파일이 정상적으로 생성되었고, 내용이 `persistent-data`임을 확인하였다.

---

### 4. 첫 번째 컨테이너 삭제

데이터를 생성한 첫 번째 컨테이너를 삭제하였다.

```bash
docker rm volume-test
```

실행 결과는 다음과 같다.

```text
volume-test
```

컨테이너 `volume-test`는 삭제되었지만, Docker 볼륨 자체는 삭제하지 않았다.

---

### 5. 두 번째 컨테이너에서 데이터 유지 확인

같은 Docker 볼륨을 두 번째 컨테이너에 다시 마운트하여 이전에 생성한 파일이 남아 있는지 확인하였다.

```bash
docker run --name volume-test-2 -v workstation-volume:/data ubuntu bash -lc 'cat /data/message.txt'
```

실행 결과는 다음과 같다.

```text
persistent-data
```

첫 번째 컨테이너에서 생성한 `/data/message.txt` 파일의 내용이 두 번째 컨테이너에서도 그대로 출력되었다.

---

### 6. 결과 정리

Docker 볼륨 `workstation-volume`을 생성한 뒤, 첫 번째 컨테이너에서 해당 볼륨을 `/data` 경로에 마운트하고 `message.txt` 파일을 생성하였다.

이후 첫 번째 컨테이너를 삭제했지만 Docker 볼륨은 삭제되지 않았다.  
같은 볼륨을 두 번째 컨테이너에 다시 연결하자 이전에 생성한 `persistent-data` 데이터가 그대로 유지되어 있었다.

이를 통해 컨테이너의 생명주기와 데이터 저장소의 생명주기가 서로 분리되어 있음을 확인하였다.  
즉, 컨테이너는 삭제되어도 Docker 볼륨에 저장된 데이터는 유지되며, 다른 컨테이너에서 동일한 볼륨을 연결하여 계속 사용할 수 있다.
