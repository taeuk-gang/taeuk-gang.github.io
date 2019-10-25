---
title: 서비스 워커 학습 일지
toc: true
date: 2019-09-27 22:51:30
tags:
    - Javascript
    - ServiceWorker
categories:
    - Javascript
---

## 서비스 워커 학습 일지

오프라인, 주기적 백그라운드 동기화, 푸쉬알람 등이 웹에서 지원이 됨
서비스 워커는 이런 기능들의 기반을 제공

### 정의

서비스 워커 = 백그라운드에서 실행되는 스크립트

- 웹페이지와 별개로 작동

- 사용자 상호작용이 필요하지 않은 기능에 대해 서비스 제공

### 유의 사항

- DOM에 직접 액세스할 수 없음
  대신 postMessage 방식으로 메시지 응답
- 네트워크 프록시, 페이지의 네트워크 요청 처리 방법을 제어 가능
- `onfetch`, `onmessage` 핸들러를 통해, 사용하지 않을 때는 종료하고 필요할 때 실행 가능
- 재사용 정보의 경우, `indexedDB`를 통해 사용
- 프라미스가 많이 사용됨
- `HTTPS`를 사용해야함

### 수명 주기

수명주기 또한, 웹 페이지와 별개임

서비스 워커를 사이트에 설치하려면, 페이지에서 자바스크립트를 등록

```html
<script>
    if ('serviceWorker' in navigator) {
        navigator.serviceWorker.register('/service-worker.js');
    }
</script>

or

<script>
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }, function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
</script>
```

### `register()`

페이지를 로드할 때마다, `register()` 호출 가능

#### 주의점

서비스 워커가 위치한 파일 위치를 root로 `fetch`가 작동함

ex. `/example/sw.js`에 위치를 등록하면, `/example/` URL에서만 `fetch`이벤트를 처리

#### 확인

`chrome://inspect/#service-workers`에서 확인 가능

#### 설치

```js
self.addEventListener('install', function(event) {
  // Perform install steps
});
```

`install` 콜백 안에서 다음 절차를 수행해야 합니다.

1. 캐시를 엽니다.
2. 파일을 캐시합니다.
3. 필요한 모든 자산이 캐시되었는지 확인합니다.

```js
var CACHE_NAME = 'my-site-cache-v1';
var urlsToCache = [
  '/',
  '/styles/main.css',
  '/script/main.js'
];

self.addEventListener('install', function(event) {
  // Perform install steps
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(function(cache) {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
});
```

> 흐음... 이건 사용해봐야 알겠는데?

원하는 캐시 이름을 사용

`caches.open()`을 호출하여, `cache.addAll()`을 호출하여 파일 배열 전달

`event.waitUntil()`메서드는 설치 소요 시간 및 설치 성공여부를 확인

모든 파일이 성공적으로 캐시되면 서비스 워커가 설치됩니다. 
어느 파일 **하나라도** 다운로드하지 못하면 설치 단계가 실패합니다.

------

으음... 지금 문서 요약은 가능해도, 실제 코딩과 결합해봐야 알겠는데...

서비스 워커로 `js` 파일을 관리하는 웹사이트가 있어서 공부해볼려 했는데, 그 사이트부터 분석해봐야겠다.

[서비스 워커 이용 사이트](https://flash-cards.netlify.com/)

------

#### 요청 캐시 및 반환

#### 서비스 워커 업데이트

#### 이슈