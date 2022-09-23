---
title: 아키텍처 변천사
date: 2022-09-20 00:00:00
category: 아키텍처
tags:
  - 아키텍처
math: true
mermaid: true
author: 강태욱
marp: true
backgroundColor: #fff
_class: lead
paginate: true
backgroundImage: url('https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20210919162643014.png')
---
<script src="https://unpkg.com/mermaid@8.13.2/dist/mermaid.min.js"></script>
<script>
mermaid.initialize({
  startOnLoad: window.clientInformation.userAgent.includes(`Electron`) ? true : false,
  flowchart: { useMaxWidth: true, htmlLabels: true, curve: 'cardinal' },
  securityLevel:'loose'
});
</script>
<script>
// 버그픽스: mermaid
window.target = document.querySelector("#p");
window.observer = new MutationObserver(function(mutations) {
  mutations.some(function(mutation) {
    if (mutation.target.classList.contains(`bespoke-marp-active`) && mutation.target.querySelector(`.mermaid`)) {
        window.mermaid.init(undefined,document.querySelectorAll(".mermaid"));
        return true;
    }
  });
});
window.config = {
  attributes: true,
  childList: true,
  subtree: true || null,  
}; 
window.observer.observe(target, config);
</script>

<style>
ul, ol, p {
  font-size: 0.75rem;
}
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}

header {
  color: #fff;
  font-size: 0.75rem;
}

footer {
  font-size: 0.75rem;
}

ol li {
  list-style-type: decimal;
}

.mermaid,
.mermaid * {
  overflow: visible;
  font-size: initial;
  text-align: center;
}

.mermaid > svg {
  max-height: 45vh;
}

/* two-colums */

div.twocols {
  margin-top: 35px;
  /* pdf */
  /* margin-top: 250px; */
  column-count: 2;
}

div.threecols {
  margin-top: 35px;
  /* pdf */
  /* margin-top: 250px; */
  column-count: 3;
}

div.twocols p:first-child,
div.twocols h1:first-child,
div.twocols h2:first-child,
div.twocols ul:first-child,
div.twocols ul li:first-child,
div.twocols ul li p:first-child {
  margin-top: 0 !important;
}
div.twocols p.break {
  break-before: column;
  margin-top: 0;
}
</style>

# 아키텍처 변천사

> 아무렇게나 코드를 던져두면 필요할 때마다 찾기가 힘듦

> 그래서 개발자들은 비슷한 코드를 모아 서로 이해할 수 있게 분류하기로 결정
(아키텍처 = 모두가 이해하고 따를 수 있도록 하는 구조)

> 그런데 개발의 시간이 흐르면서 분류 방식에 대한 논쟁 및 트렌드 변경으로 각 아키텍처에 대한 차이를 알고 이해하기 위해 글을 작성

![bg w:300 right center](https://user-images.githubusercontent.com/26294469/74609919-04dc9100-5132-11ea-8c79-d4ff79d5bfde.gif)

---
<!-- header: MVC -->

<div class="mermaid">
flowchart LR
   view --> model
   controller --> model
   controller -->|data| view
</div>

- 화살표의 방향은 의존성을 의미 (ex. view는 model을 알고, model은 view를 모름)
- 기존 mvc의 약점으로 지적된 사항들
  - view 가 model을 알고 있는 것의 문제
    - model = 비즈니스 로직(businness domain)이기 때문에 변동성이 잦음 (화면 표시에 대한 이유와는 다른 변동성)
    - 변화에 대한 이유가 다른데 의존성을 가지고 있음
    - model과 view의 관계가 너무 강한 것이 주로 이야기됨
    - ex. Java Spring (서버에서는 view -> model 의존성이 없기 때문에 적합)

### 구성

- Controller: 사용자의 요청사항을 데이터를 Model에 전달하고, 데이터를 View에 반영
- Model: 데이터를 주관하는 영역, 데이터를 다루는 로직을 모델에 모아두어 view와 격리
- View: 화면상에 출력되는 내용, 클라이언트 측 기술인 html/css/javascript들을 모아둔 컨테이너

---
<!-- header: 변형된 MVC -->

<div class="mermaid">
flowchart LR
   view --> controller
   model --> controller
   controller --> model
   controller -->|data| view
</div>

- 모든 것을 controller가 관리하는 형태
  - 기존 위의 mvc의 view -> model에 대한 의존성이 사라졌지만, controller의 역할이 커짐
  - view와 model이 조금만 변경되도 controller까지 변경이 필요함 (유지보수 및 개발 공수가 큼)

---
<!-- header: MVP -->

<div class="twocols">

<div class="mermaid">
flowchart LR
   view[view - getter,setter ] --> presenter
   model --> presenter
   presenter --> model
   presenter -->|data| view
</div>

- presenter가 view 컴포넌트의 getter, setter를 통해 접근
  - native dom이 아닌 인터페이스로 취급 (model을 주지 않아 로직이 없음) 
  - 모든 기능에 1:1 대응되는 getter, setter 작성 필요
- ex. MFC, Android, visual basic

<p class="break"></p>

```js
var PhotoView = Backbone.View.extend({

    //... is a list tag.
    tagName:  "li",

    // Pass the contents of the photo template through a templating
    // function, cache it for a single photo
    template: _.template( $("#photo-template").html() ),

    // The DOM events specific to an item.
    events: {
      "click img" : "toggleViewed"
    },

    // The PhotoView listens for changes to 
    // its model, re-rendering. Since tHere's
    // a one-to-one correspondence between a 
    // **Photo** and a **PhotoView** in this
    // app, we set a direct reference on the model for convenience.

    initialize: function() {
      this.model.on( "change", this.render, this );
      this.model.on( "destroy", this.remove, this );
    },

    // Re-render the photo entry
    render: function() {
      $( this.el ).html( this.template(this.model.toJSON() ));
      return this;
    },

    // Toggle the `"viewed"` state of the model.
    toggleViewed: function() {
      this.model.viewed();
    }

});
```

</div>

---
<!-- header: MVVM -->

<div class="mermaid">
flowchart LR
   binder --> view
   binder -->|observe| view-model
   view-model --> model
   model --> view-model
</div>

- view-model = view를 대신하는 순수한 데이터 구조체
  - view-model에 변경이 감지됬을 때, view가 자동으로 갱신, 혹은 view에서 이벤트가 발생했을 때 view-model을 변경
  - view-model은 view에 대한 존재를 몰라야함

---

### Binder

- 보통은 아래 2가지 방식이 고려됨, 서로 장단점이 있음 - [참고](https://www.slideshare.net/gyeongseokseo/virtual-dom)

<div class="mermaid">
flowchart LR
   scanner --> binder
   scanner --> Dom
   binder -->|observe| view-model
   view-model --> model
   model --> view-model
</div>

  1. DOM 스캔 방식 (angular, svelte): Binder에서 Scanner 분리
  - model과 view 분리 쉬움
  - DOM에 대한 의존성이 크기 때문에 scanner를 분리 (binder를 보호하기 위함)
  - 변화율(변동에 대한 시간)에 따라 분리
  
  
  2. 자체 DOM([vdom)을 소유 (react)
  - Component 방식(model과 view가 같이 관리 - setState)

---
<!-- header: 참고링크 -->

- [TodoMVC](https://todomvc.com/)
- [Learning JavaScript Design Patterns](https://www.oreilly.com/library/view/learning-javascript-design/9781449334840/ch10s05.html)
- [프론트엔드 아키텍처 트렌드](https://yozm.wishket.com/magazine/detail/1663/)
- [프론트엔드에서-MV-아키텍쳐](https://velog.io/@teo/%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C%EC%97%90%EC%84%9C-MV-%EC%95%84%ED%82%A4%ED%85%8D%EC%B3%90%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94)
- [생활코딩 MVC](https://opentutorials.org/course/697/3828)
- [객체지향 JS](https://www.youtube.com/watch?v=RT38Za1pkdI)
- [Android MVP](https://brunch.co.kr/@oemilk/75)
- [Proxy와 가상 돔을 사용하여 나만의 프레임워크 만들기](https://meetup.toast.com/posts/158)
- [JavaScript Proxy. 근데 이제 Reflect를 곁들인](https://meetup.toast.com/posts/302)
- [프레임워크별 DOM렌더링 전략](https://www.youtube.com/watch?v=FhYefTLiJkE)
- [Book: 프레임워크 없는 프론트엔드 개발 - VanillaJS로 프레임워크의 원리 구현](http://www.yes24.com/Product/Goods/96639825)