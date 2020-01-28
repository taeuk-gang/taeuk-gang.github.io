---
title: Vue Init Template 구조 파악
toc: true
date: 2020-01-28 15:26:47
tags:
	- Vue.js
categories:
	- Vue.js
---

## 구조

```
📦vue-pwa-tree
 ┣ 📂docker # Ngnix 환경에서 돌려질 도커 이미지 환경설정
 ┣ 📂public # '/' 경로
 ┃ ┣ 📂img
 ┃ ┃ ┗ 📂icons # PWA Icon 폴더
 ┃ ┣ 📜index.html # 시작 파일
 ┣ 📂src # 작업 폴더
 ┃ ┣ 📂assets # 리소스
 ┃ ┣ 📂components # 공용 컴포넌트
 ┃ ┣ 📂router # 라우터
 ┃ ┣ 📂store # 상태관리
 ┃ ┣ 📂views # 페이지
 ┃ ┣ 📜App.vue # 시작 파일
 ┃ ┣ 📜main.ts # 메인
 ┃ ┣ 📜registerServiceWorker.ts # 서비스워커
 ┃ ┣ 📜shims-tsx.d.ts # ?
 ┃ ┗ 📜shims-vue.d.ts # ?
 ┣ 📂tests # 테스트 코드
 ┃ ┣ 📂e2e # e2e
 ┃ ┗ 📂unit # 유닛 테스트
 ┣ 📜.dockerignore
 ┣ 📜.gitignore
 ┣ 📜babel.config.js
 ┣ 📜cypress.json
 ┣ 📜devspace.yaml
 ┣ 📜Dockerfile
 ┣ 📜Jenkinsfile
 ┣ 📜package-lock.json
 ┣ 📜package.json
 ┣ 📜README.md
 ┗ 📜tsconfig.json # 이건 좀 봐두자
```

## 흐름

![image](https://user-images.githubusercontent.com/26294469/73253730-6f1fa700-4200-11ea-9a45-c0f02a28aad2.png)

