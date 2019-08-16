---
title: Book-tell-javascript
toc: true
date: 2019-08-16 15:40:19
tags:
categories:
---

# 자바스크립트를 말하다 (1) 복습

Speaking Javascript 책을 읽으며, 특별히 눈에 들어왔던 내용만 기록



## Statement(문)과 Expression(표현식)

```js
var foo;
```

문은 '동작'을 의미하며, 프로그램은 기본적으로 문의 연속입니다.

```js
// 표현식
var x = y >= 0 ? y : -y;

// 이런식으로도 가능
func(y >= 0 ? y : -y)

// 이런 것도 표현식
foo(7, 1);
```

표현식은 값을 생성하며, 함수 매개변수와 할당문의 오른편을 말합니다.



## 원시값

다음은 모두 원시 값*입니다.

- 불리언: true, false
- 숫자: 1736, 1.351
- 문자열: 'abc'
- 값 아닌 값: undefined, null



### 특징1: 항상 불변

프로퍼티를 변경, 추가, 제거 할 수 없음

```js
let str = 'abc';
str.length = 1;

console.log(str.length); // 3 <= 효과없음
str.foo = 3;
console.log(str.foo); // undenfined <= 마찬가지
```



## 객체 = 원시값이 아닌 것



### 종류1: 객체 리터럴

```js
{
    firstName: `taeuk`,
    lastName: `Kang`
}
```



### 종류2: 배열 리터럴

```js
[1,2,3]
```



### 종류3: 정규표현식 리터럴

```js
/^a+b+$/
```



### 특징1: 유일성을 비교하며, 값이 같더라도 엄연히 다르다

참조 비교시

```js
const empty1 = {};
const empty2 = {};
console.log(empty1 === empty2); // false

// 얇은 복사는 참조만 가져오기에 같다고 함
const obj1 = {};
const obj2 = obj1;
console.log(obj1 === obj2); // true
```



### 특징2: 변경이 가능 (원시값과 차별점)

```js
const obj = {};
obj.foo = 123;
console.log(obj.foo); // 123
```





## undefined와 null

정보가 없음을 의미하는 두 단위

개인적으로 생각하는 차이점은 `undefined`는 선언도 되지 않았다. `null`은 선언되었지만 값이 존재하지 않는다. 로 생각한다.



`null`은 객체가 아니다라는 뜻



`undefined`, `null` 둘다 프로퍼티와 표준 메서드가 존재하지 않음

#### 체크법

```js
if (x === undefined || x === null) { // undefined == null은 true지만, ===은 false이다.
    ...
}
    
// or
    
if (!x) {
    ...
}
```



## `typeof`와 `instanceof` 로 값 분류

```js
typeof value // boolean or string, function, object(ex. [], {})
```

참고로, `typeof null`은 object로 반환된다. 자바스크립트 자체 버그잼

```js
value instanceof Constructor

const b = new Bar();
console.log(b instanceof Bar); // true

{} instanceof Object // true
[] instanceof Array // true
[] instanceof Object // true, Array이는 Object의 부속 생성자

undefined instanceof Object // false
null instanceof Object // false, typeof와 다른 점
```



## 숫자

```js
1 === 1.0 // true
```

### NaN = Not a Number

에러값

```js
Number(`xyz`) // NaN
```

### Infinity

에러로 생긴 값

```js
3 / 0 // infinity

Math.pow(2, 1024); // 너무 큰 숫자
```



## 연산자

조금 신기한 함수 연산자

```js
+a(); // 함수는 문자열을 반환하고, 연산자는 이를 숫자!로 변환
++a(); // Error!
```



## 끌어올림 = hoisting

호이스팅을 한글화하면 끌어올림이라고 하는구나, 처음 앎



## 자릿수 강제

```js
function pair(x, y) {
    if (arguments.length !== 2) {
        throw new Error(`Need exactly 2 arguments`);
    }
}
```

그런데 `arguments`를 이용한 코딩을 잘 안하는걸로 알고 있는데 좀 알아봐야겠군...



## `Arguments`를 배열로 변환

`arguments`는 배열이 아님, 조금 특별한 객체일 뿐. 하지만 `length`가 붙어있음.
그러나, 인덱스를 써서 요소에 접근은 가능하지만, 제거하거나 배열 메서드를 호출할 수는 없다.

그러므로 가끔 배열로 써야할 경우 다음 방법을 사용

```js
function toArray(arrayLikeObject) {
    return Array.prototype.slice.call(arrayLikeObject);
}
```



## IIFE 패턴: 새 스코프

`IIFE`가 뭔지 몰랐었는데 즉시 호출 함수식을 말하는거 였음. 이피 잼

```js
let result = [];
for (var i = 0; i < 5; i++) {
    result.push(() => i);
}

console.log(result[1]()); // 5, 1이 아님
console.log(result[3]()); // 5, 3이 아님

// IIFE 식을 쓰면, 그당시 i 값을 가지는게 가능

for (var i=0; i < 5; i++) {
    (function() {
        var i2 = i;
        result.push(() => i2);
    })();
}

// 사실 let을 쓰면 끝남

let result = [];
for (let i = 0; i < 5; i++) {
    result.push(() => i);
}

console.log(result[1]()); // 5
console.log(result[3]()); // 5
```



## `in` 연산자

```js
const taeuk = {
    name: `taeuk`,
    tell() {
        return `i am ${this.name}`
    }
}

tell in taeuk // true
```



## 추출 메서드

주의점: 메서드를 추출하면 객체와의 연결이 사라짐, 함수로 변경되는 것
그러므로, `this`도 `undefined`로 변경됨

```js
const taeuk = {
    name: `taeuk`,
    tell() {
        return `i am ${this.name}`
    }
}

const func = taeuk.tell;
func() // Error!

// Binding을 하지 않아서 그럼

const func = taeuk.tell.bind(taeuk);
func() // i am taeuk 
```



## forEach의 두번째 문구가 `this` 바꾸는거였음

```js
arr.forEach(callback[, thisArg]);
```

### 매개변수

각 요소에 대해 실행할 함수. 다음 세 가지 인수를 받습니다.

- `currentValue`

  처리할 현재 요소.

- `index` Optional

  처리할 현재 요소의 인덱스.

- `array` Optional

  forEach()를 호출한 배열.

`thisArg` Optional

`callback`을 실행할 때 `this`로 사용할 값.



## 정규표현식

```js
/^abc$/
/[A-Za-z0-9]+/
```



### test() 메서드: 일치하는 것이 있는지 확인

```js
/^a+b+$/.test(`aaab`) // true

/^a+b+$/.test(`aaa`) // false
```



### exec() 메서드: 일치하는 그룹을 캡처

```js
/a(b+)a/.exec(`_abbba_aba_`) // [`abbba`, `bbb`]
```

반환된 배열의 인덱스 0은 일치하는 그룹 전체!, 1 이후부터는 캡처한 그룹!

이 메서드를 반복해 일치하는 그룹을 모두 캡처하는 방법도 존재 (추후에 알아볼 듯)



## replace() 메서드: 검색과 교체

```js
` `.replace(/<(.*?)>/g, '[$1]') // [a] [bbb]
```

으음 위 코드 정상 작동 안하는거 같은데... `replace` MDN 참조해봐야 알 듯