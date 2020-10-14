---
title: Quick Start Typescript ~ 10장 정리
toc: true
date: 2020-10-14 20:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 10장 타입 선언과 변경, 타입 호환

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



## 📝346p. 프로그래밍 언어마다의 타입 검사 방식과 타입스크립트의 타이핑 방식

1. 동적 타입 검사(dynamic type checking): Javascript의 duck typing - 런타임 시점
2. 정적 타입 검사(static type chcking): C++, Java - 컴파일 시점

타입 스크립트의 경우는 두개 모두 지원

### 타입스크립트의 4가지 타입 지정 방식

> 아래와 같은 방법의 이름은 몰라도 개발하면서 자연스럽게 사용해야한다.

1. 덕 타이핑(Duck typing)
2. 구조 타이핑(structural typing)
3. 구조 서브타이핑(structural subtyping)
4. 명목 타이핑(nominal typing)



### 덕 타이핑

```typescript
interface DuckGoose {
  speak();
  swim();
}

class Duck {
  speak() {
    console.log(`꽥`);
  }
  swim() {
    console.log(`수영 중...`);
  }
}

class Goose {
  speak() {
    console.log(`구우`);
  }
  swim() {
    console.log(`거위 수영 중...`);
  }
}

function swim(obj: DuckGoose) {
  obj.speak();
  obj.swim();
}

let duck = new Duck();
let goose = new Goose();

swim(duck);
swim(goose);
```

자바스크립트 런타임시 동적으로 타이핑이 이뤄지는 타입 지정 방식

같은 메서드를 호출하는 것을 볼 수 있음, 선언되지 않은 메소드의 경우 에러 발생



### 구조 타이핑

#### 1. 구조가 같은 클래스의 경우

```typescript
class Animal {
  name: number;
  constructor(name: string, weight: number) {}
}

class Bird {
  name: number;
  constructor(speed: number) {}
}

let animal: Animal = new Animal(`happy`, 100);
let bird: Bird = new Bird(10);

// 타입 호환 가능
animal = bird;
bird = animal;
```

타입스크립트 컴파일 시간에 타입 호환이 가능한지를 검사

클래스의 멤버 변수가 같으므로 서로 타입 호환이 가능

생성자 매개변수는 상관없음, 접근 제한자가 설정되있지 않기 때문에 생성자 내부에서만 사용할 수 있기 때문
(생성자 매개변수 기본값 `private`)

#### 2. 상속 관계를 고려한 구조가 같은 클래스의 경우

```typescript
class Person {
  public name: string;
}

class Member extends Person {
  public grade: number;
}

class Admin extends Member {}

class MemberCard {
  public name: string;
  public grade: number;
}

let admin: Admin = new Admin();
admin = new MemberCard(); // 타입 호환이 가능
```

admin과 MemberCard는 아무런 관계가 없지만, 완전히 동일하지는 않지만 같은 구조의 멤버 변수를 소유하고 있으나 타입 호환이 가능



#### 3. 구조가 같은 클래스와 인터페이스 간의 구조 타이핑

클래스와 인터페이스의 구조 같으면 타입 호환이 가능

```typescript
interface Person {
  name: string;
}

class Employee {
  name: string;
}

let person: Person;
person = new Employee();	// 타입 호환이 가능
```



### 구조 서브타이핑

타입 구조가 같아야지만 타입 호환이 이뤄지지만, 구조 서브타이핑은 구조가 부분적으로 같더라도 타입 호환을 지원

#### 구조 서브타이핑의 조건

하위 타입이 상위 타입으로만 호환

```typescript
// 상위 타입
interface TypeA {
  a: string;
  b: string;
}

// 하위 타입
interface TypeB {
  a: string;
  b: string;
  c: string;
}
```



#### 1. 타입이 없지만 구조가 일부 같은 변수 간의 구조 서브타이핑

```typescript
let infoUpper = {
  name: "taeuk",
  country: "korea"
};

let infoSub = {
  name: "taeuk",
  country: "korea",
  status: "happy"
}

infoUpper = infoSub;
```



#### 2. 매개변수 개수가 다른 함수 타입간의 구조 서브타이핑

```typescript
let funcUpper = (a: string) => a;
let funcSub = (a: string, b: string): string => a + b;

funcSub = funcUpper;	// 하위 타입 = 상위 타입

console.log(funcSub(`hello`, `world`));
```



#### 3. 구조가 일부 같은 객체와 인터페이스 간의 구조 서브타이핑

```typescript
interface GroupUpper {
	name: string;	
}

let groupSub = {
  name: `Typescrript Group`,
  id: 1,
}

let groupUpper: GroupUpper;
groupUpper = groupSub;
```



### 명목 타이핑

명시적으로 지정된 타입 간에만 타입이 호환

```typescript
enum EastAsia1 {
  korea = 88,
  china = 86,
  japan = 81,
}

enum EastAsia2 {
  korea = 88,
  china = 86,
  japan = 81,
}

let east1: EastAsia1 = EastAsia1.china;
let ease2: EastAsia2 = EastAsia2.korea;

// 구조적으로는 같지만, 아래의 코드는 작동하지 않음
// let east1: EastAsia1 = EastAsia2.china;
// let east2: EastAsia2 = EastAsia1.korea;

east1 = 1000;
east2 = 2000;
```

 