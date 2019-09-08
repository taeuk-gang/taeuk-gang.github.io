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

true가 반환되면, update를 실행한다. 
false가 반환되면, 변화가 없다는 뜻이다.

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

