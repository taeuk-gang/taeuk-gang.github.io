---
title: Quick Start Typescript ~ 5장 정리
toc: true
date: 2020-03-15 12:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성했습니다.
>
> Typescript 학습하는 목적으로 이해도가 높지 못해, 작성시 참고한 링크를 작성했습니다.



##  📝p134. 산술 연산자

ES7의 지수 연산자(`Math.pow`)를 지원, `**`를 이용

사용 예시

```typescript
console.log(10 ** 3);	// 1000
```



## 📝p136. 비교 연산자

> 다른 언어와 다르게, 자바스크립트는 `===` 등호 3개가 존재, `==`와 다른 점은 `==`은 값만 비교한다면 `===`은 값과 타입을 비교하는 것이 다름

```typescript
'1' == 1;	// true
'1' === 1;	// false
```

위 사항은 자바스크립트 또한 마찬가지지만, 타입스크립트는 같은 타입과의 비교만 지원한다.



## 📝p137. 논리 연산자

> 논리 연산자는 주로 변수 선언과 함께 함수 초기값 선언에 많이 사용된다.

사용 예시

```typescript
// 이런식으로 기본값이 없으면, default를 만든다. 라는 것으로 선언 가능
const value: string = value || 'default';

// 부정 연산자는 depth를 깊게하지 않기 위한, if문에 많이 사용됨
if (!isLogin) {
	console.error(`no login!`);
    location.href = `/login`;		// login 라우팅으로 이동
}
```



## 📝p139 디스트럭처링

### 종류

1. 객체 디스트럭처링
2. 배열 디스트럭처링



### 1. 객체 디스트럭처링

```typescript
let { id, country = 88 } = { id: `happy` };

console.log(id);	// happy
console.log(country);	// 88
```

#### rename

```typescript
let { id: newName1, country: newName2 } = { id: `happy`, country: 88 };
console.log(newName1);	// happy
console.log(newName2);	// 88

console.log(id, country);	// error Not defined
```



#### 함수 파라미터 초기값 설정

```js
test({ name: `happy` });	// happy, none 출력

function test({ name, country = `none` }) {
    console.log(name, country);
}
```



#### type 키워드 활용

```typescript
type C = { a: string, b?: number };

fruit({ a: `apple`, b: 10 });	// apple 10
fruit({ a: `apple`});	// apple undefined

function fruit({ a, b }: C) {
    console.log(a, b);
}
```



### 2. 배열 디스트럭처링

```js
let numbers = [`one`, `two`, `three`, `four`, `five`];

// 이전 방식
let num1 = numbers[0];	// one
let num2 = numbers[1];	// two


// 이후 방식
let [num1, num2] = numbers;	// one, two

// 결과 동일
console.log(num1, num2);	// one, two

// 중간 빼오기
let [, , num3, num4, ] = numbers;
console.log(num3, num4);	// three, four

// 교체
[num4, num3] = [num3, num4];
console.log(num3, num4);	// four, three

// 초기값 지정
let [color1, color2 = `blue`] = [`black`];
console.log(color1, color2);	// black blue
```



## 📝p146. 전개 연산자

### 배열

```js
let [first, ...second] = [1, 2, 3];

console.log(first, second);	// 1  2, 3
```



### 객체

```js
let numGroup = { n1: 1, n2: 2, n3: 3};
let { n2, ...n13 } = numGroup;
console.log(n2, n13);	// 2    { n1: 1, n3: 3};
```

