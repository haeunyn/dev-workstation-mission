

## Git 설정 로그

### 1. Git 버전 확인

Git이 정상적으로 설치되어 있는지 확인하기 위해 다음 명령어를 실행하였다.

```bash
git --version
```

실행 결과는 다음과 같다.

```text
git version 2.39.5 (Apple Git-154)
```

이를 통해 현재 사용 중인 Git 버전이 `2.39.5`이며, macOS에서 제공하는 Apple Git이 설치되어 있음을 확인하였다.

---

### 2. Git 사용자 정보 및 기본 브랜치 설정

Git 커밋에 사용할 사용자 이름과 이메일을 설정하고, 새 저장소를 만들 때 기본 브랜치 이름이 `main`이 되도록 설정하였다.

```bash
git config --global user.name "haeunyoon2786"
git config --global user.email "MASKED"
git config --global init.defaultBranch main
```

`user.name`은 커밋 작성자 이름을 의미하고, `user.email`은 커밋 작성자 이메일을 의미한다.  
`init.defaultBranch`는 `git init`으로 새 저장소를 생성할 때 기본 브랜치 이름을 지정하는 설정이다.

이메일 주소는 개인정보 보호를 위해 보고서에 마스킹 처리하였다.

---

### 3. Git 설정 확인

설정이 정상적으로 적용되었는지 확인하기 위해 Git 설정 목록을 조회하였다.

```bash
git config --list
```

실행 결과는 다음과 같다.

```text
init.defaultbranch=main
user.name=haeunyoon2786
user.email=MASKED
init.defaultbranch=main
```

조회 결과 `user.name`, `user.email`, `init.defaultbranch` 값이 정상적으로 설정된 것을 확인하였다.

`init.defaultbranch=main` 항목이 중복으로 표시되었지만, 이는 전역 설정과 다른 설정 범위에 같은 값이 함께 존재할 때 나타날 수 있다. 값이 모두 `main`으로 동일하므로 실습에는 문제가 없다.

---

### 4. 결과 정리

Git 버전 확인을 통해 Git이 정상적으로 설치되어 있음을 확인하였다.  
또한 Git 사용자 이름, 이메일, 기본 브랜치 설정을 완료하였고, 설정 조회를 통해 값이 정상적으로 적용된 것을 확인하였다.

이를 통해 이후 Git 저장소를 생성하고 커밋할 때 작성자 정보가 올바르게 기록되며, 기본 브랜치가 `main`으로 생성되도록 준비하였다.
