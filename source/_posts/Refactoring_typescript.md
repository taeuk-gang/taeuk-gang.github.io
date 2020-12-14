---
title: 리팩토링 - Typescript
toc: true
date: 2020-12-15 23:43:18
tags:
    - Typescript
categories:
    - Typescript
---

# 리팩토링: Typescript

> 1. 작성 방식
>
> AS-IS 를 제시하고, 리팩토링 기법을 사용한 결과인 TO-BE 와 비교
>
> 2. 연관성 있는 리팩토링 기법은 묶기
> 3. 중요도 순서별로 순서 배열하기

## 1. 값을 참조로 바꾸기

만약 데이터를 갱신해야하는 경우면, 해당 데이터를 값으로 복사하면 안된다. 갱신의 경우에는 참조를 이용

### AS-IS

```typescript
let customer = new Customer(customData);
```

### TO-BE

```typescript
let customer = customerRepository.get(customerData.id);
```



## 2. 객체 통째로 넘기기

변화에 대응이 쉬움, 그러나 함수가 레코드 자체에 의존하지 않는다면 쓰지말 것

### AS-IS

```typescript
const low = aRoom.daysTemRange.low;
const high = aRoom.daysTemRange.high;

if (aPlan.withinRangle(low, high))
```

### TO-BE

```typescript
if (aPlan.withinRange(aRoom.daysTempRange))
```



## 3. 계층 합치기

클래스 계층구조가 개발하면서, 부모와 자식관계가 너무 비슷해져 더는 독립적일 필요가 없는 경우에 사용

### AS-IS

```typescript
class Employee {...}
class Salesperson extends Employee {...}
```

### TO-BE

```typescript
class Employee {...}
```



## 4. 기본형을 객체로 바꾸기

>  직관적이지는 않으나, 이후 프로그램이 커질수록 유지보수가 쉬워짐

### AS-IS

```typescript
const PRIORITY_VALUE = {
  low: 0,
  normal: 1,
  high: 2,
  rush: 3,
} as const;
type PRIORITY_VALUE = typeof PRIORITY_VALUE[keyof typeof PRIORITY_VALUE];
type PRIORITY_KEY = keyof typeof PRIORITY_VALUE;

class Order { 
  _priority: PRIORITY_KEY;

  constructor(data: any) {
    this._priority = data.priority;
  }

  get priority(): PRIORITY_KEY {
    return this._priority;
  }
  
  set priority(value: PRIORITY_KEY) {
    this._priority = value;
  }
  
  higherThan(otherValue: PRIORITY_KEY): boolean {
    return PRIORITY_VALUE[this._priority] > PRIORITY_VALUE[otherValue];
  }
}

const orders: Order[] = [
  new Order({priority: `normal`}), 
  new Order({priority: `high`}), 
  new Order({priority: `rush`})
];

console.log(
  orders.filter(order => order.higherThan(`high`))
);
```

### TO-BE

```typescript
const PRIORITY_VALUE = {
  low: 0,
  normal: 1,
  high: 2,
  rush: 3,
} as const;
type PRIORITY_VALUE = typeof PRIORITY_VALUE[keyof typeof PRIORITY_VALUE];
type PRIORITY_KEY = keyof typeof PRIORITY_VALUE;

// 2. 기본형 대신, 클래스 생성
class Priority {
  _value: PRIORITY_KEY;

  constructor(value: PRIORITY_KEY) {
    this._value = value;
  }
  
  priority(): PRIORITY_KEY {
    return this._value;
  }
  
  higherThan(other: Priority): boolean {
    return PRIORITY_VALUE[this._value] > PRIORITY_VALUE[other._value];
  }
}

class Order { 
  _priority: Priority;

  // 1. 캡슐화
  constructor(data: any) {
    this._priority = new Priority(data.priority);
  }
  
  // 3. 게터 및 세터 수정
  get priority(): Priority {
    return this._priority;
  }

  get priorityValue(): PRIORITY_KEY {
    return this._priority.priority();
  }
  
  set priorityValue(value: PRIORITY_KEY) {
    this._priority = new Priority(value);
  }
}

const orders: Order[] = [
  new Order({priority: `normal`}), 
  new Order({priority: `high`}), 
  new Order({priority: `rush`})
];

console.log(
  orders.filter(order => order.priority.higherThan(new Priority(`normal`)))
);
```

절차

1. 변수 캡슐화
2. 클래스 생성
3. setter와, getter 수정



## 5. 단계 쪼개기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 6. 레코드 캡슐화하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 7. 매개변수 객체 만들기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 8. 매개변수를 질의 함수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 9. 매직 리터럴 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 10. 메서드 내리기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 11. 메서드 올리기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 12. 명령을 함수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 13. 문장 슬라이드하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 14. 문장을 함수로 옮기기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 15. 문장을 호출한 곳으로 옮기기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 16. 반복문 쪼개기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 17. 반복문을 파이프라인으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 18. 변수 이름 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 19. 변수 인라인하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 20. 변수 쪼개기

### AS-IS

```typ

```

### TO-BE

```typescript

```



## 21. 변수 추출하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 22. 변수 캡슐화하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 23. 생성자 본문 올리기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 24. 생성자를 팩터리 함수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 25. 서브클래스 제거하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 26. 서브클래스를 위임으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 27. 세터 제거하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 28. 수정된 값 변환하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 29. 슈퍼클래스 추출하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 30. 슈퍼클래스를 위임으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 31. 알고리즘 교체하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 32. Assertion 추가하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 33. 여러 함수를 변환 함수로 묶기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 34. 여러 함수를 클래스로 묶기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 35. 예외를 사전확인으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 36. 오류 코드를 예외로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 37. 위임 숨기기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 38. 인라인 코드를 함수 호출로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 39. 임시 변수를 질의 함수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 40. 제어 플래그를 탈출문으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 41. 조건문 분해하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 42. 조건부 로직을 다형성으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 43. 조건식 통합하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 44. 죽은 코드 제거하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 45. 중개자 제거하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 46. 중첩 조건문을 보호 구문으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 47. 질의 함수를 매개변수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 48. 질의 함수와 변경 함수 분리하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 49. 참조를 값으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 50. 컬렉션 캡슐화하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 51. 클래스 인라인하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 52. 클래스 추출하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 53. 타입 코드를 서브클래스로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 54. 특이 케이스 추가하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 55. 파생 변수를 질의 함수로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 56. 플래그 인수 제거하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 57. 필드 내리기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 58. 필드 올리기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 59. 필드 옮기기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 60. 필드 이름 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 61. 함수 매개변수화하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 62. 함수 선언 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 63. 함수 옮기기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 64. 함수 인라인하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 65. 함수 추출하기

### AS-IS

```typescript

```

### TO-BE

```typescript

```



## 66. 함수를 명령으로 바꾸기

### AS-IS

```typescript

```

### TO-BE

```typescript

```

