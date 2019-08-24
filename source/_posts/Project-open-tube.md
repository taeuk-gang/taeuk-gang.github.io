---
title: 오픈 튜브 프로젝트 일지
toc: true
date: 2019-08-24 23:12:45
tags:
    - Projects
    - open-tube
categories:
    - Projects
    - open-tube
---

# 1차 작업 일지

## 요약
1차 레이아웃 작업 내용 ㅇㅅㅇ

## 구현사항

- SCSS + Autoprefixer 추가
- 로그인 페이지 / 메인 페이지 구현
- 로그인 구현 (firebase Auth - Apache 2.0 이용)
- 폴더구조 변경
  - 상세사항은 아래에 작성
- 크로스 플랫폼 고려 (모바일 환경 고려)
  - 상세사항은 아래에 작성
- IE 지원 X
  - 그리드 자동배치, 파이어베이스 미지원으로 인하여 지원 종료
- Shadow DOM 제거 - [FOUC](https://www.google.com/search?ei=mfFOXb6vBITemAWm-oKADQ&q=FOUC&oq=FOUC&gs_l=psy-ab.3..35i39j0i67j0l8.1084500.1085562..1085738...1.0..0.147.593.0j5....3..0....1..gws-wiz.....10..0i10.lzSwpNONsTA&ved=0ahUKEwj-3_ev3fjjAhUEL6YKHSa9ANAQ4dUDCAo&uact=5) 현상으로 인하여 제거

## 코드 테스트 방법

### 로그인 확인

1. 구글 로그인 여부 확인
2. 라우팅 처리 잘 되어있는지 확인
   비로그인시 `/report ` 라우팅 접속 불가

### 폴더구조 확인

- 삭제된 파일 및 이동된 폴더 검사

### 모바일 창에서 확인

- 브라우저 크기를 작게하여 확인

### IE 미지원 확인

- IE 에서 접속시, 해당 브라우저는 지원되지 않습니다. 문구 표시



## 스크린샷

### 로그인 페이지

| 이름   | 이미지                                                       |
| ------ | ------------------------------------------------------------ |
| 로그인 | ![image](https://user-images.githubusercontent.com/26294469/62824505-1caee780-bbda-11e9-82d4-cec9e0fa2536.png) |

### 메인 페이지

| 이름 | 이미지                                                       |
| ---- | ------------------------------------------------------------ |
| 메인 | ![image](https://user-images.githubusercontent.com/26294469/62629569-5c6e9880-b968-11e9-95da-584c6a83b9de.png) |



### 모바일 창

- GIF 캡쳐 프로그램 오류로, 색 변조 존재함.... (하늘색 무시해줘용)

| 이름   | 이미지                                                       |
| ------ | ------------------------------------------------------------ |
| 로그인 | ![image](https://user-images.githubusercontent.com/26294469/62824603-87acee00-bbdb-11e9-9cd1-497ddb86bdf0.gif) |



### 폴더구조 변경

📦client
 ┣ 📂.storybook
 ┃ ┗ 📜config.js
 ┣ 📂functions
 ┃ ┣ 📜.eslintrc.json
 ┃ ┣ 📜.gitignore
 ┃ ┣ 📜index.js
 ┃ ┣ 📜package-lock.json
 ┃ ┗ 📜package.json
 ┣ 📂public
 ┃ ┣ 📂src
 ┃ ┃ ┗ 📂css
 ┃ ┃ ┃ ┣ 📜foundation.min.css
 ┃ ┃ ┃ ┗ 📜style.css						# 추가 - SCSS 번들링 파일
 ┃ ┣ 📜index.css
 ┃ ┗ 📜index.html
 ┣ 📂src
 ┃ ┣ 📂components
 ┃ ┃ ┣ 📜app-footer.js
 ┃ ┃ ┣ 📜filter-list.js
 ┃ ┃ ┣ 📜nav-top.js
 ┃ ┃ ┗ 📜report-list.js
 ┃ ┣ 📂libs										# 삭제 - LitRender 파일 삭제
 ┃ ┃ ┣ 📜actions.js
 ┃ ┃ ┣ 📜i18n.js
 ┃ ┃ ┣ 📜redux-zero.js
 ┃ ┃ ┗ 📜store.js
 ┃ ┣ 📂pages									# 추가 - 페이지별 JS파일 별도 관리
 ┃ ┃ ┣ 📜page-login.js
 ┃ ┃ ┗ 📜page-reports.js
 ┃ ┣ 📂scss										# 추가 - SCSS 파일 모음
 ┃ ┃ ┣ 📂components
 ┃ ┃ ┃ ┣ 📜app-footer.scss
 ┃ ┃ ┃ ┣ 📜filter-list.scss
 ┃ ┃ ┃ ┣ 📜nav-top.scss
 ┃ ┃ ┃ ┗ 📜report-list.scss
 ┃ ┃ ┣ 📂pages
 ┃ ┃ ┃ ┣ 📜page-login.scss
 ┃ ┃ ┃ ┣ 📜page-reports.scss
 ┃ ┃ ┃ ┗ 📜vars.scss
 ┃ ┃ ┗ 📜main.scss
 ┃ ┣ 📂stories
 ┃ ┃ ┗ 📜index.stories.js
 ┃ ┣ 📂_locale									# 추가 - 언어리소스 모음
 ┃ ┃ ┗ 📜ko.js
 ┃ ┗ 📜main.js
 ┣ 📂test
 ┃ ┗ 📜index.html
 ┣ 📜.babelrc
 ┣ 📜.codebeatignore
 ┣ 📜.eslintignore
 ┣ 📜.eslintrc.js
 ┣ 📜.firebaserc
 ┣ 📜.gitignore
 ┣ 📜.travis.yml
 ┣ 📜database.rules.json
 ┣ 📜firebase.json
 ┣ 📜firestore.indexes.json
 ┣ 📜firestore.rules
 ┣ 📜package-lock.json
 ┣ 📜package.json
 ┣ 📜postcss.config.js
 ┣ 📜README.md
 ┣ 📜storage.rules
 ┣ 📜wct.conf.json
 ┗ 📜webpack.config.js



## 기타


------------

# 2차 작업 일지

## 요약
- 일반 유저와 관리자를 `uid`를 통해 구분하는 코드를 작성했으며, 사용자가 우리 서비스에 레포트를 요청할 수 있는 모달창을 구현했습니다.

## 구현 사항
- `firebase.auth ~ uid`를 통해, 관리자ID 구별 (서버리스 함수로 구현)

- CSS duplicate 되던 버그 수정

- 쉬운 아이콘 사용을 위해 Foundation-icon 모듈 추가 [(자세한 사항](https://zurb.com/playground/foundation-icon-fonts-3)]

- 레포트 요청 모달창 구현 (아래의 스크린샷 첨부)

## 코드 테스트 방법

### 유저 이름 확인
- 로그인 페이지에서 로그인을 하여, 메인 페이지 상단바의 사용자 이름이 뜨는지 확인

### 레포트 요청 모달창 확인
- 메인 페이지에서 레포트 요청 버튼을 클릭하여 정상 표시되는지 확인

## 스크린샷

이름 | 이미지 
--------- | ---------
사용자 이름 | ![image](https://user-images.githubusercontent.com/26294469/62828631-28280000-bc26-11e9-87d8-810868335c3d.png)

이름 | 이미지 
--------- | ---------
모달창 | ![image](https://user-images.githubusercontent.com/26294469/62828579-26aa0800-bc25-11e9-936c-0a5301870de6.png)


## 기타


----------

# 3차 작업 일지

## 요약
- 더미 API를 받고, 레포트를 불러오는 기능과 레포트 모달창 구현

## 구현 사항
- [x] 더미 API 작성(레포트 상태, 영상제목, URL)
- [x] 레포트 UI 추가
- [x] 레포트 Router 추가

## 코드 테스트 방법
1. 구글 open.tube.service 계정으로 로그인 (슬랙: info에 ID/PW 있음)
2. 레포트가 정상적으로 받아오는지 확인
3. 레포트를 클릭하여 모달창 뜨는지 확인

## 스크린샷

이름 | 이미지 
--------- | ---------
더미 DB | ![image](https://user-images.githubusercontent.com/26294469/62915236-868fe280-bdce-11e9-835b-2723e81646c9.png)

이름 | 이미지 
--------- | ---------
더미 API | ![image](https://user-images.githubusercontent.com/26294469/62915282-c1921600-bdce-11e9-9435-5cc1a2ff093f.png)

이름 | 이미지 
--------- | ---------
메인 페이지 | ![image](https://user-images.githubusercontent.com/26294469/62915304-e090a800-bdce-11e9-92c0-996b42b8d44b.png)

이름 | 이미지 
--------- | ---------
메인 페이지 | ![image](https://user-images.githubusercontent.com/26294469/62915332-01f19400-bdcf-11e9-8410-293bef55f359.png)

## 기타
- close #44 

---------

# 4차 작업 일지

## 요약
- 웹팩 번들링 파일 분석을 위한 모듈 추가 및 라우팅 최적화

## 구현 사항
- `webpack-stylish`, `webpack-visualizer-plugin` 모듈 추가
> 기능 설명: 각 모듈의 파일 사이즈, 번들링 시간 등을 파악
- 라우팅 최적화
- `firebaseui` cdn -> 로컬로 변경, 네트워크에서 느리게 받아오는 것을 확인하여 변경

## 코드 테스트 방법
- 각 리뷰어가 확인할 사항 없음

## 스크린샷
- No ScreenShot

## 기타

--------

# 5차 작업 일지

## 요약
- 레포트 페이지 구현 및 CSS 테마1 적용

## 구현 사항
- 레포트 - 영상 삽입 (동적 이용을 위해 Youtube Iframe API 사용)
- 레포트 - 영상 기본 정보
- 레포트 - 얼굴 인식
- 레포트 - 댓글 분석
- 레포트 - 키워드 분석

## 코드 테스트 방법
1. https://open-tube.web.app 접속

2. open.tube.service 구글 ID 로그인

3. ID의 부여된 4개의 레포트 테스팅

## 스크린샷

이름 | 이미지 
--------- | ---------
로그인 페이지 | ![image](https://user-images.githubusercontent.com/26294469/63513039-2876a400-c520-11e9-9c2a-bdc5e7c241fd.png)

이름 | 이미지 
--------- | ---------
메인 페이지 |  ![image](https://user-images.githubusercontent.com/26294469/63512896-cf0e7500-c51f-11e9-8dd4-b8c412267867.png)

이름 | 이미지 
--------- | ---------
요청 페이지 |  ![image](https://user-images.githubusercontent.com/26294469/63513076-40e6be80-c520-11e9-82da-e6f7364ed3b6.png)

> History API 수정 전으로, 아직 API 연결 안함 (아직 Status 분류가 되어있지 않음)

이름 | 이미지 
--------- | ---------
영상정보 및 얼굴인식 |  ![image](https://user-images.githubusercontent.com/26294469/63513172-71c6f380-c520-11e9-9e31-04ac0b878f96.png)

> Seconds Link 클릭시, 그 시간대로 영상 재생

이름 | 이미지 
--------- | ---------
댓글 분석 |  ![image](https://user-images.githubusercontent.com/26294469/63513273-b783bc00-c520-11e9-8f2d-a6747c6b59cd.png)

> 각 항목별 오른차순·내림차순 정렬 가능

이름 | 이미지 
--------- | ---------
키워드분석 |  ![image](https://user-images.githubusercontent.com/26294469/63513350-e6019700-c520-11e9-9673-4d9282a325c8.png)


## 특이 사항
- Youtube Iframe API 사용
- Word-Cloud 오픈소스(MIT) 사용

## 기타
- closed issue #58 


-----------

# 6차 작업 일지

## 요약
레포트 상태 분류 / API 연결 / GIF 설명 삽입
## 구현 사항
- 레포트 - 상태 분류 표시 (대기/ 분석/ 완료)
- 모달 메세지 컴포넌트 추가
- 레포트 요청 API 연결
- 로그인 GIF 설명 추가

## 코드 테스트 방법
1. https://open-tube.web.app 접속

2. open.tube.service 구글 ID 로그인

3. ID의 부여된 4개의 레포트 테스팅

## 스크린샷

이름 | 이미지 
--------- | ---------
최종상태 |  ![ezgif-3-1a5ed28f0577](https://user-images.githubusercontent.com/26294469/63587689-97670200-c5df-11e9-82dc-4f855a3bb452.gif)

## 기타

-----------

# 7차 작업 일지

## 요약
- 자잘한 버그 수정

## 기타
