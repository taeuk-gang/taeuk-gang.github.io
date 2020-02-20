---
title: Vuex + Typescript 구성
toc: true
date: 2020-02-21 02:04:47
tags:
	- Vue
	- Vuex
	- Typescript
categories:
	- Vue
	- Vuex
---

> 이 문서는 "타입스크립트, AWS 서버리스로 들어올리다" 책을 기반으로 작성되었습니다.

## 개요

`vuex`는 vue 애플리케이션 상태관리 패턴 라이브러리이다.

부모 <> 자식 간 컴포넌트 통신(`props`, `emit`)으로, 모든 것을 처리하기에는 힘들기 때문에 만들어졌다. 

## 구조

![image](https://user-images.githubusercontent.com/26294469/74972784-b434a300-5465-11ea-8c50-879ca073b788.png)

### 용어 설명

- `state`: 변수 조회
- `getter`: 변수 조회 (`computed`적 역할을 함)
- `action`: 변수 내용 변경, `async`같은 비동기적 처리가 가능
- `mutation`: 변수 내용 변경, 동기적으로 이루어짐, `state`는 `mutate`를 통해서만 변경될 수 있다.



- `dispatch`: `action` 실행
- `commit`:  `mutate` 실행
- `mutate`:  `state` 변경
- `render`:  컴포넌트 View 수정

## Example Code

### src/store/Counter.ts

```typescript
import { Module, GetterTree, MutationTree, ActionTree, ActionContext } from 'vuex';

// state로 사용할 클래스
export class Counter {
    public count: number = 0;
}

// state를 출력하면서, 표시를 다르게 하기위해 설정(state를 동시에 컴포넌트 이용시)
const getters: GetterTree<Counter, any> = {
    doubleCount(state: Counter): number {
        return state.count;
    },
};

// state를 바로 변경할 수 없게함. 꼭 mutate를 통해 변경 가능
const mutations: MutationTree<Counter> = {
    increment(state: Counter, step: number) {
        state.count = state.count + step;
    },
}

// action은 지연된 상태 변경이 가능하다(비동기적 처리)
const actions: ActionTree<Counter, any> = {
    inc(state: ActionContext<Counter, any>, step: number) {
        state.commit(`increment`, step);
    },
}

// module간 state, getters, mutations, actions 따로 관리 가능하다.
const Counter: Module<Counter, any> = {
    namespaced: true, // <- false일 경우, getters, mutations, actions의 이름을 공용으로 사용
    state: new Counter(),
    getters,
    mutations,
    actions,
};

export default Counter;
```

### src/store.ts

```typescript
import Vue from 'vue';
import Vuex, { Module } from 'vuex';

import Counter from './store/Counter';	// <- module import

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {},
    modules: {	// <- module 등록
        Counter,
    },
});

export default store;
```

### src/App.html (컴포넌트 - `Template` 부분)

```html
<h1>Counter</h1>
<p>{{ $store.state.Counter.count }}</p>
<p>{{ $store.getters[`Counter/doubleCount`] }}</p>
<p>
    <input type="button" 
           @click="$store.commit('Counter/increment', 1)"
           value="+1 증가(Mutate)" />
</p>

<p>
    <input type="button"
           @click="$store.dispatch('Counter/inc', 2)"
           value
</p>
```

`vuex`는 `$store`을 통해 전역 사용

#### state 바로 접근법

```typescript
$store.state.Counter.count
```

#### getters 호출

```ty
$store.getters[`Counter/doubleCount`]
```

`namepsaced`가 설정되어있기 때문에, `<모듈이름>/<뮤테이션이름>`으로 구분한다.

#### commit (mutations 호출)

```typescript
$store.commit(`Counter/increment`, {name: `Kang`, age: 26});
```

`commit` 을 통해 `mutations` 호출

#### dispatch (actions 호출)

```typescript
$store.dispatch(`Counter/inc`, 2);
```



-----

## mapper

컴포넌트에서 `vuex`가 많이 사용될 경우, 축약해서 사용할 수 있는 방법

그래도 위의 기본은 알고가자

### src/App.ts

```typescript
import Vue from 'vue';
import Component from 'vue-class-component';
import WithRender from './App.html';
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex';
import './App.scss';

@WithRender
@Component({
    computed: { // <- state, gettes 등록
        ...mapState({
            count: (state: any) => state.Counter.count,
        }),
        ...mapGettes({
            doubleCount: `Counter/doubleCount`,
        }),
    },
    methods: { // <- mutations, actions 등록
        ...mapMuations({
            increment: `Counter/increment`,
        }),
        ...mapActions({
            inc: `Counter/inc`,
        }),
    },
})

export default Class App extends Vue {}
```

### src/App.html

```html
<h2>Mapper</h2>
<p>{{ count }}</p>
<p>{{ doubleCount}}</p>
<p>
    <input type="button" @click="increment(1)" value="+1 증가(mutation)" />
</p>
<p>
    <input type="button" @click="inc(2)" value="+2 증가(action)"
</p>
```

### mapper 장단점

`vuex`를 간략하게 사용할 수 있지만, 직접적인 호출이 아니기 때문에 모르는 사람이 본다면 가독성, 이해도가 떨어질 수 있다. (고인물 생성)

-----------

## vue-property-decorators를 이용한 사용

### 설치

```bash
npm i vuex-module-decorators
```

### src/store/Counter.ts

```typescript
import { VuexModlue, Module, Mutation, Action } from 'vuex-module-decorators';

@Module({	// <- Module 데코레이터, 이 클래스 vuex 모듈로 사용가능
    namespaced: true,
})

class Counter extends VuexModule {	// <- VuexModule 상속
    public count: number = 0;	// <- state로 매핑

	get doubleCount(): number {	// <- getters로 매핑
    	return this.count + 2;
    }
    
    @Muation	// <- mutation
    public increment(step: number) {
        this.count += step;
    }
    
    @Action		// <- action
    public inc(step: number) {
        return step;	// <- return값을 commit을 통해 전달함
    }
}

export default Counter;
```

이것 또한 데코레이터를 다 외워야 함은 있지만, mapper보다 훨씬 가독성이 뛰어난 편으로 보여 사용성이 좋아보인다.

`Action` 부분이 특이한데, return 값이 `payload`로  자동으로 `commit`으로 전달된다.

`Action`내에서 다른 `mutation` 실행법

```typescript
this.context.commit(`increment`, step);
```

