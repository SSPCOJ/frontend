# SSPC Frontend

SSPC의 프론트엔드입니다.

## 개발하기

node.js v16.20.2를 사용합니다.

1. `npm install`로 종속성을 설치합니다.
2. `npm run build:dll`로 dll을 빌드합니다.
3. `export TARGET="http://localhost"`등으로 `TARGET`환경변수를 백엔드 서버로 지정합니다.
4. `npm run dev`로 개발 서버를 엽니다.

## 빌드하기

1. `npm run build`를 이용해서 빌드합니다.
2. 결과물인 dist폴더를 deploy 폴더의 data/backend/dist로 붙여넣습니다. \
   [SSPC Deploy](https://github.com/SSPCOJ/deploy)에서 컨테이너를 한번 생성하면 data폴더가 생성됩니다.
3. deploy의 docker-compose.yml에서 oj-backend의 volumes에 `- ./data/backend/dist:/app/dist`를 추가합니다.
