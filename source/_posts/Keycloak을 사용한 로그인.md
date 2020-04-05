---
title: Keycloak을 사용한 로그인 구현
toc: true
date: 2020-03-02 02:04:47
tags:
	- Keycloak
	- Auth
categories:
	- Auth
	- Login
---

## 목적

`Keycloak`을 사용한 Client 관점의 로그인 구현 개발 일지

개인 학습 용도로, 틀린 요소가 있을 수 있음 [참고 링크](https://medium.com/keycloak/secure-vue-js-app-with-keycloak-94814181e344)

## 백엔드 설치

그래도 백엔드가 없으면, Client에서 되는지 안되는지 모르므로 간단하게 도커를 이용해 환경설정을 해준다.

```sh
docker pull jboss/keycloak
```

```sh
docker run -d -e KEYCLOAK_USER=<USERNAME> -e KEYCLOAK_PASSWORD=<PASSWORD> -p 8081:8080 jboss/keycloak
```

환경구성 끝 (세상이 좋아졌다...)

## ![띄운 이미지 http://localhost:8081](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302222924356.png)

## 백엔드 설정

### 설정 화면 들어가기

|                       1. 버튼 클릭하기                       |                        2. 로그인 하기                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302223431929.png" width="400"> | <img src="https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302224827885.png" width="400"> |



### 1.realm 만들기

> realm이 뭐지?
>
> 인증, 인가가 작동하는 범위
> SSO를 예로 들면 특정 클라이언트들이 공통적으로 속한 Realm에 한정되며, 기본적 으로 삭제가 불가능한 Master라는 Realm을 제공받음
>
> Keycloak에서 Client
>
> Keycloack에게인증을 맡길 애플리케이션
> Web or REST API Service
>
> Keycloak에서 User
>
> Client에 로그인할 사용자
> 하나의 Realm은 종속된 n개의 User를 관리
> 기본적으로 User 개체는 Username, Email, First Name, Last Name 항목을 가짐 + Custom Attr 가능

#### Add realm (1)

![Add realm](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302223544755.png)

#### Add realm (2)

![Create](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302223653898.png)

### 2. Client 추가

![Client 추가](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302225114776.png)

> Client protocol 종류?
>
> openid-connect : 2014년에 개발된 더 새로운 표준 인증 서비스, Oauth 위에 놓임
>
> saml : 인증 및 권한 부여 서비스를 제공하는 방법을 정의, Oauth 는 권한 부여만 처리, 멀티 도메인 위주?
>
> *Oauth와의 차이점 : 오쓰(OAuth)는 2006년부터 구글과 트위터에서 공동으로 개발한 SAML보다 다소 새로운 표준, 모바일에서의 SAML의 결함을 보완 / XML이 아닌 JSON 기반
>
> 으음 가볍게 openid-connect를 써주면 될 듯

![결과화면](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302233640462.png)

> 반드시 `Valid Redirect URIs`를 `http://localhost:8080/*`로 할 것! 

### 3. Client 설정

2가지 키값이 중요하다. (`Valid Redirect Url` / `Web Origin`) 

### 4. Realm settings - Login 설정

![image-20200302230342042](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200302230342042.png)



## 프론트엔드 설치

**기본 vue init**

```bash
vue create vue-keycloak
```

**keycloak-js 설치 (공식 라이브러리)**

```bash
npm i keycloak-js --save
```

## 프론트엔드 코드 작성

`src/main.js` 작성

```js
import Vue from 'vue'
import App from './App.vue'
import * as Keycloak from 'keycloak-js'

Vue.config.productionTip = false

/* 	url: keycloak auth 도메인
	realm: keycloak realm 이름,
	clientId: keycloak client 이름,
	onLoad: login-required 말고 다른 속성이 뭐가 있을까?
*/
let initOptions = {
  url: `http://127.0.0.1:8081/auth`,
  realm: `accordion-keycloak`,
  clientId: `keycloak-client`,
  onLoad: `login-required`,
};

let keycloak = Keycloak(initOptions);

init();

function init() {
  keycloak.init({
    onLoad: initOptions.onLoad,
  }).success(auth => {
    const ONE_MINUTE = 60000;
  
    if (!auth) {
      window.location.reload();
    } else {
      console.info(`Auth ok`);
    }
  
    new Vue({
      render: h => h(App),
    }).$mount(`#app`);
  
    localStorage.setItem(`vue-token`, keycloak.token);
    localStorage.setItem(`vue-refresh-token`, keycloak.refreshToken);
  
    window.setTimeout(refreshToken.bind(null, keycloak), ONE_MINUTE);
  }).error(() => {
    console.error(`Auth Fail`);
  })
}

function refreshToken() {
  keycloak.updateToken(70).success(refreshed => {
    if (refreshed) {
      successRefresh(refreshed);
    } else {
      warnRefresh();
    }
  }).error(errorRefresh);
}

function successRefresh(refreshed) {
  console.debug(`Token refreshed ${refreshed}`);
}

function warnRefresh() {
  console.warn(`Token not refreshed, valid for 
  ${Math.round(keycloak.tokenParsed.exp + keycloak.timeSkew - new Date().getTime() / 1000)} seconds`);
}

function errorRefresh() {
  console.error('Failed to refresh token');
}
```

![Demo](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/test.gif)

## 결론

`FE`에서 Vue.js를 사용하긴 했지만 별도 Vue 의존성이 있는건 아니라서, VanilaJS 또는 React에서도 바로 사용할 수 있을 것 같다. 또한, Client에서 사용이 어렵지않아, 쉽게 사용할 수 있을 것 같다.

그렇지만, Keycloak 객체에 어떤 메소드들이 있는지 정리할 필요성이 있다. 
(라이브러리를 사용함에는 그 기능을 완벽히 파악하는 것이 중요하기 때문에...)

[Javascript docs](https://github.com/keycloak/keycloak-documentation/blob/master/securing_apps/topics/oidc/javascript-adapter.adoc)

## 참고링크

[Secure Vue.js app with Keycloak](https://medium.com/keycloak/secure-vue-js-app-with-keycloak-94814181e344)

[Vue + Keycloak github](https://github.com/akoserwal/keycloak-integrations/tree/master/vue-keycloak)

[Keycloak을 이용한 SSO 구축](https://tech.socarcorp.kr/security/2019/07/31/keycloak-sso.html)

[keyclaok github](https://github.com/keycloak/keycloak)