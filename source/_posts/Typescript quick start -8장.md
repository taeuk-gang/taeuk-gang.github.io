---
title: Quick Start Typescript ~ 8장 정리
toc: true
date: 2020-04-12 13:39:05
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 8장 모듈

##  📝253p. 모듈 필요성

1. 유지보수
2. 전역 스코프 오염 방지
3. 재사용성

### 모듈러 프로그래밍 기반 과정

1. 모듈 식별
2. 모듈 분리 선언
3. 외부 공개



## 📝254p. 내부 모듈, 외부 모듈 차이

타입스크립트 1.5부터 `Namespce`라는 특징과 ES6 모듈 특지이 추가 ECMAScript 표준 용어집에 2가지 형태의 모듈 구분

1. 내부 모듈 - `namespace`
2. 외부 모듈 - `export`



내부모듈, `namespace`란 여러 파일에 걸쳐 하나의 이름을 공유, `reference`를 통해 참조

외부모듈은 파일마다 이름 공간이 정해짐, `import`를 통해 참조



그러므로, `namespace`는 프로젝트와 분리해 라이브러리 단위의 모듈을 구성할 때 좋음

> `@types` 폴더에서 구분하여 타입선언 등 을 하는 듯 하다



## 📝259p. Namespace

```typescript
namespace Hello {}
```

### namespace = module

키워드는 다르지만, 역할과 기능상 차이가 없습니다

#### 키워드 중복 이유

ES2015에서 `namespace` 용어가 표준으로 채택되면서, 원래 Typescript 1.5에서 사용하던 `module` 용어가 자연스럽게 Deprecated됨

> 그런데, Typescript 타입 만드는 example code를 보면 아직은 module이라는 용어가 많이 쓰이는 듯 하다
>
> [참고링크 - chart.js](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/chart.js/index.d.ts)
>
> [여기](https://www.slideshare.net/gloridea/dts-74589285)는 왜 `module`과 `namespace`를 동시에 사용한걸까? 



```typescript
// namespace 사용 예시
namespace Hello {
    function print() {}
}
    
// module 사용 예시
module Hello {
    function print() {}
}
```

ES6 변환 결과

```js
var Hello;
(function: Hello) {
    funtion print() {}
}(Hello || (Hello = {}));	// 모듈이 있으면 전달, 없으면 초기화 = 느슨한 확장 loose argument
```



## 📝261p. 한 파일에 여러 네임스페이스 선언

네임스페이스마다 구분이 필요하게 되므로, `export` 선언 필요

```typescript
namespace MyInfo1 {
    export let name = `name1`;
    export function getName() {
        return MyInfo2.name;
    }
}
    
namespace MyInfo2 {
    export let name = `name2`;
    export function getName() {
        return MyInfo1.name;
    }
}
    
console.log(MyInfo1.getName());
console.log(MyInfo2.getName());
```

변환시 `var`로 변환되어 호이스팅 특성때문에 순서와 상관없이 서로 호출 가능



## 📝263p. ★ 네임스페이스 여러 파일에 선언

프로젝트 규모 커지면, 파일 단위로 모듈을 분할

`tsc` 명령시, 타입스크립트 컴파일러가 자동으로 네임스페이스간 참조 관계를 고려함

그러나, 개별 파일을 컴파일시에는 `///<reference path="to/path" />`가 필요함

파일 상단의 표시하면 됨

그러나, 사실 `tsc` 명령어로도 같이

```bash
ts-node car2.ts # undefined, undefined 출력

tsc --out out.js car2.ts # 합쳐 컴파일 필요
```

| ts-node car2.ts                                              | tsc -out out.js car2.ts                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20200413001659451](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200413001659451.png) | ![image-20200413001720503](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200413001720503.png) |

컴파일 후에도, 결과를 명시적으로 표시되게 하려면, 네임스페이스를 모듈로 선언하고 `import`를 선언 필요

(다음장에서 설명됨)



## 📝268p. 네임스페이스 모듈

### 사용법

```typescript
// car1.ts
export namespace Car {
    export let auto: boolean = false;
    export interface ICar {
        name: string;
        vendor: string;
    }
}
```

```typescript
import * as ns from './car1';

// namespace를 한번 더 선언해서 사용? 이유가 있나?
namespace Car {
    console.log(ns.Car.auto);
    
    class Taxi implements ns.Car.ICar {
        name: string;
        vendor: string;
    }
}
    
console.log(ns.Car.auto);
```



## 📝271p. 네임스페이스 이름 확장

네임스페이스 이름은 `.`을 허용, 이름 계층 확장하는데 사용

**사용 예시**

```typescript
// 순서 바뀌어도 문제는 없지만, 상위에서 하위로 선언하는게 맞음
namespace Animal{}
namespace Animal.Pet{}
```

그러나 상속에 개념이 전혀 아니고, 서로 다른 네임스페이스이므로, 변수나 메소드 공유 X



## 📝274p. 브라우저에서 네임스페이스 모듈 호출

>  네임스페이스간 결국 js에는 없는 개념으로, 순서대로 js 스크립트 호출해서 사용해야한다는 것 같음



## 📝276p. 모듈 사용법

### 개별 export

```typescript
export interface ICar {}
export function Test() {}
```

```typescript
import { ICar, Test } from '../to/path';
```

### 함께 export

```typescript
let ver: string = `1.0`;
let display = () => `hello`;

export { ver, display };
```

```typescript
import { ver, display } from '../to/path';
```

### 모두 export

```typescript
export let ver: string = `1.0`;
export let display = () => `hello`;
```

```typescript
import * as m from './to/path'

console.log(m.ver);
console.log(m.display());
```

### 모듈 재노출

```typescript
export * from '../to/path';
export * from '../to/path';
```

```typescript
import * as m from './to/path';
```

모듈 파일을 가져와서 다시 `export` 하는 예시, 최상위에 모듈에서 많이 사용됨



## 📝281p. ★네임스페이스로 감싸서 재노출

> namepsace 간 class보다 상위의 개념이기 때문에 감싸서 노출하면 편한 것 같다

```typescript
// car-info.module.ts <- 모듈 파일 명명법
export namespace CarInfo {
    export function hello() {}
}
```

```typescript
import { CarInfo } from './car-info.module';

CarInfo.hello();
```



## 📝283p. 디폴트 무법

### export-equals, import-equals 문

> `default` 이전에 사용됬던 방식인 것 같은데, 몰랐던 부분 - 가끔 다른 프로젝트에서 종종 보였는데 이런 의미였구나

```typescript
export = Chart;

// 동일

export default Chart;
export {  Chart as default };
```

### 2

```typescript
import Validator from './validator';

// 동일

import { default as Validator } from './validator';
```

### 주의사항

모듈 하나당 `default`는 하나만 선언 가능

## 📝289 ~ 313p. 모듈시스템 생략

> 모듈 시스템은 각 파트마다 다르기 때문에 웹개발자라면 ES2015모듈을, Node.js개발자라면 CommonJs 모듈 형식을 기본으로 알고, 나머지는 상황에 따라 알아야할 것 같다





## 참고링크

> 책은 이론적인 부분으로 많이 설명되어있어, 실제에서는 어떻게 사용되고, 더 추가적인 내용을 찾아봄

- [타입스크립트 컴파일러가 모듈 타입 선언을 참조하는 과정](https://medium.com/naver-fe-platform/%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC%EA%B0%80-%EB%AA%A8%EB%93%88-%ED%83%80%EC%9E%85-%EC%84%A0%EC%96%B8%EC%9D%84-%EC%B0%B8%EC%A1%B0%ED%95%98%EB%8A%94-%EA%B3%BC%EC%A0%95-5bfc55a88bb6)
- [d.ts 만들기](https://www.slideshare.net/gloridea/dts-74589285) - 개인적으로 몇몇 부분이 잘못? 이해가 안되는 부분 존재
- [타입스크립트 전환 후기 - import, export](https://www.zerocho.com/category/TypeScript/post/5bab2086103eac558e45cdd7) - global로 선언하는건 맞는지 모르겠는 부분
- [Chart.js의 타입 정의](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/chart.js/index.d.ts)
- [정적 타이핑](https://poiemaweb.com/typescript-typing)
- [TypeScript - 남의 라이브러리 쓸 때 d.ts 파일이 없는 경우](https://velog.io/@bigbrothershin/TypeScript-%EB%82%A8%EC%9D%98-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%93%B8-%EB%95%8C-d.ts-%ED%8C%8C%EC%9D%BC%EC%9D%B4-%EC%97%86%EB%8A%94-%EA%B2%BD%EC%9A%B0)

