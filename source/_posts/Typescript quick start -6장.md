---
title: Quick Start Typescript ~ 6장 정리
toc: true
date: 2020-03-22 20:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성했습니다.
>



## 6장 함수 목차

1. 사용법 (Js vs Ts 비교)
2. 매개변수
   1. 초기값 지정
   2. `...rest` 매개변수
   3. 선택 매개변수 지정
   4. 오버로드
3. 익명 함수
   1. 화살표 함수
   2. 타입 선언
   3. 콜백 함수

##  📝p151. 함수 (Js vs Ts 비교)

### js의 경우

```js
function max(x, y) {
    return x > y ? x : y;
}

max(1, 10);	// 10
max(1, 10, 12);	// 10, 이후 인자값은 무시당함
max(`a`, `b`);	// `b`
max(`c`, `aaa`);	//`c`, 으음... 문자열 첫번째값의 아스키 코드값인가?
```

#### 결과

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323073951872.png)

### ts의 경우

```typescript
function max(x: number, y: number): number {
    return x > y ? x : y;
}
max(1, 10);	// 10
max(1, 10, 12);	// Error: 인자 개수 에러
max(`a`, `b`);	// Error: 인자 타입 에러
```

#### 결과

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323073806380.png)



## 📝p156. 매개변수 초기값

ES6부터 매개변수의 값을 지정 받지 않아도, 초기값으로 값을 지정해줄 수 있게됨

코드도 간결해지고 complexity도 낮아짐

### ES6 이전

```js
function test(param) {
    param = param || `초기값`;
    console.log(param);
}
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323074303810.png)

### ES6 이후

```js
function test(param = `초기값`) {
    console.log(param);
}
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323074341475.png)

## 📝p159. 나머지 매개변수

ES6부터 생긴 기능, 정의되지 않은 매개변수를 받기 편해짐

### ES6 이전

`arguments`는 잘 사용하지 않는 방식으로 알려짐 (보안)

```js
function test() {
	console.log(arguments);
}
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323075104117.png)

### ES6 이후

```js
function test(...args) {
    console.log(args);
}
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323075122656.png)

### 나머지 매개변수 타입 지정법

> 나머지 매개변수도 타입 지정이 가능한 것을 처음 알았다

```typescript
// 처음 1개 값을 지정하면, concat()같이 아무것도 받지않았을 때는 유효하지않음
function concat(a: string, ...restStr: string[]): string {
    return `${a} ${restStr.join(` `)}`;
}
```



## 📝p162. 선택 매개변수

### js의 경우

```js
function sum(a, b) {
    console.log(arguments);
    return a + b;
}

sum(1);	// NaN
sum(1, 2);	// 3
sum(1, 2, 3)	// 3, 이후 파라미터를 사용되진 않지만, 받음
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323080049535.png)

### ts

```typescript
function sum(a: number, b?: number): number {
	return a + b;
}

sum(1);	// NaN, js와 결과는 동일
sum(1, 2);	// 3
sum(1, 2, 3)	// Error 인자 초과
```



## 📝p164. 함수 오버로드

> 잘 몰랐던 부분

함수명은 같지만, 매개변수와 반환 타입이 다른 여러개의 함수를 선언 가능, 런타임 비용이 별도 추가되지 않음

가장 일반적인 타입을 가장 아래에 선언(ex. `any`가 가장 아래), 위일 수록 구체적 (순서 중요!)

```js
function add(a: string, b: string);
function add(a: number, b: number);
function add(a: any, b: any): any {
    return a + b;
}

console.log(add(1, 2));	// 3
console.log(add(`test1`, `test2`));	// test1test2
```



## 📝p170. 화살표 함수 유의점

```js
// bad case
const test = x => { x; };	// block{}을 사용할 경우, 무조건 return이 필요함!

// good case
const test = x => { return x; };
```



## 📝p171. filter, reduce

- [filter MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
- [reduce MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)



## 📝p173. 객체 리터럴 선언

> 개인적으로 객체 지향적으로 짤 때 좋은 코딩 스타일이라고 생각되는 문법

```js
let person = {
    name: `Taeuk`,
    hello: function (yourName) {
        console.log(`Hello ${yourName}, I'm ${this.name}`);
    }
}

person.hello(`minsu`);
```

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323081631307.png)

**주의사항**

`function`을 사용했기 때문에 `this.name`을 사용할 수 있었던 부분

만약 화살표 함수를 사용한다면 이렇게 뜰 것 이다. (현재는 this값이 window(최상위)로 잡은 케이스)

![이미지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323081744796.png)



### Typescript에서 `this` 타입 선언

> 잘 몰랐던 부분

```typescript
interface PersonType {
    name: string;
    hello(this: PersonType, yourName: string): string;
}
```



## 📝p177. `type` 명명법

>  `type`는 대문자로 시작, 책은 소문자로 시작되서 이상해서 레퍼런스를 찾아봄
>
> 대문자로 시작하는게 표준 케이스인 것 같음

```typescript
type calcType = (a: number, b: number) => number;
```

![공식 레퍼런스](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323110341117.png)

## 📝p179. jquery -> VanillaJS

> 음... 왜 굳이 예시를 Jquery로 들었는지 몰라서 작성

### Jquery

```js
$(`#myButton`).click(function() {
    alert(`버튼`);
})
```

### js

```js
document.querySelector(`#myButton`).addEventListener(`click`, () => {
   window.alert(`버튼`); 
});
```



## 📝 180. 콜백함수의 다른 예시(ex. Chrome API)

> Promise, async/ await 으로 바뀌는 추세이지만, 아직도 많은 부분에 callback 함수가 남아있다.

![image-20200323082731982](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200323082731982.png)