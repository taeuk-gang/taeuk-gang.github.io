---
title: PWA-Starter-Kit 요약
toc: true
date: 2019-10-25 21:16:18
tags:
  - Javascript
categories:
  - Javascript
---

## PWA-Starter-Kit 감상문

### `router.js`

```js
// IMPORT
import { installRouter } from 'pwa-helpers/router.js';

// ROUTING
installRouter((location) => handleNavigation(location));
```

```js
// IMPORT
import { installRouter } from 'pwa-helpers/router.js';
import { navigate } from '../actions/app.js';

// ROUTING
installRouter((location) => store.dispatch(navigate(location)));
```

```js
window.history.pushState({}, '', '/new-route');
handleNavigation(window.location);
```

```js
installRouter((location, event) => {
  // Only scroll to top on link clicks, not popstate events.
  if (event && event.type === 'click') {
    window.scrollTo(0, 0);
  }
  handleNavigation(location);
});
```



### `network.js`

```js
import { installOfflineWatcher } from 'pwa-helpers/network.js';

installOfflineWatcher((offline) => {
  console.log('You are ' + offline ? ' offline' : 'online');
});
```

네트워크 이벤트 리스너가 있지만, 굳이 쓰는 이유는 모르겠넴
하지만 좋은게 좋은거니 쓴다.

### `metadata.js`

```js
import { updateMetadata } from 'pwa-helpers/metadata.js';

updateMetadata({
  title: 'My App - view 1',
  description: 'This is my sample app',
  url: window.location.href,
  image: '/assets/view1-hero.png'
});
```

메타데이터 수정, 유용하네

### `media-query.js`

```js
import { installMediaQueryWatcher } from 'pwa-helpers/media-query.js';

installMediaQueryWatcher(`(min-width: 600px)`, (matches) => {
  console.log(matches ? 'wide screen' : 'narrow sreen');
});
```

오, 미디어 쿼리에 따른 자바스크립트 사용은 정말 좋다

### `axe-report.js`

```js
import 'axe-core/axe.min.js';
import { axeReport } from 'pwa-helpers/axe-report.js';

describe('button', function() {
  it('is accessible', function() {
    const button = document.createElement('button');
    button.textContent = 'click this';  // Test should fail without this line.
    return axeReport(button);
  });
});
```

Test Code 작성법이구만...

### `connect-mixin.js`

```js
import { connect } from 'pwa-helpers/connect-mixin.js';

class MyElement extends connect(store)(HTMLElement or LitElement) {
  stateChanged(state) {
    this.textContent = state.data.count.toString();
  }
}
```

엘리먼트에 걸어둔 this.value값을 리덕스값과 연동하는 것이 좋네

### `lazy-reducer-enhancer.js`

```js
import { combineReducers } from 'redux';
import { lazyReducerEnhancer } from 'pwa-helpers/lazy-reducer-enhancer.js';
import someReducer from './reducers/someReducer.js';

export const store = createStore(
  (state, action) => state,
  compose(lazyReducerEnhancer(combineReducers))
);
```

```js
store.addReducers({
  someReducer
});
```

