---
title: lit-element 요약본
toc: true
date: 2019-09-07 12:15:22
tags:
    - Javascript
    - lit-element
categories:
    - Javascript콛
---

> 실제 써먹을 것 Code만 간략히 적기

## lit-element 개발환경

### lit-element 모듈 설치

```bash
npm install lit-element
```

### Javascript 작성

```js
import { LitElement, html } from 'lit-element';

class MyElement extends LitElement {  
  // lit-element관련 및 이벤트핸들러 입력 영역
    
  render(){
    return html`
      <p>A paragraph</p>
    `;
  }
    
  // 커스텀 함수 입력 영역
}

customElements.define('my-element', MyElement);
```

### 불러오기

#### HTML

```html
<script type="module" src="/path/to/my-element.js"></script>
```

#### Javascript

```js
import './my-element.js';
```

### babel 설정

#### 설치

```bash
npm install --save-dev @babel/core
npm install --save-dev @babel/plugin-proposal-class-properties
npm install --save-dev @babel/plugin-proposal-decorators
```

#### `babel.config.js` 설정

```js
const plugins = [
  '@babel/plugin-proposal-class-properties',
  ['@babel/plugin-proposal-decorators', { decoratorsBeforeExport: true } ],
];

module.exports = { plugins };
```

## 주의사항

- lit-element은 virtual Dom과 다르지만, Dom을 완전히 직접 조작하는 것은 비효율적이다.
  (가변형 데이터는 prop으로 관리하라)

## 사용하기

### prop

```js
static get properties() {
  return {
      myProp: String // converter, type, attribute, reflect, noAccessor, hasChanged;
  };
}
```

#### converter

```js
prop1: {
    converter: {
        fromAttribute: (value, type) => { // attr 변경시 실행
            
        },
        toAttribute: (value, type) => { // prop 변경시 실행
            
        }
    }
}
```

#### type

```js
static get properties() {
    return {
        prop1: { type: String },
        prop2: { type: Number },
        prop3: { type: Boolean }
    }
}
```

#### attribute (attr -> prop)

```js
// Observed attribute will be called my-prop
myProp: { attribute: 'my-prop' }

// No observed attribute for this property
myProp: { attribute: false }

// observed attribute for this property (default)
myProp: { attribute: true }
```

#### reflect (prop -> attr)

```js
myProp: { reflect: true }
```

#### noAccessor

```js
static get properties() { 
  return { myProp: { type: Number, noAccessor: true } }; 
}
```

noAccessor가 true면, prop이 getter와 setter 접근자에 의해 변경된다.
noAccessor가 false면 getter와 setter가 사용되지 않는다.

#### hasChanged

```js
myProp: { hasChanged: true }
```

true가 반환되면, update를 실행한다. 
false가 반환되면, 변화가 없다는 뜻이다.

```js
myProp: {
    type: Number,
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
```



### loop

```js
html`<ul>
  ${this.myArray.map(i => html`<li>${i}</li>`)}
</ul>`;
```

### conditionals

```js
html`
  ${this.myBool?
    html`<p>Render some HTML if myBool is true</p>`:
    html`<p>Render some other HTML if myBool is false</p>`}
`;
```

## 바인딩

- textContent: `<p>${...}</p>`
- attribute: `<p id="${...}"></p>`
- boolean attribute: `?checked="${...}"`
- property: `.value="${...}"`
- event handler: `@event="${...}"`

### textContent 바인딩

```js
html`<div>${this.prop1}</div>`
```

### attribute 바인딩

```js
html`<div id="${this.prop2}"></div>`
```

속성 값은 항상 문자열 또는 문자열로 반환될 수 있는 값이여야 한다.

### boolean attribute 바인딩

```js
html`<input type="checkbox" ?checked="${this.prop3}>i like pie</input>"`
```

`true`면 attribute로 추가되고, `false`면 사라진다.

### property 바인딩

```js
html`<input type="checkbox" .value="${this.prop4}" />`
```

### event handler 바인딩

```js
html`<button @click="${this.clickHandler}">pie?</button>`
```

## `<slot></slot>` light Dom 렌더링

### 설정(일반)

```js
render() {
    return html`
		<div>
			<slot></slot>
		</div>
	`;
}
```

### 설정(네이밍)

```js
render() {
    return html`
		<div>
			<slot name="one"></slot>
		</div>
	`;
}
```

### 사용(일반)

```html
<my-element>
	<p>Render me</p>
    <p>Me too</p>
    <p>Me three</p>
</my-element>
```

### 사용(네이밍)

```html
<my-element>
    <p slot="one">slot one</p>
</my-element>
```

## 일반적인 엘리먼트 태그 사용하기

예를 들면, `<header>`, `<article>`, `<footer>` 을 이용해 템플릿 구성하는 방법
커스텀 엘리먼트 특정 `define` 명령어와 HTML에서 `is` attr로도 구현 가능한 걸로 앎.

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

### import 이용시

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

## CSS 사용

- ★추천★: css``를 이용한 선언 (static styles property)
- `render` 안에 `<style>` 태그를 이용한 방법
- 외부 스타일 시트를 이용한 방법 `<link rel="stylesheet" href="..." />`

### css``

```js
class myElement extends LitElement {
    static get styles() {
        const mainColor = css`red`
        const unSafeStr = `red`
        
        return css`
		:host {
			display: block;
			color: ${mainColor};
			background-color: ${unsafeCSS(unSafeStr)};
		}
		`
    }
}

// or 여러개의 CSS 선언시

class MyElement extends LitElement {
    static get styles() {
        return [ css`:host { display: block; }`, ...]
    }
}
```

### `<style>`태그 사용

```js
import {LitElement, property} from 'lit-element';

class MyElement extends LitElement {
  const mainColor = css`blue`;

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

### 외부 스타일시트 사용

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

## shadow Dom에서의 CSS 작성법

### `:host(...)`

```css
:host {
    display: block;
    color: blue;
}

:host(.important) {
    color: red;
    font-weight: bold;
}
```

### `::slot`

```css
::slotted(*) { font-family: Roboto; }
::slotted(span) { color: blue; }
```

### 커스텀 엘리먼트명으로 CSS 작성

```css
my-element { 
    font-family: Roboto;
    font-size: 20;
    color: blue;
}
```

 `:host`보다 우선순위가 높다.

### CSS Value 적용

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

## Event

### `@이벤트명`을 이용

```js
render() {
  return html`<button @click="${this.handleClick}">`;
}
```

### Dom으로 추가되기 전에 받는 이벤트는 `constructor()`에서 선언

```js
constructor() {
  super();
  this.addEventListener('DOMContentLoaded', this.handleLoaded);
}
```

### `firstUpdated()`

```js
firstUpdated(changedProperties) {
  this.addEventListener('click', this.handleClick);
}
```

### `connectedCallback()`

### `discconectedCallback()`

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

### custom event

```html
<my-element @my-event="${(e) => { console.log(e.detail.message) }}"></my-element>
```

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

### standard event

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

### 이벤트 버블링

#### 버블링 확인

```js
handleEvent(e){
  console.log(e.bubbles);
}
```

### e.target

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

### composedPath

```js
handleMyEvent(event) {
  console.log('Origin: ', event.composedPath()[0]);
}
```

### shadow root 통과

```js
firstUpdated(changedProperties) {
  let myEvent = new CustomEvent('my-event', { 
    detail: { message: 'my-event happened.' },
    bubbles: true, 
    composed: true // 이부분!
  });
  this.dispatchEvent(myEvent);
}
```

## LifeCycle

### `requestUpdate()`

```js
// Manually start an update
this.requestUpdate();

// Call from within a custom property setter
this.requestUpdate(propertyName, oldValue);
```

### `performUpdate()`

```js
async performUpdate() {
  await new Promise((resolve) => requestAnimationFrame(() => resolve()));
  super.performUpdate();
}
```

### `shouldUpdate(changedProp)`

```js
shouldUpdate(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
        console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    // prop1이 바뀔 때만, update 
    return changedProperties.has('prop1');
}
```

### `update(changedProp)`

### `render()`

### `firstUpdated(changedProp)`

```js
firstUpdated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
        console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    const textArea = this.shadowRoot.getElementById(this.textAreaId);
    textArea.focus();
}
```

### `updated(changedProp)`

```js
updated(changedProperties) {
    changedProperties.forEach((oldValue, propName) => {
        console.log(`${propName} changed. oldValue: ${oldValue}`);
    });
    let b = this.shadowRoot.getElementById('b');
    b.focus();
}
```

### `updateComplete()`

```js
await this.updateComplete;
// do stuff

// or
this.updateComplete.then(() => { /* do stuff */ });
```

## npm 모듈로 만들기

#### [npm packages 가이드에 따라 작성](https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry)

#### [한글 가이드 블로그 글](https://heropy.blog/2019/01/31/node-js-npm-module-publish/)

## npm 모듈 사용하기

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

## Polyfill

1. npm install

   ```bash
   npm install --save-dev @webcomponents/webcomponentsjs
   ```

2. HTML Script 추가

   ```
   <head>  
     <!--script src="./path-to/custom-elements-es5-loader.js"></script-->
     <script src="path-to/webcomponents-loader.js"defer></script> 
     <script type="module">
       window.WebComponents = window.WebComponents || { 
         waitFor(cb){ addEventListener('WebComponentsReady', cb) }
       }
   
       WebComponents.waitFor(async () => { 
         import('./path-to/some-element.js');
       });
     </script>
   </head>
   ```

> 이유는 알 수 없는데 위 튜토리얼 사항대로 하면, ie11은 지원되지 않는다.
> 아래와 같이하면 되는걸로 확인

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.4.4/polyfill.min.js"></script>
    <script src="https://unpkg.com/@webcomponents/webcomponentsjs@2.2.10/bundles/webcomponents-sd-ce-pf.js"></script>
    <script src="https://unpkg.com/@webcomponents/webcomponentsjs@2.2.10/custom-elements-es5-adapter.js"></script>
    <script src="./main-bundle.js"></script>
```

