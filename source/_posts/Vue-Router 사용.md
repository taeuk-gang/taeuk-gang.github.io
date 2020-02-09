---
title: Vue Router 사용
toc: true
date: 2020-01-28 19:04:47
tags:
	- Vue.js
categories:
	- Vue.js
---

## Vue Router

### Install

CDN 또는 `npm install`을 통해 설치

#### CDN

```php+HTML
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

#### NPM

```bash
npm install vue-router
```

```typescript
// main.ts

import router from './router';
```

```js
// router/index.ts

import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

### Use

#### Html

```html
<p>
    <!-- `<router-link>`는 `<a>` 태그로 렌더링-->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
</p>
<!-- 렌더링 되는 장소 -->
<router-view></router-view>
```

#### Typescript

```typescript
// File: router/index.ts

// 1. 모듈과 Home 템플릿 불러오기
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue';

Vue.use(VueRouter)

// 2. 라우트 정의
const routes = [
  {
    path: '/',
    name: 'home',
    component: Home,
  },
  {
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue'),
  },
];

// 3. VueRouter 만들기
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes,
});
```

```typescript
// File: main.ts
// 4. mount 하기 (router 옵션을 전체 앱에 주입)
const app = new Vue({
    router
}).$mount(`#app`)
```

```vue
<!-- File: Home.vue -->
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'home',
  components: {
    HelloWorld,
  },
};
</script>
```

`<router-link>`는 현재 라우트와 일치할 때 자동으로 `.router-link-active` 클래스가 추가

