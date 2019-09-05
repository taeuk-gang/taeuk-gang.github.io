---
title: lit-element 학습 일지
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

> `sample Litelement project` 예제는 firebase를 이용한 템플릿이다.
>
> 프로젝트에 서비스워커도 사용하고, polyfill도 넣고, 구조를 약간 살펴볼 필요성이 있어보임
>
> 살펴본 결과, polymer에 의존된 부분이 많아 템플릿으로 쓰기 부적절하다 생각됬다.
>
> polymer.json을 webpack.config.json으로 바꾸는 등 표준적인 스펙으로 변경해야겠다.

## LitElement 컴포넌트 생성

```bash
npm install lit-element
```

1. LitElement 모듈 불러오기(`import`)
2. LitElement를 상속받는 `class` 생성
3. `render` 부분 선언
4. `define`을 통한 커스텀 엘리먼트화

### 예시

```js
// my-element.js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {  
  render(){
    return html`
      <p>A paragraph</p>
    `;
  }
}

customElements.define('my-element', MyElement);
```

여기까지는 lit-html과 다를게 없음

## lit-element 불러오기

```html
<head>
  <script type="module" src="/path/to/my-element.js"></script>
</head>
<body>
	<my-element></my-element>
</body>
```

### 다른 모듈 불러오기

```js
import './my-element.js';

class MyOtherElement extends LitElement{
  render(){
    return html`
      <my-element></my-element>
    `;
  }
}
customElements.define('my-other-element', MyOtherElement);
```

### 서드파티 LitElement 불러오기

> 서드파티가 뭐지?
>
> **서드 파티 개발자**(3rd party developer)또는 써드 파티는 일반적으로 하드웨어 생산자와 소프트웨어 개발자의 관계를 나타내는 용어로 사용된다.
>
> 하드웨어 생산자는 퍼스트 파티(first party)로, 소프트웨어 개발자는 서드 파티(third party)로 불리기도 한다. 
>
> 하드웨어 생산자인 모기업과 자사간의 관계 또는 하청관계등 전혀 관련없는 소프트웨어 개발자를 써드 파티라고 부르고 제품의 사용자를 세컨드 파티(2nd party) 그리고 하드웨어 생산자인 모기업과 자사간의 관계 또는 하청관계등 여타의 관계하에 소프트웨어를 개발하는 업체를 퍼스트파티라고 표현하는등 업체별 분야별로 약간씩 서로 다른 사례나 관례를 가지고 있다.
>
> 출저: 위키피디아

#### 다운받기

```bash
cd my-project-folder

# 서드파티 모듈 불러오기
npm install package-name
```

#### HTML에서 사용하기

```html
<head>
  <script type="module" src="node_modules/package-name/existing-element.js"></script>
</head>
<body>
  <existing-element></existing-element>
</body>
```

#### Javascript에서 사용하기

```js
import 'package-name/existing-element.js';

class MyElement extends LitElement{
  render(){
    return html`
      <existing-element></existing-element>
    `;
  }
}
customElements.define('my-element', MyElement);
```

--------------

## 템플릿

### 목차

- 템플릿 정의 및 렌더링
  - 템플릿의 성능 설계
  - `properties, loop, conditionals` 문법 사용
  - child-element 속성 바인딩
  - slot-element 사용하기

- 다른 템플릿과 함께 사용하기
- shadow-dom 사용하기
- 템플릿 치트시트
- 더 읽을거리

### 템플릿 정의 및 렌더링

```js
class MyElement extends LitElement {
  render() {
    return html`<p>template content</p>`;
  }
}
```

html``(백틱)을 이용한 정의

### 템플릿 성능 설계

#### `render` 함수 설명

- element의 state가 바뀌지 않음
- 사이드 이펙트가 없다 (?)
- 오직 element properties에만 의존한다
- 같은 property value면 같은 결과를 가진다

##### 비효율적인 DOM 렌더링 예시

```js
// class 내부 코드
constructor() {
  super();
  this.addEventListener('stuff-loaded', (e) => {
    this.shadowRoot.getElementById('message').innerHTML=e.detail;
  });
  this.loadStuff();
}
render() {
  return html`
    <p id="message">Loading</p>
  `;
}
```

##### 개선된 효율적인 코드

```js
constructor() {
  super();
  this.message = 'Loading';
  this.addEventListener('stuff-loaded', (e) => { this.message = e.detail } );
  this.loadStuff();
}
render() {
  return html`
    <p>${this.message}</p>
  `;
}
```

위 두 차이를 보면 알 수 있는 것은, DOM을 직접 조작하지마라! 이다.

#### `Properties`, `loop`, `conditionals` 사용

##### properties

```js
static get properties() {
  return { myProp: String }; // 선언방식 = 속성: 타입
}
...
render() { 
  return html`<p>${this.myProp}</p>`; 
}
```

##### loop

```js
html`<ul>
  ${this.myArray.map(i => html`<li>${i}</li>`)}
</ul>`;
```

map을 이용한 반복 정의

##### conditionals

```js
html`
  ${this.myBool?
    html`<p>Render some HTML if myBool is true</p>`:
    html`<p>Render some other HTML if myBool is false</p>`}
`;
```

React와 비슷하게 사용되네

##### 전체 code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      myString: { type: String },
      myArray: { type: Array },
      myBool: { type: Boolean }
    };
  }
  constructor() {
    super();
    this.myString = 'Hello World';
    this.myArray = ['an','array','of','test','data'];
    this.myBool = true;
  }
  render() {
    return html`
      <p>${this.myString}</p>
      <ul>
        ${this.myArray.map(i => html`<li>${i}</li>`)}
      </ul>
      ${this.myBool?
        html`<p>Render some HTML if myBool is true</p>`:
        html`<p>Render some other HTML if myBool is false</p>`}
    `;
  }
}

customElements.define('my-element', MyElement);
```

#### 자식 엘리멘트 바인딩

html``을 이용할 때, textContent, attributes, boolean, properties, event handler 등을 간편하게 받을 수 있는 문법이 있다.

- textContent: `<p>${...}</p>`
- attribute: `<p id="${...}"></p>`
- boolean attribute: `?checked="${...}"`
- property: `.value="${...}"`
- event handler: `@event="${...}"`

요소의 속성이 포함될 수 있고, LitElement는 속성 변경을 관찰하고 이에 자동으로 템플릿을 업데이트를 한다.

데이터바인딩은 항상 단방향

##### textContent 바인딩

```js
html`<div>${this.prop1}</div>`
```

##### attribute 바인딩

```js
html`<div id="${this.prop2}"></div>`
```

속성 값은 항상 문자열 또는 문자열로 반환될 수 있는 값이여야 한다.

##### boolean attribute 바인딩

```js
html`<input type="checkbox" ?checked="${this.prop3}>i like pie</input>"`
```

`true`면 attribute로 추가되고, `false`면 사라진다.

##### property 바인딩

```js
html`<input type="checkbox" .value="${this.prop4}" />`
```

##### event handler 바인딩

```js
html`<button @click="${this.clickHandler}">pie?</button>`
```

자동으로 저 엘리멘트를 `this`로 설정한다.

##### 전체 code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      prop1: String,
      prop2: String,
      prop3: Boolean,
      prop4: String
    };
  }
  constructor() {
    super();
    this.prop1 = 'text binding';
    this.prop2 = 'mydiv';
    this.prop3 = true;
    this.prop4 = 'pie';
  }
  render() {
    return html`
      <!-- text binding -->
      <div>${this.prop1}</div>

      <!-- attribute binding -->
      <div id="${this.prop2}">attribute binding</div>

      <!-- boolean attribute binding -->
      <div>
        boolean attribute binding
        <input type="checkbox" ?checked="${this.prop3}"/>
      </div>

      <!-- property binding -->
      <div>
        property binding
        <input type="checkbox" .value="${this.prop4}"/>
      </div>

      <!-- event handler binding -->
      <div>event handler binding
        <button @click="${this.clickHandler}">click</button>
      </div>
    `;
  }
  clickHandler(e) {
    console.log(e.target);
  }
}

customElements.define('my-element', MyElement);
```

#### `slot-element`를 이용한 `light Dom` 렌더링

##### shadow Dom vs light Dom

shadow Dom을 소개하기 때문에 그것과 비교하기 위해 light Dom이라는 별도 용어를 사용

기본적으로 모든 부분을 렌더링하진 않는다. (부분 렌더링)

```html
<my-element>
	<p>I won't render</p>
</my-element>
```

##### slot-element 사용

```js
render() {
    return html`
		<div>
			<slot></slot>
		</div>
	`;
}
```

Light Dom의 자식 엘리멘트는 이제 slot부분을 렌더링할 수 있다.

```html
<my-element>
	<p>Render me</p>
    <p>Me too</p>
    <p>Me three</p>
</my-element>
```

간단히 요약하면, 자바스크립트에서 `<slot></slot>`이라고 표시를 하고, 
HTML에서 여러 자식 엘리멘트를 삽입할 수 있구나

##### slot에 이름 붙이기

```js
render() {
    return html`
		<div>
			<slot name="one"></slot>
		</div>
	`;
}
```

index.html

```html
<my-element>
    <p slot="one">slot one</p>
</my-element>
```

자바스크립트에서 name으로 선언된 슬롯명과 HTML에서 slot attribute 선언된 문자열이 같은 것만 허용

ex. `<slot name="one"></slot>`은 `<p slot="one"></p>`만 허용, 거꾸로도 마찬가지임

##### 전체 code example

my-element.js

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  render(){
    return html`
      <div>
        <slot name="one"></slot>
        <slot name="two"></slot>
      </div>
    `;
  }
}
customElements.define('my-element', MyElement);
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <script src="/node_modules/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js"></script>
  <script src="/node_modules/@webcomponents/webcomponentsjs/webcomponents-bundle.js"></script>
  
  <script type="module" src="./my-element.js"></script>
  <title>lit-element code sample</title>
</head>
<body>
    <!-- Assign light DOM child to a specific slot -->

    <my-element>
      <p slot="two">Include me in slot "two".</p>
    </my-element>

    <!-- 
      Named slots only accept light DOM children with a matching `slot` attribute. 
      
      Light DOM children with a `slot` attribute can only go into a slot with a matching name. 
    -->

    <my-element>
      <p slot="one">Include me in slot "one".</p>
      <p slot="nope">This one will not render at all.</p>
      <p>No default slot, so this one won't render either.</p>
    </my-element>
</body>
</html>
```

##### `name`을 사용하라! `id`는 아무 영향이 없다.

my-element.js

```js
render(){
  return html`
    <div>
      <slot id="one"></slot>
    </div>
  `;
}
```

index.html

```html
<my-element>
  <p slot="one">nope.</p>
  <p>ohai..</p>
</my-element>
```

아무런 영향이 없음

### 다른 엘리멘트 템플릿으로 템플릿 구성하기

예를 들면, `<header>`, `<article>`, `<footer>` 을 이용해 템플릿 구성하는 방법

```js
class MyPage extends LitElement {
    render() {
        return html`
		${this.headerTemplate}
		${this.articleTemplate}
		${this.footerTemplate}
		`
    }
    
    get headerTemplate() {
        return html`<header>Header</header>`
    }
    
    get articleTemplate() {
        return html`<article>Article</article>`
    }
    
    get footerTemplate() {
        return html `<footer>Footer</footer>`
    }
}
```

import를 이용한 구성방법

my-article.js

```js
import { LitElement, html } from 'lit-element';

class MyArticle extends LitElement {
  render() {
    return html`
      <article>article</article>
    `;
  }
}
customElements.define('my-article', MyArticle);
```

my-page.js

```js
import './my-header.js';
import './my-article.js';
import './my-footer.js';

class MyPage extends LitElement {
  render() {
    return html`
      <my-header></my-header>
      <my-article></my-article>
      <my-footer></my-footer>
    `;
  }
}
```

`lit-html`과 공통사항이 많음

### Specify the render root

기본적으로, LitElement는 open 상태인 `shadowRoot`를 생성하고 그 아래에 Dom 구조를 형성한다.

```html
<!-- 형태 -->
<my-element>
  #shadow-root
    <p>child 1</p>
    <p>child 2</p>
```

```js
class LightDom extends LitElement {
    render() {;
        return html`
			<p>light DOM</p>
		`;
    }
    
    createRenderRoot() {
        return this;
    }
}
```

### cheat sheet

#### Render

```js
render() {
    return html`
		<p>template</p>
	`
}
```

#### Properties, loops, conditionals

```js
// Property
html`<p>${this.prop}</p>`

// Loop
html`${this.array.map(i => html`<li>${i}</li>`)}`

// Conditional
html`${this.bool ? html`<p>foo</p>` : html`<p>bar</p>`}`
```

#### Data binding

```js
// Attribute
html`<p id="${...}"`

// Boolean attribute
html`<input type="checkbox" ?checked="${...}">`

// Property
html`<input .value="${...}">`

// Event handler
html`<button @click="${this.do}"></button>`
```

#### Composition

```js
// From multiple templates on same class
render() {
    return html`
		${this.headerTemplate}
		<article>Article</article>
	`
    
    get headerTemplate() {
        return html`<header>Header</header>`
    }
}
```

```js
// By importing elements
import './my-header.js'

class MyPage extends LitElement {
    render() {
        return html`
			<my-header></my-header>
			<article>Article</article>
		`
    }
}
```

#### slots

```js
render() {
    return html`
		<slot name="thing"></slot>
	`
}
```

```html
<my-element>
    <p slot="thing">stuff</p>
</my-element>
```

### Futher reading

LitElement는 lit-html의 `html`, `render` 기능을 사용한다. 

- [on the lit-html homepage](https://lit-html.polymer-project.org/)
- [in the Template Reference](https://lit-html.polymer-project.org/guide/template-reference)

