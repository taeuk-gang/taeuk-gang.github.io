---
title: 브라우저 확장 프로그램 개발 시작하기
toc: true
date: 2021-01-31 19:43:18
tags:
    - Browser_Extension
categories:
    - Browser_Extension
secret: true
---

# 브라우저 확장 프로그램 개발 시작하기

![크롬 스토어](https://i.loli.net/2021/01/31/7zYgsq9xyUhNw1M.png)

![웨일 스토어](https://i.loli.net/2021/01/31/e1SpMhv9nWZAoaY.png)



## 글쓰기에 앞서...

남에게 도움을 되는 글을 쓰기 이전에, 제가 도움이 줄 수 있는 것이 어떤게 있을까 곰곰히 생각해봤습니다. 제가 학부생 시절에 출시한 프로젝트 중에 웨일 확장 프로그램으로 PDFasT, 네이버 증권 확장앱, 나만의 즐겨찾기 등 출시한 경험이 있고 국내에 이것에 관한 도서나 글이 별로 없다는 생각이 들어 확장앱 개발에 관한 글을 작성해보려고 합니다.

한번에 전부 작성할 생각은 없고 일주일에 1편씩 간단하게 작성해보려고 합니다.



## 글 작성 항목으로 예정중인 사항

> 작성 예정으로 생각하고 있는 사항으로, 언제든지 변경될 수 있습니다.

|                | 예정 사항                                                    |
| -------------- | ------------------------------------------------------------ |
| 1주차          | 1주차: 일반 웹 개발과의 차이점<br />- 브라우저 API<br />- 매니페스트<br /> - 콘텐츠 <> 백그라운드 통신 |
| 2주차          | 환경설정 (+ Typescript)<br />- 프레임워크(종속성X) 없이 구현할 예정입니다. |
| 3주차          | 템플릿 만들어보기                                            |
| 4주차          | 확장앱의 종류 3가지                                          |
| 5주차          | 매니페스트 설정<br />- 필수적으로 알아아할 설정<br />- 부가적인 설정은 부록으로 빼기 |
| 6주차          | 개발 영역<br />- 콘텐츠 스크립트<br />- 백그라운드 스크립트<br />- 확장앱 페이지 |
| 7주차 ~ 11주차 | 필수적으로 알아야할 브라우저 API<br />- 브라우저 액션<br />- 이벤트<br />- 익스텐션<br />- i18n - 다국처리<br />- 페이지 액션<br />- 권한<br />- 런타임<br />- 스토리지<br />- 탭 <br />- 윈도우 |
| 8 ~ 12주차     | 선택적으로 알면 좋은 브라우저 API<br />- 알람<br />- 북마크<br />- 단축키<br />- 컨텍스트 메뉴<br />- 쿠키 제어<br />- 캡처<br />- 인터넷 사용기록<br />- 콘텐츠 제어<br />- 다운로드<br />- 히스토리<br />- 알람<br />- 주소창<br />- 북마크<br />- 웹요청 조작 |
| 13주차         | 테스트와 디버깅                                              |
| 14주차         | 배포하기                                                     |



## 글 작성 방식에 관한 생각

저는 모든 개발에서 가장 중요한 것은 DEMO라고 생각합니다. DEMO가 없으면, 이 사람이 정말로 구현하고 있는지에 대한 의심이 들기 시작하고 그것은 글에 대한 의심으로 이어질 것입니다. 그래서 각 작성항목마다 DEMO로 ZIP 파일을 올리려고 생각 중입니다. 

일반 웹 개발 형태였으면, URL제공이었겠지만 확장앱으로 일반적인 설치는 ZIP으로 이루어집니다.
실제 배포 스크립트도 버전 업데이트와 함께 ZIP 파일을 생성하기도 하고요.



## 1주차: 일반 웹 개발과의 차이점

> 확장앱이 무엇인지 아는 사람들은 안읽으셔도 됩니다.

확장앱은 브라우저에 설치하여 기능을 확장하는 개념의 애플리케이션입니다. 일반 웹 개발과 기술 스택은 동일하지만, 사용자가 권한을 허락한다면 브라우저 API를 이용하여 일반적인 웹에서 못하는 것들을 할 수 있다는 점이 있습니다.

예를 들면, 사람들이 많이 애용하는 광고차단 확장앱 AdBlock의 경우는 콘텐츠 스크립트 조작을 통해 사용자가 현재 보고있는 웹 페이지에서 광고를 제거합니다.

이런 사용자 화면만이 아닌 백그라운드에서 웹요청을 조작하는 Allow CORS, Postman 등, 또는 행아웃처럼 백그라운드에서 채팅을 할 수 있는 확장앱, Redux DevTools처럼 개발자 도구의 기능을 추가하는 등 다양한 확장앱들이 많습니다.



또한, 확장앱 개발 영역 중 콘텐츠 스크립트는 사용자가 현재 보고있는 페이지내의 조작을 할 수 있습니다. 확장앱을 무분별하게 설치시에 보안 우려가 되는 사항이기도 합니다. 예를 들면, 마우스 우클릭 해제 확장앱인척 사용자의 비밀번호를 가로챌 수도 있을 가능성도 있습니다. 일반적인 웹의 경우 HTTPS 보안 연결 및 도메인 검사로 안전한 사이트인지 검사할 수 있는 반면, 확장앱은 더 할 수 있는 범위가 넓기 때문에 사용자의 정보의 위험도가 더 큽니다.

대표적으로, 크롬 확장앱이 정상적인 기능을 하다가, 업데이트를 하고 나서 아래의 이미지를 띄우면서, 크롬 보안 업데이트를 하는 척하는 악성 프로그램이 많습니다.

![image-20210131210950904](https://i.loli.net/2021/01/31/eHuYpcP2gMhAl9t.png)







기타 주의점으로는, 브라우저에서 표시한 호환성이 다를 수 있습니다. 예를 들면, customElements API가 Chrome 54부터 추가되어 Polyfill없이 사용할 수 있을 것 같지만 Polyfill이 필요합니다.



일반 웹개발과 다른 점을 크게 나누자면 아래의 3가지가 있습니다.



### 차이점1. 브라우저 API

> [크롬 브라우저 API](https://developer.chrome.com/docs/extensions/reference/) 에 접속하면, 브라우저 확장 기능으로 제공되는 API를 볼 수 있습니다.

위의 링크를 참고하여 일반적인 웹 API가 아닌 브라우저 API만의 기능을 확인하고 어떤 것을 추가적으로 개발할 수 있는지 확인을 권장합니다. 그리고 브라우저 API를 이용하기전에 API Docs를 참고하여 필요 권한이 무엇인지 파악하고, 매니페스트 파일에 추가 하는 것이 중요합니다. 권한은 설치시에 명시됩니다.

![1. 필요 권한 파악하기](https://i.loli.net/2021/01/31/31wzSMHYIGCiVPZ.png)



### 차이점2. 매니페스트

> 해당 [Chrome Developer](https://developer.chrome.com/docs/extensions/mv2/g etstarted/) 에서 관련사항을 확인할 수 있습니다.

```json
{
    // 필수사항
    "manifest_version": 2,
    "name": "__APP_NAME__",
    "version": "1.0.0",

    // 확장앱의 종류를 아래 옵션 중 하나만 선택하여 설정 가능
    "browser_action": {...},
    "page_action": {...},
  
    // 추천 작성사항
    "description": "__APP_DESCRIPTION__",
    "icons": {...},
    "default_locale": "ko",
    "background": {
        "scripts": [...],
        "page": "..."
    },
    "content_scripts": [...],
    "permissions": [...],
    ...
}
```

매니페스트 파일은 확장앱 개발시에 가장 먼저 확인되는 사항으로 내용이 올바르지 않으면 확장앱 실행이 되지 않습니다. 앱의 기본적인 정보, 확장앱의 종류, 아이콘, 스크립트 파일, 권한 등을 명시합니다.

`__APP_NAME__`, `__APP_DESCRIPTION__` 같은 사항은 i18n 브라우저 API를 이용한 다국어 처리 사항으로, ko.json, en.json 파일에 따라 입력이 됩니다. 이것은 매니페스트 파일뿐만이 아닌 애플리케이션 내 파일에서도 적용할 수 있습니다.



### 차이점3. 콘텐츠 <> 백그라운드 통신

> 해당 관련 [MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Content_scripts#communicating_with_background_scripts) 에서 관련사항을 확인할 수 있습니다.

확장앱 개발은 아래의 3가지로 개발 영역이 나뉩니다.

1. 콘텐츠 스크립트
2. 백그라운드 스크립트
3. 확장앱 페이지 (index.html)

그리고 콘텐츠 스크립트에서는 브라우저 API가 제한됩니다. 

![출처: Whale Developer](https://i.loli.net/2021/01/31/4tNSndY8Tc96roI.png)

콘텐츠 스크립트 개발시의 주의사항은 실제 웹페이지 로드시에 불러들이기 때문에 성능에 영향을 줄 수 있습니다. 무분별한 확장앱 설치가 브라우저 성능 감소의 원인이 되기도 합니다.

예를 들어, 콘텐츠 스크립트를 조작하는데 Jquery를 썼고 모든 페이지의 권한을 주었다고 한다면, 이제부터 모든 웹페이지를 접속할때마다 Jquery를 불러오게 됩니다.

그래서, 콘텐츠 스크립트 개발시에는 가벼운 작업을 권장하고 필요하다고 하다면 백그라운드 스크립트에서 작업 후, 통신을 통해 콘텐츠 스크립트에 반영되게 구현을 하는 것이 좋습니다.

통신에 관한 자세한 개발 사항은 다음 문서에서 작성하기로 하겠습니다.



## 마지막으로...

처음 글을 작성하여 상당히 두서없이 작성하게 되었는데 피드백 남겨주시면 적극 반영하겠습니다.

그리고 다음 문서부터는 실제 개발과 함께 글을 작성해보려고 합니다.



## 도움이 되는 글들

- [Chrome Developer](https://developer.chrome.com/docs/extensions/mv3/getstarted/)

- [Whale Developer](https://developers.whale.naver.com/getting_started/)
- [Browser API MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Gecko/Chrome/API/Browser_API)
