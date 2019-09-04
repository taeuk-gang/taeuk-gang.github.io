---
title: lit-element 학습 일지 (1)
toc: true
date: 2019-08-29 23:16:48
tags:
    - Javascript
categories:
    - Javascript
---

> 개인이 학습한 내용으로, 틀린 내용이 존재할 수 있습니다.

## 소개

### 목차

- LitElement란 무엇인가?
- 다음 단계

### LitElement란 무엇인가?

LitElement는 어떤 프레임워크로든 모든 웹 페이지에서 작동하는 
빠르고 가벼운 웹 구성요소를 만들기 위한 간단한 기본 클래스다.

LitElement는 [lit-html](https://lit-html.polymer-project.org/)를 사용하여 shadow-dom으로 렌더링하는 방식이다. 그리고 API를 추가하여, 속성을 관리한다. 
속성은 기본적으로 관찰되며, 엘리멘트들은 이러한 속성들이 바뀔때마다 비동기적으로 업데이트된다.

LitElement를 이용하여 앱을 구성하려면, [PWA Starter kit](https://pwa-starter-kit.polymer-project.org/)을 확인해봐라.

> PWA Starter Kit이 뭐지?
>
> Progressive web application을 쉽게 만들 수 있는 샘플 프로젝트 (템플릿)
>
> [PWA Starter Kit](https://github.com/Polymer/pwa-starter-kit)은 PWA 앱 개발을 쉽게 시작할 수 있도록 제공되는 샘플 프로젝트다. 환경 구성과 페이지 구성(디자인, 반응형 레이아웃, 등)이 포함돼 있다. PWA Starter Kit을 활용하는 예제는 [Google I/O 2018](https://events.google.com/io/)의 "[PWA starter kit: build fast, scalable, modern apps with Web Components](https://www.youtube.com/watch?v=we3lLo-UFtk)" 세션을 참고한다.
>
> 출저: [2018년과 이후 JavaScript의 동향](https://d2.naver.com/helloworld/5644368)](https://d2.naver.com/helloworld/5644368)
>
> 으음... 프로젝트 구조 및 필요해보이는 모듈들을 분석이 필요해보임

### 다음 단계

- [시작하기](https://lit-element.polymer-project.org/guide/start): 개발환경 구축과 컴포넌트 생성

- [템플릿](https://lit-element.polymer-project.org/guide/templates): lit-html을 이용한 템플릿 작성

- [속성](https://lit-element.polymer-project.org/guide/properties): properties와 attributes 관리
- [라이프사이클](https://lit-element.polymer-project.org/guide/lifecycle): lifecycle API를 이용한 LitElement 작동

-----------

## 시작하기

### 목차

- 설치
- LitElement 컴포넌트 생성
  - LitElement TypeScript Decorators 사용
- 컴포넌트 불러오기
  - 너만의 컴포넌트 불러오기
  - 서드파티 LitElement 컴포넌트 불러오기

### 설치

#### npm과 Node.js 가 필요 (설치: [instructions on NodeJS.org](https://nodejs.org/en/))

#### npm을 이용한 Polymer CLI 설치

```bash
npm install -g polymer-cli
```

#### 로컬에서 서버 띄우기

```bash
polymer server
```

Polymer CLI 자세히 - [Polymer CLI documentation](https://polymer-library.polymer-project.org/3.0/docs/tools/polymer-cli) 

간단히 생성히 보기 - [sample LitElement project](https://github.com/PolymerLabs/start-lit-element).

