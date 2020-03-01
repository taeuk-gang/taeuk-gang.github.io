---
title: Typora 신기능 - 이미지 자동 업로드
toc: true
date: 2020-02-21 02:04:47
tags:
	- 문서화
	- Typora
categories:
	- 문서화
---

## 기능 설명

Typora - Winodow 버전에 신기능이 업데이트됬다. 이전부터 Mac은 있던 기능인데 Window만 없었다. 
이미지를 삽입시, 자동으로 웹 서버에 올려주고 링크를 연결해주는 기능이다.

![너무 좋아!](https://user-images.githubusercontent.com/26294469/74609914-0312cd80-5132-11ea-9aae-74cb65d7571e.gif)

### Typora에서 사용 영상(GIF)

![사용영상](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/사용영상.gif)

## 설정법

### 1. Typora 메뉴 `서식 - 이미지 -  전역 이미지 설정` 으로 들어가기

아래 `Image Upload Setting` 부분이 새로 추가된 기능 메뉴이다.

현재는 이미 셋팅된 상태이지만, 하기 전에는 Install 버튼을 따로 눌러 설치를 해야한다.

그리고 Uploader를 `PicGo-Core`로 설정

![image-20200229230615472](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229230615472.png)

### 2. Open Config File 버튼을 누르고, `config.json` 설정

그럼 `config.json` 파일이 뜰텐데, 현재 아무것도 설정되있지 않은 상태이다.

이미지를 업로드하기 좋은 곳이 어디있을까 생각하다가, `github`가 제일 좋을 것 같아 아래와 같이 설정할 수 있다. (* github repo 만드는 법은 생략)

#### Github용 `config.json` 템플릿

```json
{
  "picBed": {
    "current": "github",
    "github": {
      "repo": "<GITHUB USER_ID/REPO NAME>",
      "token": "<사용자 토큰 - 생성 법은 별도로 아래의 작성해뒀어요~>",
      "path": "img/",
      "customUrl": "https://raw.githubusercontent.com/<GITHUB USER_ID>/<REPO NAME>/image",
      "branch": "master"
    }
  },
  "settings": {
    "showUpdateTip": true,
    "autoStart": true,
    "uploadNotification": true,
    "miniWindowOntop": true
  },
  "needReload": false,
  "picgoPlugins": {}
}
```

#### 예시 `config.json`

```json
{
  "picBed": {
    "current": "github",
    "github": {
      "repo": "taeuk-gang/save-image-repo",
      "token": "<사용자 토큰 - 생성 법은 별도 아래에 작성>",
      "path": "img/",
      "customUrl": "https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image",
      "branch": "image <마스터 커밋에 올리고 싶지않은 사람은 별도 브랜치를 생성>"
    }
  },
  "settings": {
    "showUpdateTip": true,
    "autoStart": true,
    "uploadNotification": true,
    "miniWindowOntop": true
  },
  "needReload": false,
  "picgoPlugins": {}
}
```

이후, `config.json` 저장한 뒤 Typora에서 이미지를 삽입하면 `Upload Image` 버튼이 생긴 것을 확인 할 수 있다!

![커밋 사진](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200301105851396.png)

![폴더 올라간 모습](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200301105914720.png)



---------

### (기타) Github 사용자 토큰 생성법

#### 1. Github setting 들어가기

![image-20200229231913748](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229231913748.png)

#### 2. Developer settings 들어가기

![image-20200229232016404](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229232016404.png)

#### 3. Personal access tokens 들어가기

![image-20200229232041803](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229232041803.png)

#### 4. Generate new token 버튼 클릭하여 토큰 생성!

![image-20200229232136453](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229232136453.png)

설정은 아래와 같이 진행 (P.S 필요없는 권한이 있을 수 있음)

![image-20200229232230130](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229232230130.png)

![image-20200229232245165](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200229232245165.png)

