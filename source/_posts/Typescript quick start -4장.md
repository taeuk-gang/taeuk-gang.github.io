---
title: Quick Start Typescript ~ 4장 정리
toc: true
date: 2020-03-08 12:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> :book: Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성했습니다.
>
> Typescript 학습하는 목적으로 이해도가 높지 못해, 작성시 참고한 링크를 작성했습니다.



##  📝p39. `package.json`

프론트엔드에서 가장 먼저 살펴보는 파일

1. `package.json`

2. 번들러: `webpack.config.js` or `vue.config.js` 

   - JS 로더: `tsconfig.json`, `babel.config.js` or `.babelrc`

   - CSS 로더: `postcss.config.js`

3. `index.html` - JS/CSS/Font 삽입 보기

4. 그외 Lint(코딩 컨벤션): `eslintrc.js`



### `package.json` 작성
> 개인적으로 중요하다고 생각되는 부분이나 몰랐던 부분만 정리


[이 사이트 (공식)](https://docs.npmjs.com/files/package.json)에서 자세히 설명되어있다. 

+[한글 사이트](https://programmingsummaries.tistory.com/385)

#### name

주의사항: name에는 대문자를 포함해서는 안된다.

#### version

버전 관리는 아래와 같은 [Rule](https://docs.npmjs.com/about-semantic-versioning)이 존재

> 모르고 있던 부분

![Semantic Versioning](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200308145529955.png)

#### scripts

> 개인적으로 가장 `package.j0son` 파일을 열었을 때, 가장 먼저 보는 항목
>
> **참고링크**
> [npm-scripts](https://docs.npmjs.com/misc/scripts)
> [npm-run-scripts](https://docs.npmjs.com/cli/run-script)

개발자가 설정해둔 커맨드 라인 명령어를 alias처럼 `npm run <key값>`으로 사용할 수 있다.

```json
// package.json

"scripts": {
    "test:make": "mkdir make-test-folder"
}
```

```bash
npm run test:make

# mkdir make-test-folder 명령어가 실행되고, 현재 경로에 make-test-folder 디렉토리가 생긴다.
```

#### dependencies

실제 배포될 때 포함되는 패키지들

`npm install` or `npm install --save` 로 저장된 패키지들

#### devDependencies

개발용으로 필요한 패키지들 (lint, test, bundle etc)

`npm install -D` or `npm install --save-dev`로 저장된 패키지들





##  📝p40. npm 주요 명령어

npm 주요 명령어를 짧게 칠 수 있다. (기본 aliases)

자주 사용하는 명령어라서 짧게 치면 편하다.

```bash
# 이전
npm install <패키지명>
# 축약
npm i <패키지명>

# 여러 축약

## 글로벌 설치
npm i -g <패키지명>

## devDependency 설치
npm i -D <패키지명>

## 삭제
npm rm <패키지명>
```

[그 외 명령어들](https://docs.npmjs.com/cli-documentation/)



##  📝p48. `tsconfig.json` 설정

타입스크립트 컴파일 옵션 정의된 파일

> [공식 홈페이지](https://www.typescriptlang.org/tsconfig)
>
> [참고: 간단히 정리된 블로그](https://vomvoru.github.io/blog/tsconfig-compiler-options-kr/)

`removeComments`의 경우 주석을 제거하여 컴파일 해주지만, 주석을 이용하여 webpack에서 번들링하는 경우가 존재하는 점을 유의

ex. /router/index.ts

```typescript
const DashboardView = () => import(/* webpackChunkName: "dashboard" */ '@/views/admin/dashboard/DashBoardView.vue');
```

위와 같이 URL 라우팅별로, code split하여 특정 JS만 불러오게 하는 기능을 주석으로 처리할 수도 있다.

*해당 라우팅으로 들어갔을 때, `dashboard.js` 파일만 불러오게하는 기능이다.



##  📝p76. 변수 선언

> 기존 JS(ES5) `var` 과 ES6에서 생긴 `let`, `const` 차이점이 중요
>
> 현재는 `var`은 잘 사용하지않는 편, 실제로 ESLint에서도 [no-var](https://eslint.org/docs/rules/no-var) option을 사용하여 사용 못하게 설정됨

### `var` vs `let`, `const` 차이점

1. 호이스팅
2. 함수 레벨 스코프(var) vs 블록 레벨 스코프(const, let)

#### 호이스팅 차이

선언이 유효범위에서 최상단으로 이동하는 것

JS에서 대표적으로 호이스팅(끌어올림)으로 되는 것이 `function`과 `var`이 있다. 안되는 것으로는 `class`, `const`, `let`이 있다.

##### var의 경우

```js
// 개발 작성 코드
var test = `global`;	// 이렇게 할당된 변수는 window.test 객체로 할당된다.
console.log(window.test);	// `global`

func();
function func() {
	console.log(test);	// (A) 여기 표시값?
	var test = `local`;
	console.log(test);	// (B) 여기 표시값? 
}
```

```js
// 실제 실행되는 코드
var test; 
test = `global`;

console.log(window.test);	// `global`

function func() {	// 함수도 호이스팅 되는 것을 알 수 있다.
	var test;
	console.log(test);	// (A) undefined
	test = `local`;
	console.log(test);	// (B) `local` 
}

func();
```

(A) 부분에서 func내에서 지역변수로 `test` 변수가 호이스팅 했기 때문에 글로벌 변수 `test`를 가져오지 못하는 것을 알 수 있다.



##### const, let의 경우

```js
const testConst = `global-const`;
let testLet = `global-let`;
console.log(window.testConst);	// undefined const
console.log(window.testLet);	// undefined let	var과 다르게 window 객체에 할당되지 않는다.

// func();	// Error, 아직 선언되지 않음

const func = () => {
// 	console.log(testConst);	// Error, 아직 선언되지 않음
// 	console.log(testLet);	// Error, 아직 선언되지 않음
	const testConst = `local-const`;
	let testLet = `local-let`;
	console.log(testConst);	// `local-const`
	console.log(testLet);	// `local-let`
}

func();
```



#### 함수 레벨 스코프(var) vs 블록 레벨 스코프(const, let)

##### var의 경우

```js
func();

function func() {	// 변수 범위 = 함수 안
    if (true) {
        var test = `any`;
    }
    console.log(test);	// `any`
}
```

##### const, let의 경우

```js
func();

function func() {
    if (true) {	// 변수 범위 = {} 블록 안
        const testConst = `const`;
        let testLet = `let`;
    }
    console.log(testConst);	// Error, not defined
    console.log(testLet);	// Error, not defined
}
```



##  📝p86. 타입 계층도

![출처: Quick start Typescript p.86](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200308220210471.png)

> [참고하면 좋은 링크](https://www.typescriptlang.org/docs/handbook/basic-types.html)
>
> 타입 체크가 Javascript에서 없던 부분이라 아직 많이 부족한 부분

### any

가장 상위에 있는 타입



---------

#### primitive type

1. string, number, boolean
2. **symbol**
3. **enum**
4. **문자열 리터럴**

#### Object

1. Array
2. **Tuple**
3. Functopn
4. 생성자 <- ?
5. Class
6. **Interface**

#### Union

2개 이상의 타입 하나의 타입으로 지정

```typescript
const x: string | number;
```

#### intersection

두 타입을 합쳐 하나의 타입으로 만듦

```typescript
interface Dog {
    leg: number;
}
interface Bird {
    wing: number;
}
let dogBird: Dog & Brid = {
    leg: number;
    wing: number;
}
```



-------

#### Primitive type

##### (생략) string, number, boolean 

##### symbol

유일하고 불변적인 식별자 ([자세한 사항](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Symbol))

ES6 이상에서부터 지원

```typescript
let test = Symbol(`test`);
let test2 = Symbol(`test`);

console.log(test === test2);	// false
```

사용처

```typescript
const RED = Symbol();
const YELLOW = Symbol();
const ORANGE = Symbol();

// 그 자체로 식별자 역할을 함
```

> 실제 개발상에서 사용해본 적이 없어서 아직 잘 모르겠다.
>
> [사용 사례에 대한 링크](https://perfectacle.github.io/2017/04/16/ES6-Symbol/)



##### type

특정 문자열만 허용하는 타입

```typescript
type EventType = "keyup" | "mouseover";
```



##### enum

```typescript
enum Color { Red = 1, Green, Blue };
let color: Color.Green;
console.log(color); // 2
```

> Object와의 차이점? ([여기참고](https://medium.com/@seungha_kim_IT/typescript-enum%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-3b3ccd8e5552))
>
> 1. `object`는 속성 자유롭게 변경 가능, `enum`은 그렇지 않음
> 2. `enum`은 항상 리터럴 타입 사용
> 3. `enum`의 속성값으로는 문자열 또는 숫자만 허용됨

사용 이유

```typescript
const korean = `ko`
const english = `en`

// 코드 중복
type Language = `ko` | `en`

// 코드기 길어짐
type Language = typeof korean | typeof english

const code: Language = korean

// enum 사용시 가독성이 증가함
enum Language {
    korean = `ko`,
    english = `en`
}

const code: Language = Language.korean
```

--------

#### Object Type

##### Array

###### array type

```typescript
let array; string[] = [`a`, `b`, `c`];
```

###### generic array type <>

```typescript
let array: Array<string> = [`a`, `b`, `c`];
```

차이점: primitive 타입 외에 object 타입도 받을 수 있다.

```typescript
let array: Array<() => string> = [() => `a`, () => `b`, () => `c`];
```

##### Tuple

n개에 대한 배열 타입

```typescript
let array: [string, number] = [`text`, 10];

array = [1, `t`];	// error - 각 index에 대한 타입이 안맞음
array = [`t`, 10, `e`, 1];	// error - index를 초과하여 받음
```

> 이번 장에서 다루지 않는 것들 생략

##### (생략) Function

##### (생략) 생성자

##### (생략) Class

##### (생략) Interface

 

## 📝p114 `undefined` !== `null`

`undefined`는 선언은 됬지만, 값이 할당되지 않은 상태

`null`은 선언과 값이 없다고 할당된 상태

```js
undefined === null; // false
undefined == null; // true
```

`===` 비교 연산자는 type까지 값은지 체크해주지만, `==`는 값만 체크

ex.

```js
1 == `1`;	// true
1 === `1`;	// false
```



## 📝p123 for ... in 문 주의사항

> 이전에 이슈 걸렸던 사항으로 작성

`for ... in` 문은 없는 인덱스는 출력하지 않는다.

ex.

```js
for (i in [1,,,4]) {
    console.log(i);
}
```

![for...in문](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200309004858167.png)

`for ... of`문은 출력

![for...of문](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200309004950037.png)