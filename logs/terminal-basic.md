# 터미널 기본 명령 실습 로그

## 현재 위치 확인

```bash
pwd
```

실행 결과:

```text
/Users/haeunyoon2786/dev-workstation-mission
```

## 목록 확인

```bash
ls -la
```

실행 결과:

```text
total 8
drwxr-xr-x   7 haeunyoon2786  haeunyoon2786  224 Aug 14 15:03 .
drwxr-x---+ 18 haeunyoon2786  haeunyoon2786  576 Aug 14 15:02 ..
drwxr-xr-x   3 haeunyoon2786  haeunyoon2786   96 Aug 14 15:02 app
-rw-r--r--   1 haeunyoon2786  haeunyoon2786   85 Aug 14 15:03 Dockerfile
drwxr-xr-x   3 haeunyoon2786  haeunyoon2786   96 Aug 14 15:06 logs
-rw-r--r--   1 haeunyoon2786  haeunyoon2786    0 Aug 14 15:02 README.md
drwxr-xr-x   2 haeunyoon2786  haeunyoon2786   64 Aug 14 15:02 screenshots
```

## 디렉토리 생성, 이동, 파일 생성, 내용 확인, 복사, 이름 변경, 삭제

```bash
mkdir practice
cd practice
touch empty.txt
echo "hello terminal" > hello.txt
cat hello.txt
cp hello.txt copy.txt
mv copy.txt renamed.txt
ls -la
rm renamed.txt
cd ..
rm -r practice
```

실행 결과:

```text
hello terminal
total 16
drwxr-xr-x  5 haeunyoon2786  haeunyoon2786  160 Aug 14 15:06 .
drwxr-xr-x  8 haeunyoon2786  haeunyoon2786  256 Aug 14 15:06 ..
-rw-r--r--  1 haeunyoon2786  haeunyoon2786    0 Aug 14 15:06 empty.txt
-rw-r--r--  1 haeunyoon2786  haeunyoon2786   15 Aug 14 15:06 hello.txt
-rw-r--r--  1 haeunyoon2786  haeunyoon2786   15 Aug 14 15:06 renamed.txt
```
