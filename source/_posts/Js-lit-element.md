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

-------------

## 스타일

### 목차

- 컴포넌트 개발을 위한 스타일 옵션
  - 정의 위치
  - static styles property
  - style element
  - external stylesheet
  - host element와 shadow Dom에서의 css 작성

- 컴포넌트 사용자를 위한 스타일 옵션 (`import`)
- 테마 작업
  - CSS 상속과 shadow Dom
  - Custom CSS properties
  - example theme

> 만약, Shady Css polyfill을 사용한다면, 일부 제한사항이 발생함
> 자세한 사항은 [Shady CSS](https://github.com/webcomponents/polyfills/tree/master/packages/shadycss)를 참조

### Styling options for component developers

#### 정의하는 방법 3가지

- ★추천★: css``를 이용한 선언 (static styles property)
- `render` 안에 `<style>` 태그를 이용한 방법

- 외부 스타일 시트를 이용한 방법 `<link rel="stylesheet" href="..." />`

#### css``이용: Define styles in a static styles property

추천되는 이유는 정적 스타일이 최적화 성능이 잘되있어서 인 듯

> 상세사항: We recommend using static styles for optimal performance. LitElement uses [Constructable Stylesheets](https://wicg.github.io/construct-stylesheets/) in browsers that support it, with a fallback for browsers that don’t. Constructable Stylesheets allow the browser to parse styles exactly once and reuse the resulting Stylesheet object for maximum efficiency.

컴포넌트 개발자는 shadow Dom 안에서 CSS 스타일을 정의한다.

import로 사용하는 이용자는 shadow Dom 밖에서 host 엘리멘트에 CSS 상속을 통해 CSS를 정의한다.

##### 사용법

1. `css` helper 함수를 import

```js
import {LitElement, css} from 'lit-element';
```

2. `styels`를 LitElement가 상속된 클래스에서 `static`을 이용해 정의함

```js
class myElement extends LitElement {
    static get styles() {
        return css`
		:host {
			display: block;
		}
		`
    }
}
```

또는, 배열을 이용해 정의도 가능

```js
class MyElement extends LitElement {
    static get styles() {
        return [ css`:host { display: block; }`, ...]
    }
}
```

##### 변수 사용

악의적인 코드 삽입을 방지하기위해, css``를 사용하여 삽입

```js
static get styles() {
    const mainColor = css`red`
    
    return css`
	:host {
		color: ${mainColor};
	}
	`
}
```

그래도 정 문자열을 사용하고 싶다면 아래와 같이 사용

```js
import { LitElement, css, unsafeCSS } from 'lit-element'

class MyElement extends LitElement {
    static get styles() {
        const mainColor = `red`
        
        return css`
		:host {
			color: ${unsafeCSS(mainColor)};
		}
		`
    }
}
```

다른 예제

```js
import { LitElement, css, unsafeCSS } from 'lit-element';

class MyElement extends LitElement {
  static get styles() {
    const mainWidth = 800;
    const padding = 20;   
    
    return css`
      :host { 
        width: ${unsafeCSS(mainWidth + padding)}px;
      }
    `;
  } 
}
```

> **Only use the unsafeCSS tag with trusted input.** To prevent LitElement-based components from evaluating potentially malicious code, the `css` tag only accepts literal strings. `unsafeCSS` circumvents this safeguard.

#### `<style>` 태그를 이용한 방법

> **Expressions inside a `<style>` element won’t update per instance in ShadyCSS**. Due to limitations of the ShadyCSS polyfill, you can’t use element properties in CSS rules as the expressions won’t be evaluated.
>
> 스타일 태그를 이용한 방법은 ShadyCSS에서 인스턴스당 업데이트를 하지 않는다는 의미가 잘 와닿지가 않네.
> ShadyCSS polyfill에 제한이 있을 수 있다는 뜻인가

##### 사용법

```js
import {LitElement, property} from 'lit-element';

class MyElement extends LitElement {
  @property() mainColor = 'blue';
  render() {
    return html`
      <style>
        :host {
          color: ${this.mainColor};
        }
      </style>
    `;
  }
}
```

#### 외부 스타일시트 사용하기

> We strongly recommend static styles, CSS custom properties, or [lit-html’s `classMap` or `styleMap` directives](https://lit-element.polymer-project.org/guide/TODO) if you’re styling non-host shadow root contents.

##### 사용법

```js
import {LitElement} from 'lit-element';

class MyElement extends LitElement {
  render() {
    return html`
      <link rel="stylesheet" href="./styles.css">
    `;
  }
}
```

이것은 SASS/LESS를 부를 때 좋은 방법이 될 수 있음
그러나 몇가지 주의사항이 존재

- 이 사용법은 FOUC를 발생시킴 (잠깐 CSS가 변경되는 모습이 보여지는 것)
- 재사용 컴포넌트를 만들기에는 부적합할 수 있음

### Write CSS styles for a host element and its shadow DOM

host(밖)와 shadow Dom(안)에서 CSS 스타일링 방식이 다름

- host element에서 스타일링 방법
- shadow Dom 안에서 스타일링 방법
- slot-element의 스타일링 방법

#### Write CSS styles for a host element

##### 사용법

`:host` selects the host element of the shadow root

```css
:host {
    display: block;
    color: blue;
}
```

`:host(...)` selects the host element, (...)안에 셀렉터를 사용할 수 있음

```css
:host(.important) {
    color: red;
    font-weight: bold;
}
```

#### Write CSS styles for elements in shadow DOM

일반적인 css 사용과 같음
그러나, shadow Dom 밖으로 CSS 전파가 되지 않기 때문에 범용성 있는 네이밍이나 *태그를 쉽게 사용 가능

```CSS
* {
  color: black;
}

h1 {
  font-size: 4rem;
}

#main {
  padding: 16px;
}

.important {
  color: red;
}
```

#### Write CSS styles for slotted children

##### 사용법

- `::slotted(*)` matches all slotted elements.
- `::slotted(p)` matches slotted paragraphs.
- `p ::slotted(*)` matches slotted elements in a paragraph element.

##### 전체 code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  render() {
    return html`
      <style>
        :host([hidden]) { display: none; }
        :host { display: block; }
        ::slotted(*) { font-family: Roboto; }
        ::slotted(span) { color: blue; }
        div ::slotted(*) { color: red; }
      </style>
      <slot></slot>
      <div><slot name="hi"></slot></div>
    `;
  }
}
customElements.define('my-element', MyElement);
```

### 컴포넌트 사용자를 위한 스타일 옵션 (`import`)

import로 만들어진 컴포넌트를 불러와 스타일링 할 수 있다

```html
<style>
  my-element { 
    font-family: Roboto;
    font-size: 20;
    color: blue;
  }
</style>
...
<my-element></my-element>
```

> 이렇게 엘리먼트로 선언된 CSS가 `:host`보다 우선순위가 높다.
>
> Styles set for a host element from outside its shadow DOM will override styles set with the `:host` or `:host()` pseudo-class selectors inside shadow DOM. See Inheritance.
>
> 대충 덮어쓰기를 한다는 뜻인가



## Theming

CSS 상속을 어떻게 받고 커스텀할지 설명하는 부분

### Custom CSS Properties

index.html

```html
<style>
  html {
    --themeColor1: rgb(67, 156, 144);
  }
  my-element {
    --myBackground: var(--themeColor1);
  }
</style>
```

my-element.js

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  render() {
    return html`
      <style>
        :host([hidden]) { display: none; }
        :host { display: block;
          background-color: var(--myBackground, yellow);
          color: var(--myColor, black);
          padding: var(--myPadding, 8px);
        }
      </style>
      <p>Hello world</p>
    `;
  }
}
customElements.define('my-element', MyElement);
```

### 전체 code example

index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="/node_modules/@webcomponents/webcomponentsjs/custom-elements-es5-adapter.js"></script>
    <script src="/node_modules/@webcomponents/webcomponentsjs/webcomponents-bundle.js"></script>
    <title>lit-element code sample</title>
    <style>
      body {
        --theme-primary: green;
        --theme-secondary: aliceblue;
        --theme-warning: red;
        --theme-font-family: Roboto;
      }

      my-element { 
        --my-element-text-color: var(--theme-primary); 
        --my-element-background-color: var(--theme-secondary); 
        --my-element-font-family: var(--theme-font-family);
      } 

      my-element.warning {
        --my-element-text-color: var(--theme-warning); 
      }
    </style>
  </head>
  <body>
    <my-element></my-element>
    <my-element class="warning"></my-element>
  </body>
</html>
```

my-element.js

```js
import { LitElement, html, css } from 'lit-element';

class MyElement extends LitElement {
  static styles = css`
    :host { 
      display: block;
      color: var(--my-element-text-color); 
      background: var(--my-element-background-color);  
      font-family: var(--my-element-font-family);
    }
    :host([hidden]) {
      display: none;
    }
  `;
  render() {
    return html`
      <div>Hello from my-element</div>
    `;
  }
}
customElements.define('my-element', MyElement);
```

----------

## Properties

### 목차

- Overview
  - [Property options](https://lit-element.polymer-project.org/guide/properties#property-options)
- Declare properties
  - [Declare properties in a static properties getter](https://lit-element.polymer-project.org/guide/properties#declare-properties-in-a-static-properties-getter)
  - [Declare properties with decorators](https://lit-element.polymer-project.org/guide/properties#declare-with-decorators)
- Initialize property values
  - [Initialize property values in the element constructor](https://lit-element.polymer-project.org/guide/properties#initialize-property-values-in-the-element-constructor)
  - [Initialize property values when using TypeScript decorators](https://lit-element.polymer-project.org/guide/properties#initialize-property-values-when-using-typescript-decorators)
  - [Initialize property values from attributes in markup](https://lit-element.polymer-project.org/guide/properties#initialize-property-values-from-attributes-in-markup)
- Configure attributes
  - [Convert between properties and attributes](https://lit-element.polymer-project.org/guide/properties#conversion)
  - [Configure observed attributes](https://lit-element.polymer-project.org/guide/properties#observed-attributes)
  - [Configure reflected attributes](https://lit-element.polymer-project.org/guide/properties#reflected-attributes)
- Configure property accessors
  - [Create your own property accessors](https://lit-element.polymer-project.org/guide/properties#accessors-custom)
  - [Prevent LitElement from generating a property accessor](https://lit-element.polymer-project.org/guide/properties#accessors-noaccessor)
- [Configure property changes](https://lit-element.polymer-project.org/guide/properties#haschanged)

### 개요

살펴볼 사항

- 선언된 속성이 변경될 때, 엘리멘트가 업데이트가 될지 결정

- Capture **instance values** for declared properties. Apply any property values that are set before the browser registers a custom element definition.
- Set up an observed (not reflected) attribute with the **lowercased name** of each property.
- Property 설정 `String`, `Number`, `Boolean`, `Array`, and `Object`.
- Use direct comparison (`oldValue !== newValue`) to **test** for property changes.
- Apply any property options and accessors declared by a superclass.

아직 이해가 되지 않는 부분은 영어 처리

> **Remember to declare all of the properties that you want LitElement to manage.** 
> For the property features above to be applied, you must [declare the property](https://lit-element.polymer-project.org/guide/properties#declare).
>
> lit-html와 다르게 type 선언 부분이 생김

### ★★Property options

The following options are available:

- `converter`: [Convert between properties and attributes](https://lit-element.polymer-project.org/guide/properties#conversion). (수동 검사[to, from])
- `type`: [Use LitElement’s default attribute converter](https://lit-element.polymer-project.org/guide/properties#conversion-type). (type)
- `attribute`: [Configure observed attributes](https://lit-element.polymer-project.org/guide/properties#observed-attributes). (attr -> prop)
- `reflect`: [Configure reflected attributes](https://lit-element.polymer-project.org/guide/properties#reflected-attributes). (prop -> attr)
- `noAccessor`: Whether to set up a default [property accessor](https://lit-element.polymer-project.org/guide/properties#accessors).
- `hasChanged`: Specify what constitutes a [property change](https://lit-element.polymer-project.org/guide/properties#haschanged).

type 선언말고도 할 수 있는 부분이 많네

### Declare properties

```js
// properties getter
static get properties() {
    return {
        prop1: { type: String },
        prop2: { type: Number },
        prop3: { type: Boolean }
    }
}
```

```js
// 만약, `constructor`를 이용해 초기화해주려면, 항상 `super()`를 처음에 작성한다
constructor() {
	super();
	this.prop1 = 'Hello';
}
```

#### code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { return {
    prop1: { type: String },
    prop2: { type: Number },
    prop3: { type: Boolean },
    prop4: { type: Array },
    prop5: { type: Object }
  };}

  constructor() {
    super();
    this.prop1 = 'Hello World';
    this.prop2 = 5;
    this.prop3 = false;
    this.prop4 = [1,2,3];
    this.prop5 = { subprop1: 'prop 5 subprop1 value' }
  }

  render() {
    return html`
      <p>prop1: ${this.prop1}</p>
      <p>prop2: ${this.prop2}</p>
      <p>prop3: ${this.prop3}</p>
      <p>prop4[0]:</p>${this.prop4[0]}</p>
      <p>prop5.subprop1: ${this.prop5.subprop1}</p>
    `;
  }
}

customElements.define('my-element', MyElement);
```

### Initialize property

```js
static get properties() {
    return {
        prop1: { type: String }
    }
        
    constructor() {
        super()
        this.prop1 = 'Hello'
    }
}
```

#### code example

```js
import { LitElement, html } from 'lit-element'

class MyElement extends LitElement {
    static get properties() {
        prop1: { type: String },
        prop2: { type: Number },
        prop3: { type: Boolean},
        prop4: { type: Array },
		prop5: { type: Object }
    }
    
    constructor() {
        super()
        this.prop1 = 'Hello'
        this.prop2 = 5
        this.prop3 = true
        this.prop4 = [1,2,3]
        this.prop5 = { 
            stuff: `h1`,
            otherStuff: `wow`
		}
    }
    
    render() {
        return html`
		<p>prop1: ${this.prop1}</p>
		<p>prop2: ${this.prop2}</p>
		<p>prop3: ${this.prop3}</p>
		<p>prop4: ${this.prop4.map((item, index) => 
        	html`<span>[${index}]: ${item}&nbsp;</span>`)}
		</p>

		<p>prop5:
			${Object.keys(this.prop5).map(item => 
            	html`<span>${item}: ${this.prop5[item]}$nbsp;</span>`)}
		</p>
		`
    }
}

customElements.define(`my-element`, MyElement)
```

#### Initialize property values from attributes in markup

index.html

```html
<my-element 
  mystring="hello world"
  mynumber="5"
  mybool
  myobj='{"stuff":"hi"}'
  myarray='[1,2,3,4]'></my-element>
```

이렇게 초기화한 값이 `constructor()`보다 우선순위로 적용됨

### Configure attributes

#### properties와 attribute간 전환

properties는 여러 타입을 가지지만, attribute는 항상 문자열이다. 
This impacts the [observed attributes](https://lit-element.polymer-project.org/guide/properties#observed-attributes) and [reflected attributes](https://lit-element.polymer-project.org/guide/properties#reflected-attributes) of non-string properties

- attribue를 **observe**를 사용하여 관찰할 때, attribute는 항상 문자열에서 property type값으로 변환된다.
- attribute를 **reflect**, property는 문자열로 변환된다.

#### General converter 사용하기

```js
// Use LitElement's default converter 
prop1: { type: String },
prop2: { type: Number },
prop3: { type: Boolean },
prop4: { type: Array },
prop5: { type: Object }
```

##### attribute -> property

- String: 변화없음
- Number: `Number(attributeValue)` 처리를 함
- Boolean
  - non-null: true
  - null·undefined: false

- Object·Array: JSON.parse(attributeValue)

##### property -> attribute

- String
  - null: remove attr
  - undefined:  don't change attr
  - non-null: change prop

- Number
  - null: remove attr
  - undefined: don't change attr
  - non-null: change prop

- Boolean
  - true: create attr
  - false: remove attr
- Object · Array
  - null·undefined: remove attr
  - non-null: JSON.stringify(prop)

```js
import { LitElement, html } from 'lit-element'

class MyElement extends LitElement {
    static get properties() {
        return {
            prop1: { type: String, reflect: true },
        	prop2: { type: Number, reflect: true },
        	prop3: { type: Boolean, reflect: true },
        	prop4: { type: Array, reflect: true },
        	prop5: { type: Object, reflect: true }   
        }     
    }
    
    constructor() {
        super()
        this.prop1 = ''
        this.prop2 = 0
        this.prop3 = false
        this.prop4 = []
        this.prop5 = {}
    }
    
    attributeChangedCallback(name, oldval, newval) {
        console.log(`attr change: `, name, newval)
        super.attributeChangeCallback(name, oldval, newval)
    }
    
    render() {
        return html`
			<p>prop1 ${this.prop1}</p>
			<p>prop2 ${this.prop2}</p>
			<p>prop3 ${this.prop3}</p>
			<p>prop4: ${this.prop4.map((item, index) => 
            	html`<span>[${index}]: ${item}&nbsp;</span>`)}
			</p>

			<p>prop5:
				${Object.keys(this.prop5).map(item => {
            		html`<span>${item}: ${this.prop5[item]}&nbsp;</span>}`
		        })}
			</p>

			<button @click="${this.changeProperties}">Change Prop</button>
			<button @click="${this.changeAttributes}">Change Attr</button>
		`
    }
    
    changeAttributes() {
        let randy = Math.floor(Math.random() * 10)
        let myBool = this.getAttributes(`prop3`)
        
        this.setAttribute(`prop1`, randy.toString)
        this.setAttribute(`prop2`, randy.toString)
        this.setAttribute(`prop3`, myBool ? `` : null)
        this.setAttribute(`prop4`, JSON.stringify([...this.prop4, randy]))
        this.setAttribute(`prop5`,
        	JSON.stringify(Object.assign({}, this.prop5, {[randy]: randy})))
        this.requestUpdate()
    }
    
    changeProperties() {
        let randy = Math.floor(Math.random() * 10)
        let myBool = this.prop3
        
        this.prop1 = randy.toString()
        this.prop2 = randy
        this.prop3 = !myBool
        this.prop4 = [...this.prop4, randy]
        this.prop5 = Object.assign({}, this.prop5, {[randy]: randy})
    }
    
    updated(changedProperties) {
        changedProperties.forEach((oldvalue, propName) => {
            console.log(`${propName} change. oldValue: ${oldValue}`)
        })
    }
}

customElements.define(`my-element`, MyElement)
```

> Object.assign()
>
> 객체는 키값이 중복없이 객체를 반환함
> `JSON.stringify(Object.assign({}, this.prop5, {[randy]: randy})));`
>
> `{[randy]: randy}`로 키값에서 변수를 받네

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

#### Custom converter 사용하기

```js
prop1: {
    converter: {
        fromAttribute: (value, type) => {
            
        },
        toAttribute: (value, type) => {
            
        }
    }
}
```

> During an update:
>
> - If `toAttribute` returns `null`, the attribute is removed.
> - If `toAttribute` returns `undefined`, the attribute is not changed.

##### example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { return {
    myProp: {
      reflect: true,
      converter: {
        toAttribute(value) {
          console.log('myProp\'s toAttribute.');
          console.log('Processing:', value, typeof(value));
          let retVal = String(value);
          console.log('Returning:', retVal, typeof(retVal));
          return retVal;
        },

        fromAttribute(value) {
          console.log('myProp\'s fromAttribute.');
          console.log('Processing:', value, typeof(value));
          let retVal = Number(value);
          console.log('Returning:', retVal, typeof(retVal));
          return retVal;
        }
      }
    },

    theProp: {
      reflect: true,
      converter(value) {
        console.log('theProp\'s converter.');
        console.log('Processing:', value, typeof(value));

        let retVal = Number(value);
        console.log('Returning:', retVal, typeof(retVal));
        return retVal;
      }},
  };}

  constructor() {
    super();
    this.myProp = 'myProp';
    this.theProp = 'theProp';
  }

  attributeChangedCallback(name, oldval, newval) {
    // console.log('attribute change: ', name, newval);
    super.attributeChangedCallback(name, oldval, newval);
  }

  render() {
    return html`
      <p>myProp ${this.myProp}</p>
      <p>theProp ${this.theProp}</p>

      <button @click="${this.changeProperties}">change properties</button>
      <button @click="${this.changeAttributes}">change attributes</button>
    `;
  }

  changeAttributes() {
    let randomString = Math.floor(Math.random()*100).toString();
    this.setAttribute('myprop', 'myprop ' + randomString);
    this.setAttribute('theprop', 'theprop ' + randomString);
    this.requestUpdate();
  }

  changeProperties() {
    let randomString = Math.floor(Math.random()*100).toString();
    this.myProp='myProp ' + randomString;
    this.theProp='theProp ' + randomString;
  }
}
customElements.define('my-element', MyElement);
```

이 부분은 아직 자연스럽게 코딩할 수 없을 듯, 많은 프로젝트에서 사용해봐야 할 듯
기능만 숙지하자

#### observe

attr이 변경될 때마다, `attributeChangedCallback` 을 실행시킴

##### 사용법

```js
// Observed attribute will be called my-prop
myProp: { attribute: 'my-prop' }

// No observed attribute for this property
myProp: { attribute: false }

// observed attribute for this property (default)
myProp: { attribute: true }
```

##### code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { return {
    myProp: { attribute: true },
    theProp: { attribute: false },
    otherProp: { attribute: 'other-prop' },
  };}

  constructor() {
    super();
    this.myProp = 'myProp';
    this.theProp = 'theProp';
    this.otherProp = 'otherProp';
  }

  attributeChangedCallback(name, oldval, newval) {
    console.log('attribute change: ', name, newval);
    super.attributeChangedCallback(name, oldval, newval);
  }

  render() {
    return html`
      <p>myProp ${this.myProp}</p>
      <p>theProp ${this.theProp}</p>
      <p>otherProp ${this.otherProp}</p>

      <button @click="${this.changeAttributes}">change attributes</button>
    `;
  }

  changeAttributes() {
    let randomString = Math.floor(Math.random()*100).toString();
    this.setAttribute('myprop', 'myprop ' + randomString);
    this.setAttribute('theprop', 'theprop ' + randomString);
    this.setAttribute('other-prop', 'other-prop ' + randomString);
    this.requestUpdate();
  }

  updated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
      console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
  }
}
customElements.define('my-element', MyElement);
```

#### reflect

`property`가 변경될 때마다, `attribute`에 반영된다.

```js
myProp: { reflect: true }
```

property가 수정되면, LitElement의 기능 중 하나인 `toAttribute`가 attribute를 수정한다.

- toAttribute가 null을 반환하면, attr은 제거된다.
- toAttribute가 undefined을 반환하면, attr은 수정되지 않는다.
- If `toAttribute` itself is undefined, the property value is set to the attribute value without conversion.
  (해봐야 알듯)

##### code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { return {
    myProp: { reflect: true }
  };}

  constructor() {
    super();
    this.myProp='myProp';
  }

  attributeChangedCallback(name, oldval, newval) {
    console.log('attribute change: ', newval);
    super.attributeChangedCallback(name, oldval, newval);
  }

  render() {
    return html`
      <p>${this.myProp}</p>

      <button @click="${this.changeProperty}">change property</button>
    `;
  }

  changeProperty() {
    let randomString = Math.floor(Math.random()*100).toString();
    this.myProp='myProp ' + randomString;
  }

}
customElements.define('my-element', MyElement);
```

### accessors

#### example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { 
    return { prop: { type: Number } };
  }

  set prop(val) {
    let oldVal = this._prop;
    this._prop = Math.floor(val);
    this.requestUpdate('prop', oldVal);
  }

  get prop() { return this._prop; }

  constructor() {
    super();
    this._prop = 0;
  }

  render() {
    return html`
      <p>prop: ${this.prop}</p>
      <button @click="${() =>  { this.prop = Math.random()*10; }}">
        change prop
      </button>
    `;
  }
}
customElements.define('my-element', MyElement);
```

#### Prevent LitElement from generating a property accessor

##### 사용법

```js
static get properties() { 
  return { myProp: { type: Number, noAccessor: true } }; 
}
```

noAccessor가 true면, prop이 getter와 setter 접근자에 의해 변경된다.

noAccessor가 false면 getter와 setter가 사용되지 않는다.

##### code example

super-element.js

```js
import { LitElement, html } from 'lit-element';

export class SuperElement extends LitElement {
  static get properties() {
    return { prop: { type: Number } };
  }

  set prop(val) {
    let oldVal = this._prop;
    this._prop = Math.floor(val);
    this.requestUpdate('prop', oldVal);
  }

  get prop() { return this._prop; }

  constructor() {
    super();
    this._prop = 0;
  }

  render() {
    return html`  
      <p>prop: ${this.prop}</p>
      <button @click="${() => { this.prop = Math.random()*10; }}">
        change prop
      </button>
  `;
  }
}
customElements.define('super-element', SuperElement);

```

sub-element.js

```js
import { SuperElement } from './super-element.js';

class SubElement extends SuperElement {  
  static get properties() { 
    return { prop: { reflectToAttribute: true, noAccessor: true } };
  }
}

customElements.define('sub-element', SubElement);
```

### Configure property changes

#### `hasChanged`: prop이 변경됬는지 검사

true가 반환되면, update를 실행한다. 

false가 반환되면, 변화가 없다는 뜻이다.

#### 사용법

```js
myProp: { hasChanged(newVal, oldVal) {
    if (newVal > oldVal) {
        return true
    } else {
        return false
    }
}}
```

#### code example

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties(){ return {
    myProp: {
      type: Number,

      /**
       * Compare myProp's new value with its old value.
       *
       * Only consider myProp to have changed if newVal is larger than
       * oldVal.
       */
      hasChanged(newVal, oldVal) {
        if (newVal > oldVal) {
          console.log(`${newVal} > ${oldVal}. hasChanged: true.`);
          return true;
        }
        else {
          console.log(`${newVal} <= ${oldVal}. hasChanged: false.`);
          return false;
        }
      }
    }};
  }

  constructor(){
    super();
    this.myProp = 1;
  }

  render(){
    return html`
      <p>${this.myProp}</p>
      <button @click="${this.getNewVal}">get new value</button>
    `;
  }

  updated(){
    console.log('updated');
  }

  getNewVal(){
    let newVal = Math.floor(Math.random()*10);
    this.myProp = newVal;
  }

}
customElements.define('my-element', MyElement);
```

-----------

## Events

### 목차

- 개요
  - 이벤트 핸들러 추가 위치
  - `this` 사용
  - useCase
  - Fire Event
  - Custom-event handler
- Shadow Dom에서 이벤트
  - 이벤트 버블링
  - 이벤트 리타겟팅
  - 커스텀 이벤트

### 개요

#### 이벤트 핸들러 추가 위치

##### 컴포넌트를 이용한 이벤트 추가

`@이벤트명`을 이용

```js
render() {
  return html`<button @click="${this.handleClick}">`;
}
```

##### Dom으로 추가되기 전에 받는 이벤트는 `constructor()`에서 선언

```js
constructor() {
  super();
  this.addEventListener('DOMContentLoaded', this.handleLoaded);
}
```

##### firstUpdated()

LifeCycle을 이용한 이벤트 추가, 처음으로 업데이트되거나 렌더링됬을 때 실행된다.

```js
firstUpdated(changedProperties) {
  this.addEventListener('click', this.handleClick);
}
```

##### connectedCallback()

커스텀 엘리먼트에서 존재하던 LifeCycle. 엘리먼트가 Dom에 추가되면 발생하는 LifeCycle.

##### disconnectedCallback()

엘리먼트가 Dom에서 제거되면 발생하는 LifeCycle. 
connectedCallback에서 생성된 이벤트들을 여기서 제거해준다.

###### 예제

```js
connectedCallback() {
  super.connectedCallback();
  document.addEventListener('readystatechange', this.handleChange);
}
disconnectedCallback() {
  document.removeEventListener('readystatechange', this.handleChange);
  super.disconnectedCallback();
}
```

#### `this`사용

##### 예제

```js
class MyElement extends LitElement {
  render() {
    return html`<button @click="${this.handleClick}">click</button>`;
  }
  handleClick(e) {
    console.log(this.prop);
  }
}
```

#### Fire Event

##### Fire Custom Event

```js
class MyElement extends LitElement {
  render() {
    return html`<div>Hello World</div>`;
  }
  firstUpdated(changedProperties) {
    let event = new CustomEvent('my-event', {
      detail: {
        message: 'Something important happened'
      }
    });
    this.dispatchEvent(event);
  }
}
```

##### Fire Standard Event

```js
class MyElement extends LitElement {
  render() {
    return html`<div>Hello World</div>`;
  }
  updated(changedProperties) {
    let click = new Event('click');
    this.dispatchEvent(click);
  }
}
```

##### LitElement 기반에 이벤트 핸들러 추가

```html
<my-element @my-event="${(e) => { console.log(e.detail.message) }}"></my-element>
```

###### 기본 방식

```js
const myElement = document.querySelector('my-element');
myElement.addEventListener('my-event', (e) => {console.log(e)});
```

### Shadow Dom에서 이벤트

#### 이벤트 버블링

버블링인지 아닌지 확인하는 방법

```js
handleEvent(e){
  console.log(e.bubbles);
}
```

#### Event retargeting

```html
<my-element onClick="(e) => console.log(e.target)"></my-element>
```

```js
render() {
  return html`
    <button id="mybutton" @click="${(e) => console.log(e.target)}">
      click me
    </button>`;
}
```

##### 이벤트 발생원인 찾을 때

```js
handleMyEvent(event) {
  console.log('Origin: ', event.composedPath()[0]);
}
```

#### 커스텀 이벤트

버블링은 Shadow Dom 내부에서 발생하기 때문에, Shadow-root에 도달하면 중지된다.

만약 shadow-root를 통과하고 싶다면, 다음과같이 설정한다.

```js
firstUpdated(changedProperties) {
  let myEvent = new CustomEvent('my-event', { 
    detail: { message: 'my-event happened.' },
    bubbles: true, 
    composed: true });
  this.dispatchEvent(myEvent);
}
```

## LifeCycle

### 목차

- 개요
- Methods and Prop
  - `prop.hasChanged()`
  - `requestUpdate()`
  - `performUpdate()`
  - `shouldUpdate()`
  - `update()`
  - `render()`
  - `firstUpdated()`
  - `updated()`
  - `updateComplete()`
- 예제

### 개요

Update LifeCycle:

1. property 설정.
2. 업데이트 필요한지 확인, 필요하다면 요청.
3. 업데이트
   - Process properties and attributes.
   - Render the element.
4. Resolve a Promise, indicating that the update is complete.

#### LitElement and the browser event loop

The browser executes JavaScript code by processing a queue of tasks in the [event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop).
In each iteration of the event loop, the browser takes a task from the queue and runs it to completion.

When the task completes, before taking the next task from the queue, 
the browser allocates time to perform work from other sources
—including DOM updates, user interactions, and the microtask queue.

By default, LitElement updates are requested asynchronously, and queued as microtasks. 
This means that Step 3 above (Perform the update) is executed at the end of the next iteration of the event loop.

You can change this behavior so that Step 3 awaits a Promise before performing the update. 
See [`performUpdate`](https://lit-element.polymer-project.org/guide/lifecycle#performUpdate) for more information.

For a more detailed explanation of the browser event loop, see [Jake Archibald’s article](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/).

> 흠냐... 코드가 없어서 생략

#### Lifecycle callbacks

- `connectedCallback`: Invoked when a component is added to the document’s DOM.
- `disconnectedCallback`: Invoked when a component is removed from the document’s DOM.
- `adoptedCallback`: Invoked when a component is moved to a new document.
- `attributeChangedCallback`: Invoked when component attribute changes.

> **Be aware that adoptedCallback is not polyfilled.**

> 커스텀 엘리먼트와 동일함

#### Promises and asynchronous functions

```js
// `async` makes the function return a Promise & lets you use `await`
async myFunc(data) {
  // Set a property, triggering an update
  this.myProp = data;

  // Wait for the updateComplete promise to resolve
  await this.updateComplete;
  // ...do stuff...
  return 'done';
}
```

### Method and Prop

#### prop.hasChanged()

prop이 변경됬는지 검사

#### requestUpdate()

return값, Promise
Returns the [`updateComplete` Promise](https://lit-element.polymer-project.org/guide/lifecycle#updatecomplete), which resolves on completion of the update.

```js
// Manually start an update
this.requestUpdate();

// Call from within a custom property setter
this.requestUpdate(propertyName, oldValue);
```

> 왜 oldValue를 집어넣는거지?, 아무거나 집어넣어도 update를 하는 것으로 확인 
> (다른 method에서 oldValue를 쓰나?)

##### 이전: 요소 업데이트를 수동으로 했을 경우

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  constructor() {
    super();

    // Request an update in response to an event
    this.addEventListener('load-complete', async (e) => {
      console.log(e.detail.message);
      console.log(await this.requestUpdate());
    });
  }
  render() {
    return html`
      <button @click="${this.fire}">Fire a "load-complete" event</button>
    `;
  }
  fire() {
    let newMessage = new CustomEvent('load-complete', {
      detail: { message: 'hello. a load-complete happened.' }
    });
    this.dispatchEvent(newMessage);
  }
}
customElements.define('my-element', MyElement);
```

##### `getter`, `setter` 으로, 사용했을 경우

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { 
    return { prop: { type: Number } };
  }

  set prop(val) {
    let oldVal = this._prop;
    this._prop = Math.floor(val);
    this.requestUpdate('prop', oldVal);
  }

  get prop() { return this._prop; }

  constructor() {
    super();
    this._prop = 0;
  }

  render() {
    return html`
      <p>prop: ${this.prop}</p>
      <button @click="${() =>  { this.prop = Math.random()*10; }}">
        change prop
      </button>
    `;
  }
}
customElements.define('my-element', MyElement);
```

#### performUpdate()

return값, `void` or `Promise`

```js
/**
 * Implement to override default behavior.
 */
async performUpdate() {
  await new Promise((resolve) => requestAnimationFrame(() => resolve()));
  super.performUpdate();
}
```

##### Full example code

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() { return { prop1: { type: Number } }; }

  constructor() {
    super();
    this.prop1 = 0;
  }

  render() {
    return html`
      <p>prop1: ${this.prop1}</p>
      <button @click="${() => this.prop1=this.change()}">Change prop1</button>
    `;
  }

  async performUpdate() {
    console.log('Requesting animation frame...');
    await new Promise((resolve) => requestAnimationFrame(() => resolve()));
    console.log('Got animation frame. Performing update');
    super.performUpdate();
  }

  change() {
    return Math.floor(Math.random()*10);
  }
}
customElements.define('my-element', MyElement);
```

#### shouldUpdate(changedProperties)

return값, Boolean
If `true`, update proceeds. Default return value is `true`.

updates: YES

특정 prop이 바뀔 때만 업데이트를 시킬 수 있음

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      prop1: { type: Number },
      prop2: { type: Number }
    };
  }
  constructor() {
    super();
    this.prop1 = 0;
    this.prop2 = 0;
  }

  render() {
    return html`
      <p>prop1: ${this.prop1}</p>
      <p>prop2: ${this.prop2}</p>
      <button @click="${() => this.prop1=this.change()}">Change prop1</button>
      <button @click="${() => this.prop2=this.change()}">Change prop2</button>
    `;
  }

  /**
   * Only update element if prop1 changed.
   */
  shouldUpdate(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
      console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    // prop1이 바뀔 때만, update 
    return changedProperties.has('prop1');
  }

  change() {
    return Math.floor(Math.random()*10);
  }
}
customElements.define('my-element', MyElement);
```

#### update(changedProperties)

Reflects property values to attributes and calls `render` to render DOM via lit-html. Provided here for reference. You don’t need to override or call this method.

#### render()

return값, TemplateResult 

#### firstUpdated(changedProperties)

Updates: YES

`Updated` 이전에 호출된다.

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      textAreaId: { type: String },
      startingText: { type: String }
    };
  }
  constructor() {
    super();
    this.textAreaId = 'myText';
    this.startingText = 'Focus me on first update';
  }
  render() {
    return html`
      <textarea id="${this.textAreaId}">${this.startingText}</textarea>
    `;
  }
  firstUpdated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
      console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    const textArea = this.shadowRoot.getElementById(this.textAreaId);
    textArea.focus();
  }
}
customElements.define('my-element', MyElement);
```

#### updated(changedProperties)

Updates: YES

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      prop1: { type: Number },
      prop2: { type: Number }
    };
  }
  constructor() {
    super();
    this.prop1 = 0;
    this.prop2 = 0;
  }
  render() {
    return html`
      <style>button:focus { background-color: aliceblue; }</style>

      <p>prop1: ${this.prop1}</p>
      <p>prop2: ${this.prop2}</p>

      <button id="a" @click="${() => this.prop1=Math.random()}">prop1</button>
      <button id="b" @click="${() => this.prop2=Math.random()}">prop2</button>
    `;
  }
  updated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
      console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    let b = this.shadowRoot.getElementById('b');
    b.focus();
  }
}
customElements.define('my-element', MyElement);
```

#### updateComplete()

```js
await this.updateComplete;
// do stuff

// or
this.updateComplete.then(() => { /* do stuff */ });
```

##### 예제

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {
  static get properties() {
    return {
      prop1: { type: Number }
    };
  }

  constructor() {
    super();
    this.prop1 = 0;
  }

  render() {
    return html`
      <p>prop1: ${this.prop1}</p>
      <button @click="${this.changeProp}">prop1</button>
    `;
  }

  async getMoreState() {
    return;
  }

  async changeProp() {
    this.prop1 = Math.random();
    await Promise.all([this.updateComplete, this.getMoreState()]);
    console.log('Update complete. Other state completed.');
  }
}

customElements.define('my-element', MyElement);
```

### 예제

#### Control when updates are processed

##### performUpdate

```js
async performUpdate() {
  await new Promise((resolve) => requestAnimationFrame(() => resolve());
  super.performUpdate();
}
```

#### Customize which property changes should cause an update

##### shouldUpdate

```js
shouldUpdate(changedProps) {
  return changedProps.has('prop1');
}
```

#### Customize what constitutes a property change

##### `hasChanged()`

#### Manage property changes and updates for object subproperties

Mutations (changes to object subproperties and array items) are not observable.
Instead, either rewrite the whole object, or call [`requestUpdate`](https://lit-element.polymer-project.org/guide/lifecycle#requestupdate) after a mutation.

```js
// Option 1: Rewrite whole object, triggering an update
this.prop1 = Object.assign({}, this.prop1, { subProp: 'data' });

// Option 2: Mutate a subproperty, then call requestUpdate
this.prop1.subProp = 'data';
this.requestUpdate();
```

> 아하, `requestUpdate()`는 prop.subProp의 변화를 감지해주네

#### Update in response to something that isn’t a property change

##### requestUpdate

```js
// Request an update in response to an event
this.addEventListener('load-complete', async (e) => {
  console.log(e.detail.message);
  console.log(await this.requestUpdate());
});
```

#### Request an update regardless of property changes

```js
this.requestUpdate();
```

#### Request an update for a specific property

```js
let oldValue = this.prop1;
this.prop1 = 'new value';
this.requestUpdate('prop1', oldValue);
```

#### Do something after the first update

```js
firstUpdated(changedProps) {
  console.log(changedProps.get('prop1'));
}
```

#### Do something after every update

```js
updated(changedProps) {
  console.log(changedProps.get('prop1'));
}
```

#### Do something when the element next updates

```js
await this.updateComplete;
// do stuff

// or

this.updateComplete.then(() => {
  // do stuff
});
```

#### Wait for an element to finish updating

```js
let done = await updateComplete;

// or
updateComplete.then(() => {
  // finished updating
});
```

----------

```
updateComplete.then(() => {
  // finished updating
});
```

-----------

## Publish an element

### npm으로 게시하는 방법

ES2017이상 문법으로 작성하는 것을 추천. 그렇지 않다면, 변환을 해야함

#### package.json 수정

```json
{
  "main": "my-element.js",
  "module": "my-element.js"
}
```

#### 사용방법 README 작성

#### [npm packages 가이드에 따라 작성](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)

#### [한글 가이드 블로그 글](https://heropy.blog/2019/01/31/node-js-npm-module-publish/)

### Transpiling with Babel

To transpile a LitElement component that uses proposed JavaScript features, use Babel.

Install Babel and the Babel plugins you need. For example:

```bash
npm install --save-dev @babel/core
npm install --save-dev @babel/plugin-proposal-class-properties
npm install --save-dev @babel/plugin-proposal-decorators
```

Configure Babel. For example:

#### **babel.config.js**

```js
const plugins = [
  '@babel/plugin-proposal-class-properties',
  ['@babel/plugin-proposal-decorators', { decoratorsBeforeExport: true } ],
];

module.exports = { plugins };
```

You can run Babel via a bundler plugin such as [rollup-plugin-babel](https://www.npmjs.com/package/rollup-plugin-babel), or from the command line. See the [Babel documentation](https://babeljs.io/docs/en/) for more information.

## Use a component

### 목차

- lit-element 사용하기
- Build for production
- Polyfill

### lit-element 사용하기

1. npm install 모듈명

   ```bash
   npm install some-package-name
   ```

2. Javascript에서 사용

   ```js
   import 'some-package-name';
   ```

   HTML에서 사용

   ```html
   <script type="module">
   import './path-to/some-package-name/some-component.js';
   </script>
   ```

   Or:

   ```html
   <script type="module" src="./path-to/some-package-name/some-component.js"></script>
   ```

3. 이후, READE에 따른 컴포넌트 사용

   ```html
   <some-component></some-component>
   ```

### Build for production

webpack과 비슷한 rollup을 써서 하는 듯

```js
import resolve from 'rollup-plugin-node-resolve';

export default {
  // If using any exports from a symlinked project, uncomment the following:
  // preserveSymlinks: true,
	input: ['src/index.js'],
	output: {
		file: 'build/index.js',
		format: 'es',
		sourcemap: true
	},
	plugins: [
    resolve()
  ]
};
```

rollup과 webpack3을 비교하는 글을 읽고 요약해봄(그런데 지금은 webpack4잖아?)

#### rollup 장점

1. webpack은 ESM형태 번들이 안된다고함 (ts -> js ?)
2. webpack은 빌드시, 중복코드 제거기능이 없다고함

> Webpack 에서는 `import`는 `__webpack_require__`로 바뀌고 `export`는 `exports 오브젝트`로 바뀌면서 코드가 증가합니다. 
> 그래서 상수를 사용하면 상수 이름을 그대로 쓰고 uglify가 되지 않기 때문에 오히려 코드가 증가할 수 있습니다.
> Webpack에서도 `ModuleConcatenationPlugin`이 있어 Rollup과 비슷한 효과를 볼 수 있습니다. 
> 하지만 typescript, babel 플러그인을 통해 생긴 함수의 중복은 제거할 수 없습니다.
> 대표적인 예로 `assign`, `extends` 등과 같이 ES6 이상의 문법을 ES5로 바꾸면서 생기는 polyfill이 있습니다. 
> 이 함수는 파일(모듈)마다 존재하고 각자 다른 함수로 인식해 파일 개수만큼 늘어납니다.

3. webpack이 평균 빌드 용량이 큰 듯

4. webpack3에서 Tree Shaking이 잘 안된다고 함

#### rollup 단점

1. entry(input, output)가 많아질수록 복잡해질 수 있습니다.
2. plugin의 규칙을 정할 수 없습니다.

>  흐음... 내가 rollup으로 바꿀 수가 있나?
>
> webpack에 의존되는 기술이 뭐가 있지?
> css파일모아 합치기, scss, babel, postcss(autoprefixer), webpack-dev-server 
> 생각보다 의존성이 있네... 
>
> 그런데 webpack4에서 극복한 느낌이군

### Polyfill

#### 폴리필 하는법

1. npm install

   ```bash
   npm install --save-dev @webcomponents/webcomponentsjs
   ```

2. HTML Script 추가

   ```
   <head>  
     <script src="./path-to/custom-elements-es5-loader.js"></script>
     <script 
       src="path-to/webcomponents-loader.js"
       defer>
     </script> 
     <script type="module">
       // Take care of cases in which the browser runs this
       // script before it has finished running 
       // webcomponents-loader.js (e.g. Firefox script execution order)
       window.WebComponents = window.WebComponents || { 
         waitFor(cb){ addEventListener('WebComponentsReady', cb) }
       }
   
       WebComponents.waitFor(async () => { 
         import('./path-to/some-element.js');
       });
     </script>
   </head>
   <body>
     <some-element></some-element>
   </body>
   ```

3. Ensure that `node_modules/@webcomponents/webcomponentsjs/webcomponents-loader.js` and `node_modules/@webcomponents/webcomponentsjs/bundles/**.*` are included in your build.

>  **Do not transpile the polyfills.** Bundling them is okay.

----------

> 아직 lit-element를 잘 모르는 상태로 작업한 문서기 때문에, 
> 추후 프로젝트에 많이 사용한 뒤 요약본을 다시 작성해야겠다.