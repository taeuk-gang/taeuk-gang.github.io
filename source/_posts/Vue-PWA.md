---
title: Vue.js 환경 구축 기록일지
toc: true
date: 2019-12-28 18:23:12
tags:
  - Vue.js
categories:
  - Vue.js
---

## Vue.js 환경 구축 기록일지

### 목표

- [x] Vue.js 템플릿 사용
  - [ ] 기본 문법 정리
  - [ ] vue-cli 내용 정리
  - [ ] Vuex 라우팅 구축
  - [ ] nuxt.js 서버 사이드 렌더링 짜기
  - [ ] SCSS 환경 구축
- [ ] Kubernetes 위에서 작동하기
- [ ] Github + Jenkins 연동
  - [ ] Jenkins pipeline 내용 정리
- [ ] Progressive Web App 으로 작성
- [ ] Mocha + StoryBook + Cypress 테스트 모듈 사용
- [ ] ESLint Rule 짜기
- [ ] 스타일 가이드 정리
  - [ ] Vue.js 스타일 가이드 정리
  - [ ] TOAST FE 가이드 정리
  - [ ] Clean code 가이드 라인 정리



## Vue 템플릿 사용

### 설치

```bash
npm i -g @vue/cli

npm update -g @vue/cli
```

### 프로젝트 생성

```bash
vue create vue-pwa-template
```

#### Result: 생성 내 모듈 리스트

```json
"dependencies": {
    "core-js": "^3.4.3",
    "register-service-worker": "^1.6.2",
    "vue": "^2.6.10",
    "vue-router": "^3.1.3",
    "vuex": "^3.1.2"
  },
"devDependencies": {
    "@vue/cli-plugin-babel": "^4.1.0",
    "@vue/cli-plugin-e2e-cypress": "^4.1.0",
    "@vue/cli-plugin-eslint": "^4.1.0",
    "@vue/cli-plugin-pwa": "^4.1.0",
    "@vue/cli-plugin-router": "^4.1.0",
    "@vue/cli-plugin-unit-mocha": "^4.1.0",
    "@vue/cli-plugin-vuex": "^4.1.0",
    "@vue/cli-service": "^4.1.0",
    "@vue/eslint-config-airbnb": "^4.0.0",
    "@vue/test-utils": "1.0.0-beta.29",
    "babel-eslint": "^10.0.3",
    "chai": "^4.1.2",
    "eslint": "^5.16.0",
    "eslint-plugin-vue": "^5.0.0",
    "node-sass": "^4.12.0",
    "sass-loader": "^8.0.0",
    "vue-template-compiler": "^2.6.10"
},
```



## Github + Docker 연결

### 설치

https://hub.docker.com/ 공식 홈페이지 참고

#### DockerFile 작성

```dockerfile
# Build
FROM node:lts-alpine as build-stage
WORKDIR /vue-pwa-project
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production
FROM nginx:stable-alpine as production-stage

RUN rm /etc/nginx/conf.d/default.conf
COPY ./docker/nginx.conf /etc/nginx/conf.d/nginx.conf

COPY --from=build-stage ./vue-pwa-project/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### *별도 nginx.conf 

```nginx
server {
    listen 80;

    location / {
        alias /usr/share/nginx/html/;
        try_files $uri $uri/ /index.html;
    }
}
```

#### docker build

```bash
docker build -t kangtaeuk/vue-template .
```

```bash
docker run -it -p 8080:80 --rm kangtaeuk/vue-template
```



## Jenkins 환경 구축

### 도커로 표준 젠킨스 서버 생성

목표: [jenkins/jenkins 이미지](https://hub.docker.com/r/jenkins/jenkins)를 이용하여 컨테이너 생성 및 실행

#### 1. 젠킨스 도커 이미지 가져오기

```bash
docker pull jenkins/jenkins
# or
docker pull jenkins/jenkins:<tag>
```

#### 2. 도커 볼륨 생성

```bash
docker volume create jenkins_home_classic
```

이후, 볼륨이 생성됬는지 확인하기

```bash
docker volume ls

docker volume inspect jenkins_home_classic
```

#### 3. 도커 실행

```bash
docker run -d --name jenkins_classic \
-p 8081:8080 -p 50001:50000 \
-v jenkins_home_classic:/var/jenkins_home \
jenkins/jenkins
```

#### 4. 브라우저 접속

```
http://<docker IP>:8081
```

블루오션이 없는 최신 버전 표준 젠킨스가 실행이 된다.

>  * 번외: VSCODE에서 Docker 플러그인을 사용하면 더 편하게 진행이 가능하다.

### 

### 기존 젠킨스에 블루오션 설정하기

#### 1. `Jenkins 관리 > 플러그인 관리 > 설치 가능`에서 블루오션 설치

![1577658451063](./Vue-PWA.assets/1577658451063.png)

`재시작 없이 설치하기` 클릭

#### Tip. 젠킨스 재시작법

`http://<젠킨스 URL:PORT>/restart`를 들어가면 재시작된다.

#### Result

![1577658800703](./Vue-PWA.assets/1577658800703.png)
