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
        fromAttribute: (value, type) => {
            
        },
        toAttribute: (value, type) => {
            
        }
    }
}
```



### loop

### conditionals



> 작성 중...