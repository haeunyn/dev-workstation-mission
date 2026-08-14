# 바인드 마운트 검증 로그

## 바인드 마운트 컨테이너 실행 명령
docker run -d --name workstation-web-bind -p 18081:80 -v 현재폴더/app:/usr/share/nginx/html:ro nginx:alpine

## 바인드 마운트 컨테이너 실행 결과
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

## 변경 전 접속 확인
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
curl: (52) Empty reply from server

## 호스트 파일 수정 후 접속 확인
호스트의 app/index.html 파일을 수정한 뒤 다시 접속했다.
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0100   213  100   213    0     0  86444      0 --:--:-- --:--:-- --:--:--  104k
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Updated</title>
</head>
<body>
  <h1>Bind Mount Updated</h1>
  <p>This change was made on the host machine.</p>
</body>
</html>

## 결과 정리
바인드 마운트를 사용하니 호스트의 app/index.html 변경 내용이 컨테이너 내부 웹 서버에 즉시 반영되었다.
이미지를 다시 빌드하지 않아도 변경 결과를 확인할 수 있었다.
