# 파일/디렉토리 권한 실습 로그

## 파일 권한 변경

```bash
touch permission-file.txt
ls -l permission-file.txt
chmod 755 permission-file.txt
ls -l permission-file.txt
```

실행 결과:

```text
-rw-r--r--  1 haeunyoon2786  haeunyoon2786  0 Aug 14 15:07 permission-file.txt
-rwxr-xr-x  1 haeunyoon2786  haeunyoon2786  0 Aug 14 15:07 permission-file.txt
```

## 디렉토리 권한 변경

```bash
mkdir permission-dir
ls -ld permission-dir
chmod 700 permission-dir
ls -ld permission-dir
```

실행 결과:

```text
drwxr-xr-x  2 haeunyoon2786  haeunyoon2786  64 Aug 14 15:07 permission-dir
drwx------  2 haeunyoon2786  haeunyoon2786  64 Aug 14 15:07 permission-dir
```

## 권한 설명

파일 권한에서 r은 읽기, w는 쓰기, x는 실행 권한을 의미한다.

755는 다음과 같이 해석된다.

- 소유자: 7 = r + w + x
- 그룹: 5 = r + x
- 기타 사용자: 5 = r + x

644는 다음과 같이 해석된다.

- 소유자: 6 = r + w
- 그룹: 4 = r
- 기타 사용자: 4 = r
