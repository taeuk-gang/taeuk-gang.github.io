---
title: Quick Start Typescript ~ 9장 정리
toc: true
date: 2020-04-12 18:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 9장 고급 타입

##  📝315p. 유니언 타입

```typescript
const x: string | number;
```

## 📝317p. 타입 가드

### typeof

```typescript
function myIndexOf(x: number | string, y: string) {
    if (typeof x === `string`) {
        return x.indexOf(y);
    }
    
    return -1;
}

console.log(myIndexOf(`hello`, `e`))
```

### instanceof

```typescript
function diff(x: Cat | Dog) {
    if (x instanceof Dog) {
       ... 
       return;
    }
    
    return;
}
```



## 📝320p. 고급 타입들

### 문자열 리터럴 타입

```typescript
let event; 'keyup' = 'keyup';	// O
let event: 'keyup' = 'keyup2';	// error

// or

type EventType = 'keyup' | 'mouseover';
const myEvent: EventType = 'keyup';
```



### 룩업 타입 (= 인덱스 타입)

`keyof` 명령어를 통해 타입 T의 하위 타입을 생성, 타입 T는 여러 타입으로 이뤄진 유니언이나 인터페이스 타입을 의미



확장성을 고려해 `interface`를 도입

```typescript
interface Profile {
    name: string;
    gender: string;
    age: number;
}
```

이 인터페이스를 `keyof`를 활용하여 룩업 타입으로 선언

```typescript
// # 1
// 이렇게 선언된 변수는 name , gener, age 중 하나를 할당 받기 가능
type Profile1 = keyof Profile;

let pValue: Profile1 = 'name';

// # 2
// 배열 타입의 내장 속성인, length, push, pop, concat 등을 할당받아 사용 가능
type Profile2 = keyof Profile[];

let pValue2: Profile2 = 'length';
let pValue3: Profile2 = 'push';

// # 3
// ??? 이해 안되는 부분
// 어느 문자열이든 입력 가능한건가?
type Profile3: keyof { [x: string]: Profile };

let pValue4: Profile3 = `hello`;

// # 4
// name의 string 타입을 전달, 타입이 string일 때 접근 가능한 내장 속성 이용 가능
type Profile4 = keyof Profile[`name`];

let pValue5: Profile4 = `length`;
let pValue6: Profile4 = `abcd`;	// error
```



### non-nullable 타입

타입스크립트 2.0 이전에는 `null` 이나 `undefined`는 모든 타입의 변수에 할당할 수 있었음

그러나, `tsconfig.json`에 `strictNullCheck`을 true로 바꾸면, `null`과 `undefined`가 자동으로 모든 타입의 할당되지 않고 별도로 타입으올 관리해줘야함.



### never 타입

`never`는 모든 타입의 하위 타입으로 사용할 수 있지만, `any`만 할당될 수 없다.

**사용용도**

1. 함수에 닿을 수 없는 코드 영역이 있어 반환값이 존재하지 않을 때
2. 함수에 `throw`객체가 반환되어, 오류가 발생할 때



```typescript
const neverFunc = (): never => {
    while (true) {
        
    }
    // console.log(); <- 닿을 수 없음
}
let resultNever: never = neverFunc();
```

```typescript
function error(message: string): never {
    throw new Error(message);
}
```



### this 타입

`this` 타입을 다형적 this 타입이라고도 함, 선언 위치에 따라 참조하는 대상이 달라지기 때문

#### 인터페이스에  `this` 사용 예시

```typescript
interface ListItem {
    getHead(): this;
    getTail(): this;
}
```

#### 플루언트 인터페이스 패턴 (플루언트 패턴)

그냥 자기자신 반환해서 체이닝하는 패턴

> 개인적으로 이런 형식으로 많이 사용했었음

```typescript
const Mycalc = () => {
    return {
        val: 0,
        plus(num) {
            this.val += num;
            return this;
        },
        minus(num) {
            this.val -= num;
            return this;
        }
    }
}

Mycalc().plus(3).minus(2).val	// 1
```

