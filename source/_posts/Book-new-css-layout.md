---
title: 새로운 CSS 레이아웃 도서 요약
toc: true
date: 2019-07-23 19:19:49
tags:
    - Book
categories:
    - Book
---

# 새로운 CSS 레이아웃 요약

## 이전 레이아웃

### 플롯의 문제

플롯은 모든 요소의 높이가 똑같다면 괜찮은 방식임
그렇지 않다면, 아래와 같이 레이아웃이 망가질 수 있음

<p class="codepen" data-height="776" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="58220504071e1845d78a26906252de50" data-editable="true" style="height: 776px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="플롯 - 망가진 레이아웃">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/58220504071e1845d78a26906252de50/">
  플롯 - 망가진 레이아웃</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
### ~~플롯 문제 해결~~

----------

## ~~현재~~

### ~~컴포넌트 우선 디자인~~

---------------

## 새로운 레이아웃

### `column-count`, `column-width`

<p class="codepen" data-height="429" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="36f019a92a98f5eb9ff66788ba6b9cb4" data-editable="true" style="height: 429px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="column-count">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/36f019a92a98f5eb9ff66788ba6b9cb4/">
  column-count</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
### `flex-wrap`

마지막 엘리먼트를 지워보고, 어떻게 영향을 끼치는지 확인하자

그리고 `flex: 0 0 auto;`로 변경해보자.

`flex: 0 0 auto;` = `flex-grow`, `flex-shrink`, `flex-basis` 순서로 배치됨

<p class="codepen" data-height="700" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="0d15ea20e694774bcaf4b176c9300133" data-editable="true" style="height: 700px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="flex-wrap">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/0d15ea20e694774bcaf4b176c9300133/">
  flex-wrap</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
### 그리드 레이아웃

#### * 내가 몰랐던 사실

> `grid-gap`, `grid-column-gap`, `grid-row-gap`을 `grid`를 빼고 사용이 가능하게 바뀌었다.
>
> 이제는 `gap`, `column-gap`, `row-gap`으로 사용 가능하다.
>
> 하지만, 아직 브라우저 지원률은 `grid`를 붙인 것이 높은 듯 하다.
>
> IE는 확인해봐야 알 듯... (내가 알기로 폴리필을 해도 `grid-gap` 지원 안했던 것 같은데...)

> 그리드 레이아웃 빈칸은 `.`으로 표시
>
> ```css
> grid-template-areas:
> 	"a a b"
> 	". d d"
> 	"c e e";
> ```



-------------

## 배치와 정렬

`grid-align-slef`

<p class="codepen" data-height="506" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="5b865d17d937ad09b7792d3b928d2a1e" data-editable="true" style="height: 506px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="grid-align-self">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/5b865d17d937ad09b7792d3b928d2a1e/">
  grid-align-self</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
### `align-content: space-between;` 
### `justify-content: space-between;`

<p class="codepen" data-height="471" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="24c1872bf9b4ef548373fd2dc1994332" data-editable="true" style="height: 471px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="grid-align-content">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/24c1872bf9b4ef548373fd2dc1994332/">
  grid-align-content</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### `justify-items` ,`justify-self`

<p class="codepen" data-height="492" data-theme-id="0" data-default-tab="result" data-user="taeuk_kang" data-slug-hash="94fb288a635c102666cafcd40368d806" data-editable="true" style="height: 492px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="grid-justify">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/94fb288a635c102666cafcd40368d806/">
  grid-justify</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

----------------

## ★반응형 디자인

글 작성중

-----------

## ★소스 순서와 표현 순서

-------------

## 준비하는 미래

------------

## 앞으로의 여정

## 참고파일
{% raw %}
<a href ='/assets/new-css-layout-code-master.zip' download>예제 코드 Download</a>
<br />
<a target='_blank' href ='/assets/new-css-layout-code-master/index.html'>예제 코드 Reference</a>
{% endraw %}