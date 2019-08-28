---
title: Mutation Observer 잘 쓰이는 코드 기록
toc: true
date: 2019-08-28 22:25:04
tags:
    - Javascript
categories:
    - Javascript
---

# Mutation Observer 코드

```js
const observer = new MutationObserver(mutations => {
  mutations.forEach(mutation => {
    console.log(mutation)
  })
})

observer.observe(document.querySelector(`#div-exam`), {
  attributes: true,
  childList: true,
  characterData: true,
  subtree: true || null,
  attributeOldValue: true || null,
  characterDataOldValue: true || null,
})

document.querySelector(`#attr`).addEventListener(`click`, () => {
  target.setAttribute(`class`, `attr`)
})

document.querySelector(`#child`).addEventListener(`click`, () => {
  document.querySelector(`#div-exam`).textContent = `changed!`
})
```

## 나중에, 이걸 써서 프로젝트를 다시 해보자
