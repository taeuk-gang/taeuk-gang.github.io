---
title: lit-element 학습 일지 (1)
toc: true
date: 2019-08-29 23:16:48
tags:
    - Javascript
categories:
    - Javascript
---

> 개인 학습 내용으로, 틀린 내용이 존재할 수 있습니다.(아마도 많이?)

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
> 음 기본 템플릿인 것 같다.

### 다음 단계

- [시작하기](https://lit-element.polymer-project.org/guide/start): 개발환경 구축과 컴포넌트 생성

- [템플릿](https://lit-element.polymer-project.org/guide/templates): lit-html을 이용한 템플릿 작성

- [속성](https://lit-element.polymer-project.org/guide/properties): properties와 attributes 관리
- [라이프사이클](https://lit-element.polymer-project.org/guide/lifecycle): lifecycle API를 이용한 LitElement 작동

-----------

