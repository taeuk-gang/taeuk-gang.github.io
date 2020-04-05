---
title: Javascript 성능 최적화
toc: true
date: 2020-03-03 02:04:47
tags:
	- Javascript
	- Optimize
categories:
	- Javascript
---

## 목적

이번 프로젝트에서 체감 성능은 느리지 않지만, Audit - Performance 가 좋지 않았다.

체감상으로 작업 기준을 맞추기는 힘드므로, Performance의 수치 중 하나가 일정 이상일까지는 성능 최적화를 하기로 결심했다.

쿠팡에서는 `TTI`가 2초를 넘기지 않게 노력한다고 한다. 그래서 나도 `TTI` 2초 미만으로 떨어질 때까지 최적화를 해보기로 결심했다.

![image-20200303201616551](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200303201616551.png)

## 진행사항

### node_modules 제거 후, 재설치





## 고려사항

- `polyfill.js` 파일 IE에 따른 선택적 삽입 (라우팅 쪽을 봐야하나?)
- 



## 참고링크

- [프론트엔드 성능 최적화](https://www.slideshare.net/NHNFORWARD/2018-130108045)
- [구글 - 자바스크립트 성능 최적화](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/javascript-startup-optimization?hl=ko)

- [TOAST UI - 성능 최적화](https://ui.toast.com/fe-guide/ko_PERFORMANCE/)

- [구글 - TTI 관련 문서](https://web.dev/interactive/?utm_source=lighthouse&utm_medium=devtools)

- [Audit - web.dev](https://web.dev/lighthouse-performance/)

- [Chrome - lighthouse](https://github.com/GoogleChrome/lighthouse/blob/d2ec9ffbb21de9ad1a0f86ed24575eda32c796f0/docs/scoring.md#how-are-the-scores-weighted)

- [Eliminate render-blocking resources](https://web.dev/render-blocking-resources/?utm_source=lighthouse&utm_medium=devtools)

- [Extract critical CSS](https://web.dev/extract-critical-css/)