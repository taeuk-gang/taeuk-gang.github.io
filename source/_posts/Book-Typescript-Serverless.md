---
title: 타입스크립트, AWS 서버리스로 들어올리다.
toc: true
date: 2020-01-21 14:54:56
tags:
    - Book
    - Javascript
    - Typescript
    - Vue.js
categories:
    - Book
---

> 책 읽고 도움될만한 것 정리

## 타입스크립트 Bash 명령어

```bash
# 설치
npm i -g typescript
# 컴파일
tsc helloworld.ts
# ts 컴파일 없이, 바로 실행 패키지 설치(ts-node)
npm i -g ts-node
# ts-node 실행
ts-node helloworld.ts
```



------



### 타입스크립트 Type

Boolean, Number, String, Array, [Tuple], Enum, Any, Void, Never, (Object)

#### Boolean

```typescript
let isRun: boolean = false;
```

#### Number

```typescript
let decimal: number = 5;
let hex: number = 0xff;
```

#### String

```typescript
let fullName: string = `Kang Taeuk`;
let age: number = 11;
let profile = `Full Name: ${fullName}, Age: ${age}`
```

#### Array

```typescript
let list: Number[] = [1,2,3];
# or
let list: Array<number> = [1,2,3];
```

#### Tuple = 다양한, 섞여있는

```typescript
let point: [string, number];
point = [`x`, 10];
point = [10, `x`]; // Error! 순서 상관있음
```

#### Enum

```typescript
enum Color { Red = 1, Green, Blue };
let color: Color.Green;
console.log(color); // 2
```

`Enum`은 C++, C#에서 많이 사용되는 개념

#### Any

```typescript
let sure: any = 1;
sure = `String`;
sure = true;
```

#### Void

```typescript
function log(msg): void {
    console.log(`Log: ${msg}`);
}
```

#### Null, Undefined

```typescript
let a: number = null;
```

다른 모든 타입의 하위타입으로 지정될 수 있음 (모든 타입은 null과 undefined가 될 수 있음)

> 타입스크립트 설정 `--stictNullChecks`이 켜져있다면, null이 지정되면 오류가 발생한다.

#### Never

절대 발생하지는 값의 유형. Ex. 절대 리턴이 발생하지 않는 경우, 항상 예외값 등

```typescript
function error(message: string): never {
	throw new Error(message);
}

function forever(): never {
    while (true) {
        
    }
}
```

#### Object

```typescript
let user: { name: string; age: number; } = { name: `taeuk`, age: 12};
console.log(user.name)
```

### Type alias

이미 존재하는 타입에 다른 이름을 붙여 사용할 수 있게하는 기능이 존재

```typescript
type UNIQID = string | null
function getUserID(id: UNIQID) {
    console.log(id)
}

getUserID(`id-abcd`)
getUserID(null)
getUserID(12) // error!

// 특정값만 받게도 가능
type USER_TYPE = `TESTER` | `ADMIN`
let userType: USER_TYPE = `TESTER`
userType = `asdf` // error!
```



--------



### Function

```typescript
function sum(x: number = 10, y: number = 5, z?: number): number {
    return x + y;
}

function cites(name: string, ...rest:string[]) {
    return name + `,${rest.join(`,`)}`;
}

let ourCites = cites(`seoul`, `busan`, `deagu`);
console.log(ourCites);
```



-------



### Interface

컴파일할 때 타입을 체크하고 삭제됨

```typescript
interface Size {
    width: number;
    height: number;
    version?: string;
}

interface Label {
    title: string;
    size: Size;
}

function labelPrint(label: Label): void {
    console.log(label);
}

let myLabel = <Label>{
    title: `Typescript Book`,
    size: {
        width: 20,
        height: 30
    }
}
labelPrint(myLabel);
```



-------



### Class



### Getter/ Setter



### Async