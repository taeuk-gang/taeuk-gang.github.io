---

title: 일일 커밋 알람 어플
toc: true
date: 2020-11-01 22:22:05
tags: 
    - Projects
categories: 
    - Projects
    - Daily-Commit App
---

# 일일 커밋 알림 어플

## 기획 키워드

- 타겟: 일일 커밋을 목표로 하는 개발자 = 승민이와 나, 개발자의 자기관리
- 오늘 커밋이 없으면, 1시간(옵션 조절 가능)마다 알림 메세지 발송
- 위젯 넣어서, 하루 커밋 개수 확인

## 개발 키워드

- 리액트 네이티브

## 레이아웃

https://xd.adobe.com/view/95d7530f-77a4-496a-638a-c9f49a13014a-e3e6/

![image-20201022061001705](https://i.loli.net/2020/10/22/dEr2MVx7iCquyL4.png)



## expo -> react-native-cli 로 넘어가기

`expo` 를 사용하면, 네이티브 라이브러리 사용에 제한이 되는 것을 알 수 있었음

https://reactnative.dev/docs/0.60/getting-started 여길 참고하여 셋팅

```bash
npx react-native init daily-github --template react-native-template-typescript

npx react-native run-ios

npx react-native run-android
```



## typescript template 사용할 때 버그 발생

[공식 깃허브](https://github.com/react-native-community/react-native-template-typescript) 에도 있는 known issue이고,

[같은 버그가 발생한 사람](https://kimck.tistory.com/entry/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8CReact-Native-TypeScript-%ED%85%9C%ED%94%8C%EB%A6%BF-%EC%82%AC%EC%9A%A9%EC%8B%9C-%EC%97%90%EB%9F%AC) 이 있어서, 이 글을 참고하여 해결

```bash
npm uninstall -g react-native-cli

yarn global add @react-native-community/cli

npx react-native init MyApp --template react-native-template-typescript
```

그리고 MyApp처럼 카멜케이스로 입력해야함 `-` 가 들어가면 에러가 뜸

해당 에러

![image-20201023005651603](https://i.loli.net/2020/10/22/VKJnSxg2FyGz3iR.png)

정상 설치시 뜨는 화면

![image-20201023005733123](https://i.loli.net/2020/10/22/cCWZXs29bl5pwrP.png)

Fast Debug 및 시뮬레이터 둘다 뜨는 것 확인

![image-20201023010127345](https://i.loli.net/2020/10/23/8dOPGC2MWDJYm4f.png)



## eslint auto fix

workspace setting.json에 설정

[해당링크](https://www.digitalocean.com/community/tutorials/linting-and-formatting-with-eslint-in-vs- code) 참조

```json
"editor.codeActionsOnSave": {
  "source.fixAll.eslint": true
},
"eslint.validate": ["javascript"]
```



## 백그라운드 이미지 삽입 (스플래시)

```react
import {ImageBackground} from 'react-native';

const image = require('./assets/img/logo-og.png');

<ImageBackground source={image} style={styles.image}>
  <Text style={styles.text}>백그라운드 이미지</Text>
</ImageBackground>
```

![image-20201023021115627](https://i.loli.net/2020/10/23/JgV6KYhyeNQx2dT.png)

resizemode가 제대로 적용이 안됬는데..?

아니... react-native 공식 다큐먼트가 잘못 작성함

style에 들어가는 것이 아닌, prop에 들어가야 적용이 됨.

```react
<ImageBackground resizeMode="contain">
```



## React-native-firebase 환경설정

https://rnfirebase.io/ 참고

흐음... 어째 코르도바랑 하는 일은 똑같네



## Mac 안드로이드 에뮬레이터에서 인터넷이 안될때

![image-20201023033028119](https://i.loli.net/2020/10/23/5Bn1x8e4zikOGbJ.png)

## ios google-service.json 추가 방법

폴더에 넣는 것만으로는 절대! 안되며, 이런 형식으로 프로젝트에 추가해줘야함

![image-20201023041210230](https://i.loli.net/2020/10/23/RsHJri1mzt7PADZ.png)



## ~~React-native-firebaseui 설치~~

 ~~깃허브 로그인(oauth) 만들기 목적~~

- ~~설치~~

```bash
yarn add @react-native-firebase/app

yarn add @react-native-firebase/auth

cd ios/ && pod install

npm install react-native-firebaseui --save

react-native link react-native-firebaseui
```

- ~~podfile 설정~~

```podfile
pod 'SDWebImage', '~> 4.0'
```

auth에 관한 것이 아니었음



## ~~React-native-firebaseui-auth 사용~~

~~https://github.com/oijusti/react-native-firebaseui-auth  이걸 사용해보기로함~~

### ~~멀티인덱스 에러 이슈~~

- ~~https://stackoverflow.com/questions/50199565/react-native-build-error-while-merging-dex-archives 참고하여 해결~~

안됨 => expo에서 깃허브로 로그인을 구현한 사례가 있어서 이동

-------

## ~~GitHub~~

~~https://blog.expo.io/firebase-github-authentication-with-react-native-2543e32697b4~~

~~여기를 참고하여, 따라가보기~~

expo-auth-session이 독립되면서 많이 바뀐 듯함

[공식 docs](https://docs.expo.io/guides/authentication/#github) 여기를 참고하는 것이 좋아보임



## Authorization callback URL & Redirect URL

development모드에서는 exp:// 을 사용하고, 배포시에는 scheme://를 사용



## accessToken 얻기

https://developer.github.com/apps/building-oauth-apps/authorizing-oauth-apps/

accessToken 발급 확인 완료



## 라우터 구현

https://docs.expo.io/guides/routing-and-navigation/ - 공식 docs의 예제가 오래된 것인지 작동하지 않는다.



[~~https://wordbe.tistory.com/entry/React-Native-5-React-Navigation-%EC%84%A4%EC%A0%95](https://wordbe.tistory.com/entry/React-Native-5-React-Navigation-설정)~~

~~음. 이걸 따라해보지 뭐...~~  deprecated된 패키지



[공식 문서](https://reactnavigation.org/docs/getting-started) 를 참고하여 따라해봄

[참고 예제](https://snack.expo.io/?platform=android&name=Hello%20React%20Navigation%20%7C%20React%20Navigation&dependencies=%40react-native-community%2Fmasked-view%40%5E0.1.7%2C%40react-navigation%2Fbottom-tabs%40%5E5.8.0%2C%40react-navigation%2Fdrawer%40%5E5.9.0%2C%40react-navigation%2Fmaterial-bottom-tabs%40%5E5.2.16%2C%40react-navigation%2Fmaterial-top-tabs%40%5E5.2.16%2C%40react-navigation%2Fnative%40%5E5.7.3%2C%40react-navigation%2Fstack%40%5E5.9.0%2Creact-native-paper%40%5E4.0.1%2Creact-native-reanimated%40%5E1.7.0%2Creact-native-safe-area-context%40%5E3.0.2%2Creact-native-screens%40%5E2.9.0%2Creact-native-tab-view%40%5E2.15.1&sourceUrl=https%3A%2F%2Freactnavigation.org%2Fexamples%2F5.x%2Fnew-screen.js) 따라하면 될 듯

## react-native folder structure

![image-20201027191721516](https://i.loli.net/2020/10/27/6W8j7zVoIChpZiK.png)

https://github.com/pcofilada/simple-react-native-starter



## Header 제거

![image-20201027193122335](https://i.loli.net/2020/10/27/BRkx9vblatnZADU.png)

https://reactnavigation.org/docs/stack-navigator/#headershown 여기 참고

```tsx
<Stack.Navigator initialRouteName="Home" headerMode="none">
```

headerMode 설정과 관련이 있음



## React-native http통신 + useEffect

https://ko.reactjs.org/docs/hooks-effect.html

https://loy124.tistory.com/238



## Redux + Typescript

뭐쓰지? redux? mobx? redux가 함수형에 가까우므로, 오랜만에 사용해보자

### 참고 링크

- https://www.digitalocean.com/community/tutorials/react-react-native-redux

- https://chaewonkong.github.io/posts/react-native-redux.html 

- https://react.vlpt.us/using-typescript/05-ts-redux.html#



## Redux는 선택사항 이라는 글을 읽고...

`useState`를 사용하면서, redux의 필요성에 관해 의문이 들어 자료 조사

https://delivan.dev/react/stop-asking-if-react-hooks-replace-redux-kr/



## Redux Action + axios

[https://velog.io/@secho/React-13-%EB%A6%AC%EB%8D%95%EC%8A%A4-Axios%EB%A1%9C-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0](https://velog.io/@secho/React-13-리덕스-Axios로-상태관리하기)



`createAsyncAction` : https://github.com/piotrwitek/typesafe-actions#createasyncaction

흠... 앱의 규모에 맞지않게 기술 스택이 너무 깊어지는데? 학습 목적이라고 생각을 해야하나...

기존의 간단한 코드로 가능한 사항들이 복잡도가 올라가고 있어서 불편



## Debugger

[https://medium.com/duckuism/react-native-%EB%94%94%EB%B2%84%EA%B9%85-%ED%99%98%EA%B2%BD-%EB%A7%8C%EB%93%A4%EA%B8%B0-7e46bfe89f6](https://medium.com/duckuism/react-native-디버깅-환경-만들기-7e46bfe89f6)



## Router Hook

https://reactrouter.com/web/api/Hooks



## 비율

전체 높이: 812

1. 헤더 68
2. 오늘 커밋 100
3. 첫 커밋 내용 
4. 전체 커밋 현황
5. 셋팅
6. 로그아웃