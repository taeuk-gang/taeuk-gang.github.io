---
title: 코르도바 개발 일지 (1)
toc: true
date: 2019-09-02 04:37:23
tags:
    - Project
    - Cordova
    - Javascript
categories:
    - Projects
    - mac-tour
---

## 목적

이번에 "스마트 관광 앱개발 공모전" 본선 진출을 하게 되어, 앱개발을 하게됬다.
그런데 순수 안드로이드 개발 경험은 전무하여, 웹기술(폰갭 = 코르도바)을 통한 개발에 도전하고자 한다.

![image](https://user-images.githubusercontent.com/26294469/64081334-7074a280-cd3a-11e9-9bfb-7a0ff5d75acf.png)

## 차례

1. 개발 환경 구축
2. 프로젝트 생성·관리
3. 코드 작성 (코르도바 도입)
4. 안드로이드 스토어 등록

## 개발환경 구축

모든 사항은 [코르도바 공식 DOCS](https://cordova.apache.org/docs/ko/latest/guide/cli/index.html) 에 더 상세히 작성되어있다.

### 1. Node.js 및 NPM 설치

```bash
# 설치 확인
node -v

npm -v
```

설치 방법은 공식 사이트 참고 [(사이트)](https://nodejs.org/ko/download/)



### 2. 코르도바 커맨드라인 인터페이스 설치

```bash
npm install -g cordova
```



### 3. 안드로이드 스튜디오 설치

코르도바는 안드로이드 SDK를 내부에서 이용하므로 설치할 필요가 있다. 마찬가지로 [공식 사이트](https://developer.android.com/studio/install?hl=ko) 참고



### 4. 환경변수 설정

SDK 폴더 내, `tools`, `platform-tools` 폴더 2개를 시스템 환경변수에 추가해준다.
정상적으로 설정되었다면 다음 명령어를 통해 확인

```bash
adb version
```

### 5. Gradle 설치

코르도바 빌드를 위해서는 gradle이 필요하다. [참고 사이트](https://zetawiki.com/wiki/%EC%9C%88%EB%8F%84%EC%9A%B0_gradle_%EC%84%A4%EC%B9%98) 에 따라, 설치를 완료한다.

### 6. 안드로이드 스튜디오 설정

안드로이드 스튜디오에서 각 안드로이드 API버전을 설치하고, 예뮬레이터를 추가해야한다.
테스트 기기에 따른 API 다운 및 Virtual Device 설정

![image](https://user-images.githubusercontent.com/26294469/64081646-d2370b80-cd3e-11e9-8129-3dc4b0f0988e.png)

![image](https://user-images.githubusercontent.com/26294469/64081649-debb6400-cd3e-11e9-9851-a10b773cfda3.png)



## 프로젝트 생성

```bash
cordova create hello com.example.hello HelloCordova -d

# cordova create 폴더명 앱-식별자 프로젝트명 생성과정옵션
```

현재 디렉토리에 `hello` 폴더를 생성하고, 앱 식별자로 `com.example.hello`를 가진 `HelloCordova` 이름의 프로젝트를 생성한다. 라는 의미.

`-d`는 프로젝트의 생성과정을 표시해주는 옵션이다.

### 플랫폼 추가

```bash
cd hello

cordova platform add ios
cordova platform add android
```

ios와 android용 프로젝트 파일을 생성하는 명령이다.

### 플랫폼 확인

```bash
cordova platform ls
```

### 플랫폼 제거

```bash
cordova platform remove ios
cordova platform remove android
```

### 디렉토리 구조

```
📦hello
┣ 📂hooks		# 사용자 스크립트
┣ 📂platforms	# 각 플랫폼 고유 프로젝트 파일(ios, android)
┣ 📂plugins		# 플러그인
┣ ┗ 📂www		# HTML5 리소스, 앱 설정
┣ 📂(merges)	# 각 플랫폼 고유 리소스 (자동생성되지는 않는다)
┣ 📜config.xml	# 프로젝트 설정 파일
```

`config.xml`은 빌드시에 platforms 하위 디렉토리로 각 플랫폼별로 복사된다.

`merges` 폴더는 3.5버전부터는 자동생성되지않지만, 폴더를 만들고 그 아래 원하는 플랫폼 디렉토리를 만들면 된다. `ex. merges/android`

`plugins` 아래는 플러그인과 그 설정 파일을 저장하는 장소이다. CLI 명령어를 통해 관리하기 때문에 수동으로 폴더를 관리하는 일은 적다.

### 안드로이드 시뮬레이터 이용

```bash
# 바로 실행시
cordova emulate android -d
```



### 실제 디바이스에서 프로젝트 실행

디바이스 개발자 옵션 - USB 디버깅 ON

```bash
# 연결된 디바이스가 없으면, 예뮬레이터 자동 실행
cordova run android -d
```

![image](https://user-images.githubusercontent.com/26294469/64085425-99faf180-cd6d-11e9-9ac8-9fdc7321cf7f.png)

