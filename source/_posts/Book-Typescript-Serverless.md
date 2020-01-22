---
title: Typescript 정리
toc: true
date: 2020-01-21 14:54:56
tags:
    - Book
    - Javascript
    - Typescript
    - Vue.js
categories:
    - Typescript
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
// or
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

[여기 자세하게 설명되어있어 좋았다](https://medium.com/@seungha_kim_IT/typescript-enum%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-3b3ccd8e5552)

>  `Object`와의 차이점
>
> 1. `object`는 속성 자유롭게 변경 가능, `enum`은 그렇지 않음
> 2. `enum`은 항상 리터럴 타입 사용
> 3. `enum`의 속성값으로는 문자열 또는 숫자만 허용됨

##### enum 사용 이유 살펴보기

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

// object도 이렇게하면 속성 변경시 컴파일 에러가 뜨긴함 (read-only가 되기 때문에)
const language = {
    korean: `ko`,
    english: `en`
} as const
```



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

```typescript
class Animal {
    name: string;
    leges: number;
    
    constructor(name: string, legs: number = 4) {
        this.name = name;
        this.legs = 4;
    }
    
    info(): string {
        return `${this.name} has ${this.legs} legs`;
    }
}

let dog: Animal = new Animal(`Happy`);
console.log(dog.info());
```

#### 접근제한자 public, private, protected, readonly

자바스크립트에는 접근제한자 지원X, 그래서 실질적으로 컴파일 단계에서만 막아짐

##### public

`public`으로 선언된 멤버 변수나 메서드는 어느 곳에서나 접근이 가능 (명시하지 않았을 경우 default값이 public)
클래스 인스턴스에서 홀로 접근이 가능하다.

```typescript
class Test {
    public test0: number;
    private test1: number;
    protected test2: number;
    
    constructor(test0: number, test1: number, test2: number) {
        this.test0 = test0
        this.test1 = test1
        this.test2 = test2
    }
}

const test = new Test(1,2,3)
console.log(test.test0)
console.log(test.test1) // compile err!
console.log(test.test2) // compile err!
```



##### private

`private` 해당 클래스 내부에서만 접근 가능

##### protected

해당 클래스와 서브클래스에서만 접근이 가능

##### readonly

읽기전용 속성자, getter/setter로만 생성, 설정 가능

```typescript
class Test {
    readonly val: number = 5;
    readonly val2: number;
    
    constructor(val2: number) {
        this.val2 = val2
    }
    
    public addVal() {
        this.val2++ // error
    }
}
```



-------



#### Getter/ Setter

```typescript
class Test {
    private _val: number = 5
    
    get val() {
        return this._val
    }
    
    set val(value: number) {
        this._val = value
    }
}

const test = new Test()
test.val = 10
console.log(test.val);
```



-------

#### 

#### static

별도의 인스턴스가 아닌 클래스 전체에서 공유하는 값

```typescript
class Counter {
    static count: number = 0
    
    static increase() {
        this.count++
    }
}

console.log(Counter.count)
Counter.increase()
console.log(Counter.count)
```



---------



