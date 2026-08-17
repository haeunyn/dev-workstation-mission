# Dev Workstation Mission

개발 워크스테이션 구축 실습 프로젝트입니다.  
터미널 기본 명령, 파일 권한, Docker, Docker 볼륨, Git 설정을 실습하고 로그로 정리했습니다.

## 프로젝트 구조

- `app/index.html`: NGINX에서 제공할 HTML 파일
- `Dockerfile`: 커스텀 NGINX 이미지 빌드 파일
- `logs/`: 실습 과정과 실행 결과 기록
- `screenshots/`: 브라우저 접속 결과 등 스크린샷 저장 위치

## 실습 내용

### 1. 터미널 기본 명령

다음 명령을 실습했습니다.

- `pwd` print working directory의 약자로, 현재 터미널이 위치한 디렉토리의 경로를 확인할 때 사용
- `ls` 현재 디렉토리에 있는 파일과 폴더 목록을 확인하는 명령어
- `mkdir` 새로운 디렉토리를 생성할 때 사용
- `touch` 새로운 빈 파일을 만들 때 사용
- `cp` 파일이나 디렉토리를 복사할 때 사용
- `mv` 파일이나 디렉토리를 이동할 때 사용하며, 파일 이름을 변경할 때도 사용가능함 
- `rm` 파일을 삭제할 때 사용. 단, rm 명령어로 삭제한 파일은 복구가 어려울 수 있으므로 주의해야 함

로그 파일:

- `logs/terminal-basic.md`

### 2. 파일 및 디렉토리 권한

리눅스 파일 시스템의 권한 구조(소유자-Owner, 그룹-Group, 기타 사용자-Others)를 이해하고, `chmod` 명령어로 권한을 적용하며 각 8진수 숫자의 의미를 정리했습니다.

* **권한 숫자의 구성 (8진수 표기법)**:
  * `r` (Read, 읽기) = `4`
  * `w` (Write, 쓰기) = `2`
  * `x` (Execute, 실행) = `1`

* **실습한 주요 권한 모드**:
  * `755` (`rwxr-xr-x`):
    * **소유자**: 읽기, 쓰기, 실행 모두 가능 (`7 = 4+2+1`)
    * **그룹 / 기타**: 읽기 및 실행만 가능 (`5 = 4+0+1`)
    * **주요 용도**: 실행 파일, 스크립트, 일반 디렉토리 기본 권한 (누구나 디렉토리에 접근/조회할 수 있지만 소유자만 내부 수정 가능)
  * `700` (`rwx------`):
    * **소유자**: 읽기, 쓰기, 실행 모두 가능 (`7`)
    * **그룹 / 기타**: 모든 접근 권한 없음 (`0`)
    * **주요 용도**: 개인 전용 디렉토리, 보안이 중요한 스크립트, SSH 키 저장 폴더 (`~/.ssh`)
  * `644` (`rw-r--r--`):
    * **소유자**: 읽기 및 쓰기 가능 (`6 = 4+2`)
    * **그룹 / 기타**: 읽기만 가능 (`4 = 4+0+0`)
    * **주요 용도**: 일반 문서 및 소스 코드 파일 기본 권한 (누구나 내용을 읽을 수 있지만 수정은 소유자만 가능)

> ** 디렉토리 권한 추가 검증**:  
> 파일과 달리 디렉토리에서의 `x`(실행) 권한은 '디렉토리 내부로 진입(`cd`)할 수 있는 권한'을 의미하며, `r`(읽기) 권한은 '내부 파일 목록(`ls`)을 조회할 수 있는 권한'임을 실습을 통해 확인했습니다.

로그 파일:

- `logs/permission.md`

### 3. Docker 기본 실습

Docker 데몬 동작 상태를 확인하고, 공식 테스트 이미지(`hello-world`) 및 대중적인 대화형 OS 이미지(`ubuntu`)를 실행하며 Docker의 기본 동작 원리와 컨테이너 격리 구조를 검증했습니다.

* **Docker 환경 확인**:
  * `docker version`, `docker info` 명령을 통해 Docker 데몬이 정상 작동 중임을 확인했습니다.

* **`hello-world` 컨테이너 실행**:
  * `docker run hello-world` 명령을 실행했습니다.
  * 로컬에 이미지가 없을 때 Docker Hub에서 자동으로 다운로드(`Unable to find image locally` -> `Pulling from library/hello-world`)하여 컨테이너를 생성/실행하고 종료되는 전체 흐름을 확인했습니다.

* **`ubuntu` 컨테이너 상호작용(Interactive) 실습**:
  * `docker run -it --name ubuntu-test ubuntu bash` 명령으로 우분투 컨테이너를 생성하고 터미널에 대화형으로 접속했습니다.
  * 컨테이너 내부에서 `apt-get update`, `cat /etc/os-release` 등을 실행하여 호스트 OS와 완전히 격리된 독립적인 파일 시스템과 프로세스 공간을 가짐을 검증했습니다.
  * `exit` 명령으로 터미널을 빠져나온 후, `docker ps -a`로 컨테이너의 상태(`Exited`)를 확인하고 `docker rm ubuntu-test`로 사용한 컨테이너를 정리했습니다.

> ** 핵심 검증 내용**:  
> - **이미지(Image) vs 컨테이너(Container)**: 이미지는 읽기 전용(Read-Only) 템플릿이며, 컨테이너는 이 이미지 위에 격리된 실행 환경(Read-Write 레이어)이 얹어진 프로세스임을 확인했습니다.
> - **`-it` 옵션의 의미**: `-i` (Interactive, Stdin 열기)와 `-t` (TTY, 가상 터미널 할당) 옵션을 함께 사용하여 컨테이너 내부 셸과 실시간으로 상호작용하는 방법을 익혔습니다.

로그 파일:

- `logs/docker-basic.md`
- `logs/hello-world.md`
- `logs/ubuntu-container.md`

### 4. 커스텀 NGINX 이미지 빌드 및 실행

경량화된 `nginx:alpine` 베이스 이미지를 바탕으로 커스텀 HTML 파일을 포함하는 독자적인 웹 서버 이미지를 빌드하고, 포트 매핑을 통해 호스트에서 컨테이너 내부 웹 서비스에 접속하는 실습을 진행했습니다.

* **Dockerfile 작성 및 이미지 빌드**:
  * 호스트의 `app/index.html` 파일을 컨테이너 내부 NGINX 기본 웹 루트 경로(`/usr/share/nginx/html/index.html`)로 복사하도록 `Dockerfile`을 작성했습니다.
  * `docker build -t workstation-web:1.0 .` 명령을 수행하여 커스텀 태그(`1.0`)를 가진 이미지를 빌드했습니다.

* **컨테이너 백그라운드 실행 및 포트 매핑**:
  * `docker run -d --name workstation-web -p 18080:80 workstation-web:1.0` 명령으로 컨테이너를 실행했습니다.
  * `-d` (Detached 모드) 옵션으로 백그라운드에서 실행되도록 하고, `-p 18080:80` 포트 매핑을 적용하여 **호스트의 18080번 포트 요청을 컨테이너 내부 NGINX의 80번 포트로 포워딩**했습니다.

* **서비스 응답 검증**:
  * `curl http://localhost:18080` 및 브라우저 접속을 통해 NGINX가 커스텀 `app/index.html` 페이지를 정상적으로 응답하는지 확인했습니다.
  * `docker logs workstation-web`으로 컨테이너 내부 NGINX 접속 및 접근 로그(Access Log)를 조회해 검증했습니다.

> ** 핵심 검증 내용**:  
> - **포트 매핑(`-p 호스트:컨테이너`)**: 호스트 네트워크와 컨테이너 격리 네트워크 간의 통신 통로를 개설하는 포트 포워딩 원리를 이해했습니다.
> - **레이어 기반 이미지 빌드**: Dockerfile의 각 명령(`FROM`, `COPY` 등)이 하나의 Read-Only 레이어로 추가되며 빌드 캐시(Build Cache)가 작동함을 확인했습니다.

로그 파일:

- `logs/docker-build-run.md`
- `logs/docker-operation.md`

### 5. 바인드 마운트 검증

호스트의 `app/index.html`을 컨테이너 내부 NGINX 웹 루트에 연결했습니다.  
호스트 파일을 수정하면 컨테이너 웹 서버에 즉시 반영되는 것을 확인했습니다.

로그 파일:

- `logs/bind-mount.md`

### 6. Docker 볼륨 영속성 검증

Docker 볼륨을 생성하고 첫 번째 컨테이너에서 데이터를 저장한 뒤, 컨테이너를 삭제했습니다.  
이후 같은 볼륨을 두 번째 컨테이너에 연결하여 데이터가 유지되는지 확인했습니다.

볼륨 생성:

```bash
docker volume create workstation-volume
docker volume ls
```

첫 번째 컨테이너에서 데이터 생성:

```bash
docker run --name volume-test -v workstation-volume:/data ubuntu bash -lc 'echo persistent-data > /data/message.txt && cat /data/message.txt'
```

실행 결과:

```text
persistent-data
```

첫 번째 컨테이너 삭제:

```bash
docker rm volume-test
```

두 번째 컨테이너에서 데이터 유지 확인:

```bash
docker run --name volume-test-2 -v workstation-volume:/data ubuntu bash -lc 'cat /data/message.txt'
```

실행 결과:

```text
persistent-data
```

결과적으로 첫 번째 컨테이너를 삭제해도 Docker 볼륨에 저장된 데이터는 유지되었습니다.  
이를 통해 컨테이너 생명주기와 데이터 저장소 생명주기가 분리된다는 것을 확인했습니다.

로그 파일:

- `logs/docker-volume.md`

### 7. Git 설정

커밋(Commit) 작성자 식별을 위한 사용자 정보 설정과, 표준 버전 관리 흐름에 맞춘 기본 브랜치 이름 변경 등 개발 환경 기본 Git 구성을 진행했습니다.

* **Git 사용자 정보(Identity) 설정**:
  * 커밋 메타데이터에 기록될 이름과 이메일 주소를 글로벌(`--global`) 옵션으로 등록했습니다.
  ```bash
  git config --global user.name "사용자이름"
  git config --global user.email "사용자이메일@example.com"
  
## 실행 방법

### 커스텀 NGINX 이미지 빌드
#### 1. 이미지 빌드
```bash
docker build -t workstation-web:1.0 .
```
#### 2. 컨테이너 실행 및 접속 확인
```bash
docker run -d --name workstation-web -p 18080:80 workstation-web:1.0
curl http://localhost:18080
```
#### 3. 컨테이너 중지 및 정리
```bash
docker stop workstation-web
docker rm workstation-web
```

## 배운 점

- 터미널 명령으로 파일과 디렉토리를 관리하는 방법을 익혔습니다.
- 파일 권한 숫자 표기법의 의미를 이해했습니다.
- Docker 이미지와 컨테이너의 차이를 실습으로 확인했습니다.
- 포트 매핑을 통해 컨테이너 웹 서버에 접속했습니다.
- 바인드 마운트와 Docker 볼륨의 차이를 이해했습니다.
- Git 설정과 커밋 과정을 실습했습니다.


### a. 절대 경로 vs 상대 경로

* **절대 경로**: 뿌리(최상위 디렉토리 `/`)부터 목적지까지의 **전체 주소**입니다. 어디서 실행하든 항상 같은 위치를 가리킵니다.
* *예시*: `/Users/username/documents/project/main.py`


* **상대 경로**: **현재 내가 위치한 디렉토리**를 기준으로 찾아가는 주소입니다.
* *예시*: `../project/main.py` (`..`은 상위 폴더, `.`은 현재 폴더를 의미)



---

### b. 파일 권한 (r/w/x) 및 숫자 표기법 (755, 644)

파일 권한은 **소유자(Owner) - 그룹(Group) - 기타 사용자(Others)** 순서로 적용되며, 각 자리는 2진수 스위치의 합으로 계산합니다.

* **권한 값**: `r(읽기)=4`, `w(쓰기)=2`, `x(실행/폴더진입)=1`
* **755 (`rwxr-xr-x`)**:
* **소유자(7 = 4+2+1)**: 읽기, 쓰기, 실행 모두 가능
* **그룹/기타(5 = 4+1)**: 읽기, 실행만 가능 (수정 불가)
* *용도*: 일반 실행 스크립트나 디렉토리의 기본 권한


* **644 (`rw-r--r--`)**:
* **소유자(6 = 4+2)**: 읽기, 쓰기 가능
* **그룹/기타(4 = 4)**: 읽기만 가능
* *용도*: 일반 소스 코드 및 문서 파일의 기본 권한



---

### c. 커스텀 이미지 제작 (Dockerfile 기반)

기존 베이스 이미지(예: Python, Ubuntu) 위에 **내가 만든 소스 코드와 필요한 라이브러리 설치 과정을 레시피(Dockerfile)로 작성**한 뒤, `docker build` 명령어를 통해 나만의 실행 환경 패키지(커스텀 이미지)를 생성하는 프로세스입니다.

* *핵심 흐름*: `Dockerfile 작성` $\rightarrow$ `docker build -t my-app .` 실행 $\rightarrow$ 나만의 커스텀 이미지 생성

---

### d. 포트 매핑 (Port Mapping)이 필요한 이유

Docker 컨테이너는 격리된 가상 네트워크 안에서 동작하므로, 내부에서 서버(예: 80번 포트)를 띄워도 **외부 호스트 컴퓨터에서는 직접 접근할 수 없습니다.**
따라서 외부 요청을 컨테이너 내부로 전달해 주는 **통로(포트 매핑)** 설정이 필수적입니다.

* *예시*: `docker run -p 8080:80 nginx`
* 내 컴퓨터의 `8080` 포트로 들어오는 요청을 컨테이너 내부의 `80` 포트로 연결(포워딩)해 줍니다.



---

### e. Docker 볼륨 (영속 데이터)

Docker 컨테이너는 삭제되면 내부에서 생성·수정된 데이터도 함께 사라지는 **휘발성 특징**을 가집니다.
컨테이너가 삭제되어도 데이터(DB, 파일 등)를 안 지우고 영구히 보존(영속성)하기 위해 컨테이너 내부 저장소를 호스트 컴퓨터의 저장소와 연결해 두는 기술이 Docker 볼륨입니다.

---

### f. Git vs GitHub 역할 차이

* **Git (로컬 버전관리 도구)**:
* 내 컴퓨터(로컬)에서 파일의 변경 이력을 기록하고 버전(Commit)을 관리하는 **소프트웨어**입니다.
* 인터넷이 연결되어 있지 않아도 동작합니다.


* **GitHub (원격 협업 플랫폼)**:
* Git으로 관리된 코드 이력을 인터넷(구름/클라우드)에 올리고 타인과 공유하며 협업할 수 있게 해주는 **웹 서비스**입니다.
* *비유*: Git이 내 컴퓨터 안의 '작업 노트'라면, GitHub는 노트를 공유하는 '온라인 도서관'입니다.
