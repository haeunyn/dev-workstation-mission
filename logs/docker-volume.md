# Docker 볼륨 영속성 검증 로그

## 볼륨 생성 명령
docker volume create workstation-volume
docker volume ls

## 볼륨 생성 결과
workstation-volume
DRIVER    VOLUME NAME
local     workstation-volume

## 첫 번째 컨테이너에서 데이터 생성
docker run --name volume-test -v workstation-volume:/data ubuntu bash -lc 'echo persistent-data > /data/message.txt && cat /data/message.txt'

## 실행 결과
persistent-data

## 첫 번째 컨테이너 삭제
docker rm volume-test

## 실행 결과
volume-test

## 두 번째 컨테이너에서 데이터 유지 확인
docker run --name volume-test-2 -v workstation-volume:/data ubuntu bash -lc 'cat /data/message.txt'

## 실행 결과
persistent-data

## 결과 정리
첫 번째 컨테이너를 삭제했지만 Docker 볼륨은 삭제되지 않았다.
같은 볼륨을 두 번째 컨테이너에 연결하자 이전에 생성한 데이터가 유지되어 있었다.
이를 통해 컨테이너 생명주기와 데이터 저장소 생명주기가 분리된다는 것을 확인했다.
