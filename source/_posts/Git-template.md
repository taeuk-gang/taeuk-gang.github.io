---
title: Git 이슈·PR 템플릿 등록
toc: true
date: 2019-08-25 21:43:45
tags:
    - Git
categories:
    - Git
---

# 이슈 템플릿 등록 방법

## Git UI를 이용한 방법

1. Git Repo 메뉴 중 Insight - Community 메뉴를 클릭
2. CheckList 중  Issue templates Add 버튼을 클릭
3. 이슈별 항목들을 만든다. (기본값으로, Bug, Feature 항목이 존재한다.)
   - 원하는 항목을 만들 때는, custom template를 클릭
4. 이슈 작성하기를 눌러 확인



## Git Push를 이용한 방법

1. 레포 최상단 폴더 위치에  `.github/ISSUE_TEMPLATE` 폴더를 생성

2. `ISSUE_TEMPLATE` 폴더 내에 README 작성

### 양식 예시

```markdown
---
name: Bug report
about: Create a report to help us improve
title: "제목"
labels: bug
assignees: ''
---

## 버그 내용
 
## 재현 과정
 
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

## 우려 사항

## 스크린샷 
 
## 환경(Desktop)
- OS: [e.g. iOS]
- Browser [e.g. chrome, safari]
- Version [e.g. 22]
 
## 환경(Mobile)
- Device: [e.g. iPhone6]
- OS: [e.g. iOS8.1]
- Browser [e.g. stock browser, safari]
- Version [e.g. 22]
 
## 기타
```

3. master에 push하고 이슈를 작성하여 확인

## 상세 스크린샷 과정

### Git UI를 이용

| 이름  | 이미지                                                       |
| ----- | ------------------------------------------------------------ |
| 과정1 | ![image](https://user-images.githubusercontent.com/26294469/63650306-3167af80-c784-11e9-979d-0f4ee6d4edae.png) |
| 과정2 | ![image](https://user-images.githubusercontent.com/26294469/63650325-712e9700-c784-11e9-90ee-4d3802444503.png) |
| 과정3 | ![image](https://user-images.githubusercontent.com/26294469/63650334-89061b00-c784-11e9-8825-86bbc22899b5.png) |

-----------

# PR 템플릿 등록 방법

Git Repo에서 PR 템플릿 등록은 지원하지 않는다. 그러므로, Git Repo에 특정 파일을 Push 해야한다.

1. Git Repo 최상단 위치에 `.github` 폴더 생성
2. `.github` 폴더 내에 `PULL_REQUEST_TEMPLATE.md` 이름으로 파일을 생성한다. (양식은 자유)
3. Git Repo로 PR을 날려, 양식이 적용됬는지 확인한다.