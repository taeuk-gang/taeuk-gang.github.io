---
title: Quick Start Typescript ~ 11장 정리
toc: true
date: 2020-10-18 00:00:00
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 11장 제네릭

##  📝361p 제네릭 소개

제네릭(generics)은 클래스와 함수에 **타입이 고정되는 것을 방지**하고 **재사용할 수 있는 요소를 선언**할 수 있게 하는 것

기존 C#, JAVA에서 제공되었던 기능, 타입스크립트는 0.9부터 지원

### 장점

1. 컴파일 시간에 진행해 타입 안정성을 보장
2. 타입 캐스팅과 관련된 코드를 제거 가능
3. 재사용이 가능한 코드 작성 가능

### 사용법

```typescript
function arrayConcat<T>(array1: T[], array2: T[]): T[] {
  return [...array1, ...array2];
}

const array1 = [1,2,3];
const array2 = [4,5,6];

const resultConcat = arrayConcat<number>(array1, array2);
```

`<T>`  가상의 타입(= 타입 매개변수(`type parameter`) = 제네릭 타입 변수(`generic type variables`))으로 임의의 단어를 사용해도 된다.



### 예제. 타입 캐스팅과 관련 코드 제거

> 음... 결국 문자열만 받아서, 캐스팅 코드를 없애거나, 숫자를 받아서 캐스팅 코드를 이후 처리하는거나 조삼모사 아닌가?
>
> 그럼 본질적으로 타입 캐스팅과 관련된 코드를 제거할 수 없는 것이 아닌가?

이전

```javascript
function concat(str1, str2) {
  return String(str) + String(str2);
}

concat(`abc`, 123);
```

이후

```typescript
function concat<T>(str1: T, str2: T): T {
  return str1 + str2;
}

concat<string>(`abc`, String(123));	// 음 결국? 캐스팅 코드가 필요해져버림
```



## 📝366p. 타입 매개변수 T를 특정 타입으로 제한해야할 경우

```typescript
// <T extends string>

function concat<T extends string>(str1: T, str2: T): string {
  return str1 + str2;
}
```



## 📝367p. 오버로드시의 제네릭

```typescript
function concat<T>(str1: T, str2: T): T;
function concat(str1: any, str2: any) {
  return str1 + str2;
}
```



## 📝369p. 타입 매개변수 2개 이상 선언법

```typescript
let mapArray = [];
function put<T, T2>(str1: T, str2: T2): T;
function put(idx: any, str: any) {
  mapArray[idx] = str;
}

function get<T, T2>(idx: T): T2;
function get(idx: any) {
  return mapArray[idx];
}

put<number, string>(1, `hello`);
console.log(get<number, string>(1));
```



## 📝371p. Generic class

```typescript
class 클래스명<T> {
  메소드(param: Array<T>, param2: number): T {
    return param[param2];
  }
}

const 인스턴스명 = new 클래스명<number>(파라미터);
```

Example

```typescript
interface IName {
  name: string;
}

class Profile implements IName {
  name: string = `taeuk`;
}

class Accessor<T extends IName> {
  getKey(obj: T) {
    return obj.name;
  }
}

let acc = new Accessor();
console.log(acc.getKey(new Profile()));	// LOG: taeuk
```



## 📝377p. 룩업 타입(lookup)

`keyof` 키워드로 키값을 탐색하여, 유니언 타입처럼 작동시킴

아래의 예제를 보면, 인터페이스의 키값만을 허용하는 유니언 방식처럼 작동됨을 알 수 있음

```typescript
interface INumber {
  one: number;
  two: number;
  three: number;
}

type NumberKeys = keyof INumber;	// one, two, three, 인터페이스 키값만을 허용
```

### 활용 예제

```typescript
function getValue<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}
let numbersKeys = {
  one: 1,
  two: 2,
  three: 3,
};

console.log(getValue(numbersKeys, `one`));	// LOG: 1
```



## 📝379p. 인터페이스 상속과 동시에 제네릭 확장

```typescript
interface IFilter<T> {
  unique(array: Array<T>): Array<T>;
}

class Filter<T> implements IFilter<T> {
  unique(array: Array<T>): Array<T> {
    return array.filter((v, i, array) => array.indexOf(v) === i);
  }
}

let myFilter = new Filter<string>();
let resultFilter = myFilter.unique([`a`, `b`, `c`, `a`, `b`]);
console.log(resultFilter);	// LOG: [`a`, `b`, `c`]
```



## 📝381p. 맵 객체 소개

생략

## 📝384p. 제네릭 기반의 자료구조 작성

스택, 큐, ArrayList같은 자료구조는 내장 객체로 지원되지 않아서 직접 구현을 필요로 하는데, 이 때 제네릭을 사용하는 것이 좋음

이 중 책에서는 ArrayList를 구현하는 것을 설명

자료구조를 학습하는 것이 목적이 아니기 때문에 생략

## :mag: 참고자료

- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/class

- https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-6.html

- https://poiemaweb.com/typescript-class