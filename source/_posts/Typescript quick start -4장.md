---
title: Typescript Quick Start ~ 4장 정리
toc: true
date: 2020-03-08 12:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---

## `package.json`

프론트엔드에서 가장 먼저 살펴보는 파일

1. `package.json`

2. 번들러: `webpack.config.js` or `vue.config.js` 

   - JS 로더: `tsconfig.json`, `babel.config.js` or `.babelrc`

   - CSS 로더: `postcss.config.js`

3. `index.html` - JS/CSS/Font 삽입 보기

4. 그외 Lint(코딩 컨벤션): `eslintrc.js`



### `package.json` 작성
> 개인적으로 중요하다고 생각되는 부분이나 몰랐던 부분만 정리
[이 사이트 (공식)](https://docs.npmjs.com/files/package.json)에서 자세히 설명되어있다. 

+[한글 사이트](https://programmingsummaries.tistory.com/385)

#### name

주의사항: name에는 대문자를 포함해서는 안된다.

#### version

버전 관리는 아래와 같은 [Rule](https://docs.npmjs.com/about-semantic-versioning)이 존재

> 모르고 있던 부분

![Semantic Versioning](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200308145529955.png)

#### scripts

> 개인적으로 가장 `package.json` 파일을 열었을 때, 가장 먼저 보는 항목

개발자가 설정해둔 커맨드 라인 명령어를 alias처럼 `npm run <key값>`으로 사용할 수 있다.

```json
// package.json

"scripts": {
    "test:make": "mkdir make-test-folder"
}
```

```bash
npm run test:make

# mkdir make-test-folder 명령어가 실행되고, 현재 경로에 make-test-folder 디렉토리가 생긴다.
```

##### 참고 링크

- [npm-scripts](https://docs.npmjs.com/misc/scripts)
- [npm-run-scripts](https://docs.npmjs.com/cli/run-script)



#### dependencies

실제 배포될 때 포함되는 패키지들

`npm install` or `npm install --save` 로 저장된 패키지들

#### devDependencies

개발용으로 필요한 패키지들 (lint, test, bundle etc)

`npm install -D` or `npm install --save-dev`로 저장된 패키지들





## npm 주요 명령어

npm 주요 명령어를 짧게 칠 수 있다. (기본 aliases)

자주 사용하는 명령어라서 짧게 치면 편하다.

```bash
# 이전
npm install <패키지명>
# 축약
npm i <패키지명>

# 여러 축약

## 글로벌 설치
npm i -g <패키지명>

## devDependency 설치
npm i -D <패키지명>

## 삭제
npm rm <패키지명>
```

[그 외 명령어들](https://docs.npmjs.com/cli-documentation/)



## `tsconfig.json` 설정

타입스크립트 컴파일 옵션 정의된 파일.

> 중요한 파일로, 개인 공부겸 다시 정리했습니다.
>
> [잘 정리된 블로그](https://vomvoru.github.io/blog/tsconfig-compiler-options-kr/)



-----

## 기타

### description, keywords

아래와 같이 npm 패키지를 찾았을 때 도움이 되는 항목

![npm search](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200308150016803.png)

아래와 같이 [홈페이지](https://www.npmjs.com/)를 통해 찾는 방법도 있다.

![image-20200308150135381](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200308150135381.png)