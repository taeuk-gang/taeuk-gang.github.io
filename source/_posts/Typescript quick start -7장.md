---
📝title: Quick Start Typescript ~ 7장 정리
toc: true
date: 2020-03-22 20:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>
> 분량이 많아, 이미 아는 내용이나 자주 사용하는 부분은 생략함



# 7장 클래스와 인터페이스 목차

##  📝183p. 타입스크립트의 객체지향 프로그래밍 지원

객체지향의 목적은 코드 중복을 최소화

ES6에서 `class` 키워드가 추가되면서 부족한 점이 존재

| 객체지향 프로그래밍 요소 | Js(ES6)         | Ts                         |
| ------------------------ | --------------- | -------------------------- |
| 클래스                   | class           | class                      |
| 인터페이스               | ★지원안함       | interface                  |
| 인터페이스 구현          | ★지원안함       | implements                 |
| 상속                     | extends         | extends                    |
| 생성자                   | constructor(){} | constructor(){}            |
| 접근 제한자              | ★지원안함       | private, public, protected |
| final 제한자             | ★지원안함       | readonly(Ts 2.0부터)       |
| static 키워드            | static          | static                     |
| super 키워드             | super           | super                      |

> 제한자의 경우, 실제 Js로 변환됬을 경우, 사라지는 부분으로 개발상에서만 제한의 의미가 있는 것이 아쉽다



## 📝p187. 기존Js(prototype) vs class 비교

```js
// 기존 Js Prototype 객체지향 프로그래밍
var Rectangle = (function() {
    function Rectangle(x, y) {
        this.x = x;
        this.y = y;
    }
    
    Rectangle.prototype.getArea = function() {
        return this.x * this.y;
    };
    return Rectangle;
})();

var rectangle = new Rectangle(1, 5);
var area = rectangle.getArea();
console.log(area);
```

모듈 패턴은 클로저를 이용해 비공개된 내부 메소드를 캡슐화하여, 전역공간을 더럽히지 않는 장점 존재

```typescript
// class 객체지향 프로그래밍

interface Rectangle {
    x: number;
    y: number;
    getArea(): number;
}

class Rectanlge {
    x: numberl
    y: number;
    
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    
    getArea(): number {
        return this.x * this.y;
    }
}
```



## 📝p188. 상속(extends), 포함(2) 관계

상속 생략

### 포함 관계

1. 합성(composition) - 강한관계
2. 집합(aggregation) - 약한관계

#### 합성

```typescript
class Engine{}
class Car {
    private Engine;
    constructor() {
        this.engine = new Engine();	// 인스턴스 생성
    }
}

let myCar = new Car();
myCar = null;	// null이 되면 포함된 클래스 함께 제거
```

#### 집합

```typescript
class Engine{}
class Car {
    private engine: Engine;
    constructor(engine: Engine) {
        this.engine = engine;	// 인스턴스 생성
    }
}

let engine = new Engine();	// engine 인스턴스 별도로 선언
let car = new Car(engine);	// 인스턴스 생성시, 포함되는 클래스를 같이 전달
```

위 코드(합성)과의 차이점은 집합은 `car` 객체가 제거되더라도, 
`engine` 객체는 외부에서 선언되었기 때문에 제거되지않음 (수명주기를 함께하지 않기때문에 약한관계)

> 실무에서는 어디에 주로 사용될까?

## 📝p192. 접근 제한자

| 접근 제한자 | 특징                               | 상속 여부 | 외부 객체 접근 |
| ----------- | ---------------------------------- | --------- | -------------- |
| public      | 외부 또는 자식클래스에서 접근 가능 | O         | O              |
| protected   | 자식 클래스에서 접근 가능          | O         | X              |
| private     | 해당 클래스에서만 접근 가능        | X         | X              |



## 📝p195. 축약 코딩기법

챕터와 상관없지만, 관련 있는 변수 묶어서 선언하기 좋아보여서 기록

```js
let [cWidth, cLength, cHeight] = [1, 2, 3];
```



## 📝p195. 접근 제한자 선언 > 클래스 매개변수가 됨

```typescript
class Cube {
    constructor(public width: number) {}
    
    getWidth() {
        return this.width;
    }
}
let cube = new Cube(6);
console.log(cube.width);	// 6
```



## 📝p197. get/set 코딩스타일

> 매개변수/getter/setter 한꺼번에 모아서 관리

```js
class PC {
    ram = `0G`;
	get ramCapcity() {
        return this.ram;
    }
	set ramCapcity(value) {
        this.ram = value;
    }
}
```



## 📝p197. 부모 클래스 멤버 변수 이용

`super()` 키워드와 `this` 사용

`super`는 부모 클래스의 공개 멤버(`public`)에만 접근 가능

`this`는 부모에게 상속받은 멤버와 현재 클래스 모두 접근 가능



## 📝p199. 기본 접근 제한자

**잘못 알고 있던 사항**

constructor 매개변수에서 접근 제한자를 설정 안할시, default가 `public`일줄 알았는데 `private` 임

접근 제한자를 생략할 경우, 생성자 외부에서 매개변수에 접근할 수 없음!

![image-20200406023739559](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200406023739559.png)



## 📝p202. 추상 클래스를 이용한 공통 기능 정의

> 언제 사용될까? 실무에서 자주 사용되는 케이스가 궁금

구현 메소드는 실제 구현 내용을 포함

추상 메소드는 선언만된 메소드, 그러므로 자식 클래스에서 추상 메소드를 받아 별도 구현해야함

! 추상 메서드나 추상 멤버 변수는 자식 클래스에서 사용(`overriding`)할 수 있게 `public`으로 선언해야함

추상 클래스에 기반은 둔 구현 방식은 템플릿 메서드 패턴으로 많이 알려짐

```typescript
abstract class AbstractBird {
  abstract name: string;
  
  abstract flySound(sound: string);
  
  // 구현 메소드가 있어도 상관이 없나보네
  fly(): void {        
      this.flySound(`${this.name}: 파닥`);
  }   
}

class RealBird extends AbstractBird {
  constructor(public name: string) {
      super();
  }
  
  // 오버라이딩
  flySound(sound: string) {
      console.log(`${this.name} 날아감`);
  }
}

let dogBird = new RealBird(`새`);
dogBird.fly();	// 새 날아감
```



## 📝 p205. Interface 다중 상속

> 몰랐던 부분

자식 인터페이스는 여러 부모 인터페이스를 다중 상속 가능

```typescript
interface Car { speed: number }
interface SportsCar { acceleration: number }

interface MyOptimizedCar extends Car, SportsCar {
    waterproof: boolean;
}

let myCar = <MyOptimizedCar>{};	// ?? 인터페이스만으로 인스턴스를 만들 수 있는건가?
myCar.speed = 100;
myCar.acceleration = 100;
myCar.waterproof = true;
```

! 만약 다중 상속 받을 때, 같은 이름의 메소드를 상속받으면 재정의해야함

```typescript
interface Dog {
    run(): void;
    getStatus(): {
        runningSpeed: number;
    };
}

interface Bird {
    fly(): void;
    getStatus(): {
        flightSpeed: number;
    };
}

interface DogBird extends Dog, Bird {
    getStatus(): {
        runningSpeed: number,
        flightSpeed: number;
    }
}

class NewAnimal implements DogBird {
    run(): void {}
    fly(): void {}
    getStatus(): { runningSpeed: number, flightSpeed: number; } {
        return {
            runningSpeed: 10,
            flightSpeed: 20
        }
    }
}
```



## 📝p212. 클래스를 배열 요소로 보고 배열 타입 선언

> 몰랐던 부분
>
> 클래스 자체를 타입 선언 부분에 넣을 수 있음

```typescript
class Person {
    public full: string;
    constructor(public name: string, public city: string) {
        this.full = name + `(${city})`;
    }
}

let personArray: Person[] = [
	new Person(`kim`, `name`),
    new Person(`kang`, `name`)
];
```



## 📝p215. 인터페이스에 함수 타입 정의

익명 함수에 대한 함수 타입 정의 `()`를 사용하면 정의 할 수 있음

! 매개 변수 이름과 타입이 일치하지 않더라도 상관이 없음 (??? 이유가 뭘까)

```typescript
interface IFormat {
    (data: string, toUpper?: boolean): string;
}

let format: IFormat = function (data: string, toUpper: boolean) {
    ...
}
    
let format: IFormat = function (str: string, isUpper: boolean) {
    ...
}
```



## 📝p216. 오버라이딩

오버라이딩 = 부모에서 상속받아, 자식 클래스에서 새로 구현하는 방법

**두 가지 조건 필요**

1. 조건1: 부모클래스의 매개변수 타입이 같거나 상위 타입이여야함

2. 조건2: 부모클래스의 매개변수 개수가 같거나 많아야 함

## 📝p219. 오버로딩

오버로딩 = 메서드의 이름은 같지만 매개변수의 타입과 개수가 다르게 정의하는 방법

```typescript
// 점점 상위의 타입으로 선언
typeCheck(value: number);
typeCheck(value: string);
typeCheck(value: any): void {
    if (typeof value === `number`) console.log(`this is number`);
    else if (typeof value === `string`) console.log(`this is string`);
	else console.log(`nothing`);
}
```



## 📝p222. 인터페이스를 클래스에서 구현하여 오버로딩

```typescript
interface IPoint {
    getX(x: any): any;
}

class Point implements IPoint {
    getX(x?: number | string): any {
		...
    }
}
```

! 인터페이스를 이용하면 선언과 구현을 분리하고 구현부의 구조를 강제

이 점에서 로직과 구조가 섞여 있는 클래스를 상속해 오버로딩하는 것보다

구조만을 가지고 있는 인터페이스를 이용하는 것이 복잡고 낮습니다.



## 📝p224. 다형성

### 종류

1. 클래스의 다형성
2. 인터페이스의 다형성
3. 매개변수의 다형성

### 클래스의 다형성

```typescript
class Planet {
    stopTransduction(): void {
        console.log(`stop - planet`);
    }
}

class Earth extends Planet {
    public features: string[] = [`soil`, `water`, `oxyzen`];

    stopTransduction(): void {
        console.log(`stop - earth`);
    }
    
    earthStop(): void {
        console.log(`stop2 - earth`);
    }
}

let earth: Planet = new Earth();	// ★ Earth 인스턴스를 생성했지만, 타입은 상위의 Planet임
earth.stopTransduction();	// stop - earth, 인스턴스의 메소드를 사용
console.log(earth.features);	// Error, 접근 불가
earth.earthStop();	// Error, 오버라이딩 되지 않은 메소드는 접근 불가
```

부모 클래스의 타입을 지정받은 인스턴스는 실제 동작은 부모 클래스 기준으로 실행됨

그래서 자식 클래스 멤버 변수(`features`)에 접근할 수 없음

그러나, 메소드 자체는 자식 인스턴스의 것이 실행됨 (런타임 다형성(runtime polymorphism)), ex. `duck typing`



### 인터페이스의 다형성

> 클래스와 다르지 않아 코드만 적고 생략

```typescript
interface IPerson {
    getAlias: () => string;
    getAge(): number;
}

class PoliceMan implements IPerson {
    getAlias = () => `happy`;
    
    getAge(): number {
        return 10;
    }
    
    hasClub() {
        return true;
    }
}

let policeMan: IPerson = new PoliceMan();
console.log(policeMan.hasClub());	// Error, 접근 불가
```



### 매개변수의 다형성 (유니언 타입)

```typescript
display(data: string | number) {}
```

#### 문제점

타입 가드가 빡셈

```typescript
class MonitorDisplay {
    display(monitor: Led | Oled | Uhd) {
        if (monitor instanceof Led) {}
        else if (monitor instanceof Oled) {}
        else if (monitor instanceof Uhd) {
            let myMonitor: Uhd = <Uhd>monitor;
            return myMonitor.getName();
        }
    }
}
```

`type` 키워드시 축약은 가능하지만 근본적인 해결책은 될 수 없음(클래스 타입 추가시마다, 매번 업데이트 필요)

```typescript
// 여긴 또 신기하게 대문자로 명명했네..
type MultiTypes = Led | Oled | Uhd;

class MonitorDisplay {
    display(monitor: MultiTypes) { ...(if들) }
}
```

### ★★ 매개변수의 다형성 (인터페이스)

```typescript
interface Monitor {
    getName(): string;
}

class Led implements Monitor {
    constructor(private name: string) {}
    getName(): string {
        return `LED: ` + this.name;
    }
}

class Oled implements Monitor {
    constructor(private name: string) {}
    getName(): string {
        return `Oled: ` + this.name;
    }
}

class MonitorDisplay {
    display(monitor: Monitor) {
        let myMonitor: Monitor = monitor;
        return myMonitor.getName();
    }
}
```

if문 없이 코딩이 가능함



## 📝p236. getter/setter 사용 이유

굳이 `this.name = 'anything'` 처럼 멤버 변수를 사용하지 않고 `get/set` 키워드를 사용하는 이유

값을 설정하거나 읽을 때, 로직을 추가 가능(= 조건 추가 가능)

```typescript
get name(): string {
    return this.studentName;
}

set name(name: string) {
    if (name.includes(`happy`)) {
        this.studentName = name;
    }
}
```

### ES5 변환시

[Object.defineProperty](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 참고

```js
Object.defineProperty(Student.prototype, `name`, {
    get: function() {
    	return this.studentName;
	},
    set: function(name) {
        if (name.includes(`happy`)) this.studentName = name;
    },
    enumerable: true,	// true: 객체 키 열거 가능, default: false
    configurable: true	// true: 이 속성 값 수정/삭제 가능, default: false
})

for (var prop in Student.prototype) {
    console.log(prop);	// 여기에 enumerable 하지 않으면 표시되지 않음
}
```



## 📝p240. `static`

`static` 키워드는 객체 생성 없이  접근 가능하므로 메모리 절약 효과 존재

객체 생성 없이 바로 접근 가능

`static` 멤버 변수는 인스턴스간 값 공유

```typescript
class Circle {
    static circleArea: number = 0;
    get area(): number {
        return Circle.circleArea;	// this가 아닌 Circle을 사용했음
    }
    static set area(pArea: number) {
        Circle.circleArea = pArea;
    }
}
Circle.area = 100;

let circle = new Circle();
console.log(circle.area);	// 100, 공유되는 것을 확인
```



## 📝p242. 싱글톤 패턴

`static` 키워드를 활용하면 유일한 상태 정보 저장 가능

이렇게 하기위해서는 객체 생성을 막고, 클래스 변수, 메소드 등 모두를 `static`으로 선언

단일 상태 관리에는 좋지만, 인스턴스 생성은 불가능

**생성법**

1. 부지런한 초기화 (eager initalization) - 클래스 선언시 초기화
2. 게으른 초기화 (lazy initalization) - 메소드 호출시 초기화

### Eager Initalization

```typescript
class EagerLogger {
    private static uniqueObj: EagerLogger = new EagerLogger();	// 내부에서 자체적 선언
    
    // private를 붙여 인스턴스 생성 방지
    private EagerLogger() {}
    
    // static으로 외부 접근 허용
    public static getLogger(): EaggerLogger {
        return this.uniqueObj;
    }
}
```

### Lazy Initalization

```typescript
class LazyLogger {
    private static uniqueObj: LazyLogger;
    
    private LazyLogger() {}
    
    public static getLogger(): LazyLogger {
        // 생성된 적이 없으면 새로 생성, 타입은 LazyLogger이므로 ==로 타입검사 피함
        if (this.uniqueObj == null) {
            this.uniqueObj = new LazyLogger();
        }
        
        return this.uniqueObj;
    }
}
```



## 📝p247. readonly vs const

> `const`가 사용되는 곳은 `readonly`를 사용하지 못한다고 이해하면 편한 것 같다

| 특성             | const                                  | readonly                               |
| ---------------- | -------------------------------------- | -------------------------------------- |
| 상수 선언        | 가능                                   | 가능                                   |
| 초기화 강제성    | 필수                                   | 선택                                   |
| 값 재할당        | 불가능                                 | 가능(?)                                |
| 선언 가능 대상   | 변수                                   | 멤버 변수<br />객체 리터럴<br />새타입 |
| 선언 불가능 대상 | 멤버 변수<br />객체 리터럴<br />새타입 | 변수                                   |
| 사용 용도        | 상수                                   | 읽기 전용 속성                         |
| 컴파일 선언 유지 | 유지                                   | 사라짐                                 |
| 지원 표준        | ES6                                    | TS 2.0                                 |



## 📝p250. readonly 제거되는 경우

`type` 에일리어싱시 사라짐

```typescript
let emotion: { readonly name: string } = { name: `sad` };

function aliasing(pEmotion: { name: string }) {
    pEmotion.name = `happy`;
}

console.log(emotion.name);	// sad
emotion.name = `happy`;	// Error
aliasing(emotion);
console.log(emotion.name);	// happy, 변경됨
```

