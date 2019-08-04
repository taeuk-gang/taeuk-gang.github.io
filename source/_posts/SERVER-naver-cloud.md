---
title: 처음 써보는 네이버 클라우드 이용기
toc: true
date: 2019-08-03 08:00:48
tags:
    - Server
    - Cloud
    - Naver-Cloud
categories:
    - Server
    - Cloud
---

## 서버생성

1. 네이버 클라우드 플랫폼 `console`페이지 로 이동 - [사이트](https://www.ncloud.com/)

   > ![1562329594551](https://user-images.githubusercontent.com/26294469/60750485-475abe80-9fe4-11e9-8120-16ab24087254.png)

2. 이후, All Products - Server 메뉴를 클릭하여, `+서버 생성`메뉴 선택

   > ![1562329675973](https://user-images.githubusercontent.com/26294469/60750487-475abe80-9fe4-11e9-8655-ecbcb2ab4081.png)

3. 아코디언 사용을 위한 마스터 서버 생성

   > **권장사양**
   > ![1562329730480](https://user-images.githubusercontent.com/26294469/60750488-475abe80-9fe4-11e9-8fce-632d7af65597.png)

   > 그러나, 네이버 클라우드 플랫폼은 Redhat을 지원하지않고, CentOS 7.3이 최대버전이기 때문에 CentOS 7.3을 일단 설치 후, 리눅스 상에서 업그레이드를 거치고 이미지를 생성하기로 결정
   >
   > ![1562329810471](https://user-images.githubusercontent.com/26294469/60750490-47f35500-9fe4-11e9-9a6c-5cf79b148187.png)

   > 서버 이미지 선택창 입력 후, 다음 버튼 클릭
   >
   > ![1562329916038](https://user-images.githubusercontent.com/26294469/60750491-47f35500-9fe4-11e9-8b8f-8620bf3458b3.png)

   > 기존 인증키가 있다면, 인증키 선택 후 다음
   >
   > - 없다면, 생성 - 과정이 간단함
   > - 이것은 이후 SSH 서버 접속시, 비밀번호 생성할 때 사용됨
   >
   > ![1562329956537](https://user-images.githubusercontent.com/26294469/60750493-488beb80-9fe4-11e9-94c5-dee5718f554b.png)

   > 네트워크 접근 설정 후, 다음 버튼
   > ![1562329983574](https://user-images.githubusercontent.com/26294469/60750494-488beb80-9fe4-11e9-896e-9ec1f27b61aa.png)

   > 최종 확인 후, 서버 생성 끝
   > ![1562330003973](https://user-images.githubusercontent.com/26294469/60750495-488beb80-9fe4-11e9-80df-2a46f898105c.png)

   > 이후, 생성 중 > 설정 중 > 부팅 중 > 운영 중 순으로 5분 정도 가량 시간이 소모된다.

## 서버 설정

#### 공인 IP 생성

- 메뉴 중, `Public IP` 클릭
> ![1562330124403](https://user-images.githubusercontent.com/26294469/60750498-49248200-9fe4-11e9-8184-07c560b1679e.png)

- 공인 IP 신청 클릭
> ![1562330151907](https://user-images.githubusercontent.com/26294469/60750500-49bd1880-9fe4-11e9-944b-0b84e0c44cb7.png)

- 이후, 처리 절차를 따라 생성 - 서버당 1개의 IP만 소유 가능
> ![1562334105911](https://user-images.githubusercontent.com/26294469/60750505-4a55af00-9fe4-11e9-9f0c-82837c6fc358.png)

- 다시 Server 메뉴로 이동 후, 서버 이미지 오른쪽 클릭 후, `공인 IP 설정 변경` 클릭
> ![1562330256447](https://user-images.githubusercontent.com/26294469/60750501-49bd1880-9fe4-11e9-8079-12d5db887be3.png)

- 마지막으로, 터미널을 통하여 SSH 접속
> ![1562330387218](https://user-images.githubusercontent.com/26294469/60750503-49bd1880-9fe4-11e9-9558-3389da88e70f.png)

## 커널 업데이트

```bash
// 현재 커널 릴리즈 확인
cat /etc/redhat-release

// 최신 버전 업데이트
yum update -y
```

## 내 서버 이미지 생성

- 해당 서버 오른쪽 클릭 후, `내 서버 이미지 생성` 클릭 후 진행
> ![1562332709720](https://user-images.githubusercontent.com/26294469/60750504-49bd1880-9fe4-11e9-933c-d80204b55fef.png)

### 비밀번호 로그인 방식이 아닌, publickey 방식으로 전환

1. 서버에서 `ssh-keygen`으로 비밀키/공유키 생성

2. 생성된 공개키를 서버에 등록 `cat 키-이름.pub >> ~/.ssh/authorized_keys`

3. 권한 변경
```
chmod 700 ~/.ssh # 소유자만 해당 디렉토리 읽기, 쓰기, 접근 가능
chmod 600 ~/.ssh/authorized_keys # 소유자만 해당 파일 읽기, 쓰기 가능
```

4. 이후 서버 설정 변경
> `vi /etc/ssh/sshd_config` 입력하여, 아래와 같이 설정 (패스워드 사용없이 공개키로만 접속하게 설정)
```
PubkeyAuthentication yes
PasswordAuthentication no
```

5. 서버 재설정 시작
`systemctl restart sshd`

6. 접속하고자하는 환경에서 `ssh root(또는 사용자명)@IP -i ./공개키 파일`
