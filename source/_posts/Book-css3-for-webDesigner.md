---
title: 웹디자이너를 위한 CSS3 도서 요약
toc: true
date: 2019-07-25 20:51:45
tags:
    - Book
categories:
    - Book
---

## 다시 보게된 CSS 속성들

### `text-shadow`

```css
p {
    text-shadow: 1px 1px 2px #999;
}
```

### 다중 배경 이미지

```css
body {
    background: url(image.png) no-repeat top left,
    url(image2.png) repeat-x bottom left,
    url(image3.png) repeat-y top right;
}

/* 또는 투명도를 이용한 방법 */
body {
    background: url(image.png) repeat-x fixed -130% 0,
    url(image2.png) repeat-x fixed 40% 0,
    url(image3.png) repeat-x fixed -80% 0;
}
```



### 트랜지션 지연

```css
a.foo {
    transition-property: background;
    transition-duration: 03s;
    transition-timing-function: ease;
    transition-delay: 0.5s;
}

/* 축약 표기 */
a.foo {
    transition: background 0.3s ease 0.5s;
}
```





## 브라우저 개발사 접두어

|  개발사   |  접두어  |
| :-------: | :------: |
|   Apple   | -webkit- |
|  Google   | -webkit- |
|  Mozilla  |  -moz-   |
|   Opera   |   -o-    |
| Konqueror | -khtml-  |
| Microsoft |   -ms-   |



## 애니메이션을 왜 자바스크립트로 사용하지 않나?

`Jquery`, `Prototype`, `script.aculo.us`, 그리고 요즘은 `anime.js`같은 프레임워크에서 에니메이션이 사용가능합니다. 그러나, 웹사이트 레이아웃에서 에니메이션은 개발 최소단위로 들어가진 않기에 단순히 CSS 코드 몇줄로 간단히 구현하는 것이 현명한 처사로 보입니다.



## 책에는 없지만 나름 유용한 CSS 속성들

### 

### `object-fit`, `object-position`

`<img />` 태그에서 `background-size`, `background-position` 속성과 같이 사용할 수 있는 CSS 속성