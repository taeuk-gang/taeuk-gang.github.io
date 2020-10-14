---
title: Quick Start Typescript ~ 10장 정리
toc: true
date: 2020-10-14 18:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 10장 고급 타입

##  📝342p. 타입 캐스팅과 타입 변환의 차이

명시적인 것의 차이

타입 캐스팅(type casting): 명시적으로 선언한 캐스팅 코드에 의한 타입 변경

타입 변환(type conversion): JS 인터프리터에 의한 타입 변경

```typescript
// Type casting
let a: number = 3;
let b: string;
b = String(a); // 명시적인 타입 캐스팅

// Type conversion
let a = 3;
let b = ``;

console.log(typeof b);	// string
b = a;	// 자동 타입 변환
console.log(typeof b);	// number
```

## 📝343p. 타입 캐스팅(type casting)과 타입 어설션(type assertion) 차이

컴파일 이후의 유지 여부

타입 캐스팅의 경우: 컴파일 이후, 코드 유지

타입 어설션의 경우: 컴파일 과정까지만 유효, 컴파일 이후 제거

## 📝344p. 타입 어셜선 선언 방법

```typescript
// 방법 1
let num4: number = <number>myNum;

// 방법 2
let num5: number = myNum as number;
```

방법1 (꺾쇠 방식)의 경우는 JSX 문법과 유사해 충돌 위험이 존재하여, `as`를 쓰는 것을 권장



## 📝346p. 프로그래밍 언어마다의 타입 검사 방식 2가지

1. 동적 타입 검사(dynamic type checking): Javascript의 duck typing - 런타임 시점
2. 정적 타입 검사(static type chcking): C++, Java - 컴파일 시점





타입 스크립트의 경우는 두개 모두 지원