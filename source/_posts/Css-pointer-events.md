---
title: pointer-events를 이용한 클릭 방지 및 이벤트 위임
toc: true
date: 2019-08-01 20:37:22
tags:
    - CSS
    - Javascript
categories:
    - CSS
---

## pointer-events

상위 요소에 있지만 클릭 못하게 하는 방법 + 이벤트 버블링 방지

```js
event.preventDefault()
현재 이벤트의 기본 동작을 중단한다.

event.stopPropagation()
현재 이벤트가 상위로 전파되지 않도록 중단한다.

event.stopImmediatePropagation()
현재 이벤트가 상위뿐 아니라 현재 레벨에 걸린 다른 이벤트도 동작하지 않도록 중단한다.

return false
jQuery를 사용할 때는 위의 두개 모두를 수행한 것과 같고,
jQuery를 사용하지 않을 때는&nbsp;event.preventDefault() 와 같다.
```

<p class="codepen" data-height="530" data-theme-id="0" data-default-tab="css,result" data-user="taeuk_kang" data-slug-hash="0aa3bc968286581a7597d50e4782202f" data-editable="true" style="height: 530px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="투명하지만 클릭못하는 DIV">
  <span>See the Pen <a href="https://codepen.io/taeuk_kang/pen/0aa3bc968286581a7597d50e4782202f/">
  투명하지만 클릭못하는 DIV</a> by taeuk_kang (<a href="https://codepen.io/taeuk_kang">@taeuk_kang</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
