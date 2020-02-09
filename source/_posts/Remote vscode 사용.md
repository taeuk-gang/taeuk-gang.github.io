---
title: VS Code에서 서버 내 파일 작업하기
toc: true
date: 2020-02-10 02:04:47
tags:
	- SSH
	- SERVER
	- VSCODE
categories:
	- SERVER
---

## VS Code에서 서버 내 파일 작업하기

### 요약

1. VSCODE에서 플러그인 `Remote Vscode` 설치
2. 연결할 서버에 서버용 모듈 설치
3. `ssh` 커맨더 명령어 입력 후 연결
4. `rmate` 명령어 사용

#### 1. VSCODE에서 플러그인 `Remote Vscode` 설치

![image](https://user-images.githubusercontent.com/26294469/74106488-cb90a800-4baa-11ea-9504-7084f9681963.png)

#### 2. 연결할 서버에 서버용 모듈 설치

##### 서버 접속 (SSH)

```bash
ssh <유저명>@<서버IP>
```

##### 설치

```bash
sudo wget -O /usr/local/bin/rmate https://raw.githubusercontent.com/aurora/rmate/master/rmate

sudo chmod a+x /usr/local/bin/rmate
```

##### 서버 연결 종료

#### 3. `ssh` 커맨더 명령어 입력후 연결

```bash
# 구조
ssh -L <포트포워딩> -R 52698:127.0.0.1:52698 <유저명>@<서버IP> -i <GCP의 경우, 퍼블릭키>

# 예시
ssh -L 80:0.0.0.0:8080 -R 52698:127.0.0.1:52698 taeuk1kang@34.84.202.87 -i ./taeuk1kang
```

#### 4. `rmate` 명령어 사용

이후, 접속된 서버 터미널에서 커맨드 사용

```bash
rmate <파일경로>

# 예시
rmate ./file.txt
```



#### TIP. 가끔식 `rmate` 명령어 안될 때

```bash
# 52698 포트 사용 확인
lsof -i tcp:52698

# 포트 제거
fuser -k -n tcp 52698

# 이후, 서버 ssh 재접속
```

