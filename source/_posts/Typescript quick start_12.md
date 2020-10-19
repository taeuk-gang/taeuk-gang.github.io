---
title: Quick Start Typescript ~ 12장 정리
toc: true
date: 2020-10-19 00:00:00
tags: 
    - Typescript
categories: 
    - Typescript
---




> 📖 Quick Start Typescript 을 읽고, 간단히 몰랐던 부분이나 중요하다고 생각되는 부분을 작성
>



# 12장 비동기 처리

##  📝395p. 브라우저 Promise 지원 현황

IE 지원 안함

Chrome 57 이상

Safari 10 이상

Android Browser 53 이상

IOS safari 10.2 이상

## 📝SetTimeout 3번째 인자

https://developer.mozilla.org/ko/docs/Web/API/WindowTimers/setTimeout

![image-20201020004247332](https://i.loli.net/2020/10/19/8xwAXZgfmGTuL5h.png)

`param ~ paramN` : 타이머 완료 이후, 추가적으로 전달되는 파라미터

```typescript
const promiseAsync = new Promise((resolve, reject) => {
  let sec = Math.floor(Math.random() * 5) + 1;
  setTimeout((isTrue: boolean) => {
    if (isTrue) {
      resolve(sec);
    }
  }, sec * 1000, true);
}).then(res => {
  console.log(res + `s`);
});
```



## Promise.all([]) 결과값

```typescript
function asyncDelay(order: number) {
    return new Promise((resolve) => {
        const ms = Math.floor(Math.random() * 1000) + 1;

        window.setTimeout(() => {
            console.log(`작업 완료: ` + order);
            resolve(order);
        }, ms);
    });
}

function syncResultPromise() {
    const p1 = asyncDelay(1);
    const p2 = asyncDelay(2);
    const p3 = asyncDelay(3);
    const p4 = asyncDelay(4);

    Promise.all([p1, p2, p3, p4]).then(order => {
        console.log(`동기화된 출력: ${order}`);
    });	// LOG: 동기화된 출력: 1,2,3,4
}

syncResultPromise();
```

![image-20201020005554668](https://i.loli.net/2020/10/19/nQ1J5lgir3dueyA.png)

[p1, p2, p3, p4]의 결과값을 한번에 결과값으로 제공해주는 것을 알 수 있음 



## 📝404p. RxJS

> RxJS에 관한 책이 아니기 때문에, 간단하게 설명하고 넘어간 듯 하다.
>
> 간단하게만 정리

반응형 프로그래밍 모델은 스트림 형태의 입력 이벤트를 감지해 반응을 처리할 수 있는 모델

입력된 데이터 스트림은 관측할 수 있고 다랄 수 있는 대상이 되므로 observables이 됨

**발행 구독 패턴**

옵저버 <-> 옵저버블

-> 구독

<- 통지

### 설치

```bash
npm i @reactivex/rxjs
```

### Import

```typescript
import * as Rx from '@reactivex/rxjs';
```

### 데이터 스트림의 형태

유형1: 규칙이 있고, 제한범위가 있는 경우

유형2: 규칙이 있고, 제한 범위가 없는

유형3: 규칙이 없고, 제한 범위가 있는

유형4: 규칙이 없고, 제한 범위가 없는

### 비동기 코드 제어의 방식 차이

`promise`와 다르게 오퍼레이터 메서드를 이용하여 연쇄적으로 처리할 수 있게 관련 인터페이스를 제공

1. 생성 연산자

2. 변형 연산자

3. 콤비네이션 연산자

4. 조건 연산자

5. 필터링 연산자

> 자세한 사항은 RxJs에 관한 문서가 아니기 때문에 생략
>
> 추후, 별도 문서에서 공식 도큐먼트 참고하여 작성하기로...

