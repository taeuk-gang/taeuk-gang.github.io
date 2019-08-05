---
title: Kubernetes 학습 기록 (1) - Docker
toc: true
date: 2019-08-03 19:51:32
tags:
    - Server
    - Kubernetes
categories:
    - Server
    - Kubernetes
---

"Kubernetes in Action" 책으로 학습한 내용입니다.

- ## Hello World 컨테이너 실행　
> ```bash
> docker run busybox echo "Hello world"
> ```
 > |                          결과 화면                           |
 > | :----------------------------------------------------------: |
 > | <img src="https://user-images.githubusercontent.com/26294469/62409937-8dbe3000-b619-11e9-88aa-1e77ea39dd68.png"> |



## 간단한 Node.js 앱 만들기

- ### 컨테이너 이미지 버전 실행법
> ```bash
> docker run <image>:<tag>
> ```



- ### `app.js` 파일 생성으로 애플리케이션 생성
>  ```js
> // app.js 파일
> const http = require(`http`)
> const os = require(`os`)
> 
> console.log(`Kubia server starting...`)
> 
> var handler = (req, res) => {
>     console.log(`Received request from ${req.connection.remoteAddress}`)
>     res.writeHead(200)
>     res.end(`You've hit ${os.hostname()}`)
> }
> 
> var www = http.createServer(handler)
> www.listen(8080)
>  ```



- ### 이미지용 도커 파일 만들기
> 이미지에 패키징하려면 `도커 파일` 필요
>
> `도커 파일`에는 도커가 수행할 지시 사항 목록이 들어있다.
>
> `도커 파일`은 동일한 디렉토리에 위치할 것!
>
> ```dockerfile
> FROM node:10
> ADD app.js /app.js
> ENTRYPOINT ["node", "app.js"]
> ```



- ### 컨테이너 이미지 만들기
> ```bash
> docker build -t taeuk-test .
> ```
    > 도커에게 현재 디렉터리의 내용을 `taeuk-test`라는 이미지로 빌드하라는 요청
    >
    > 도커파일을 찾고 설정에 따라 이미지 빌드
    >
    > (대문자는 되지않음)
    > |                          결과 화면                           |
    > | :----------------------------------------------------------: |
    > | <img src="https://user-images.githubusercontent.com/26294469/62410381-ab8e9380-b61f-11e9-8cd8-39d84f7ab3f8.png"> |



- ### 빌드 프로세스가 완료되면, 새 이미지가 로컬에 저장됨
> ```bash
> docker images
> ```
    > |                          결과 화면                           |
    > | :----------------------------------------------------------: |
    > | <img src="https://user-images.githubusercontent.com/26294469/62410407-de388c00-b61f-11e9-99f9-59298c60e1ad.png"> |



- ### 도커 이미지 실행
    > ```bash
    > docker run --name taeuk-container -p 9381:8080 -d taeuk-test
    > ```
    >
    > 컨테이너는 콘솔에서 분리된다. (백그라운드에서 실행)
    >
    > `-p 8080:8080` 로컬 컴퓨터의 8080은 컨테이너 내부 포트 8080에 매핑
    > 이후, http://localhost:8080을 통해 접속이 가능하다.
    >
    > *도커 데몬이 아닌 경우(Mac or Window는 VM 내부에서 데몬실행), VM의 호스트 네임이나 IP를 사용



- ### (이슈) 이미 사용된 포트일 경우, exist 상태이지만, running은 아니라서 컨테이너가 `docker ps` 명령어에도 뜨지 않는다.
> |  이미 사용중인 포트라고 해서, 다시 로컬 포트를 바꿨지만 이미 생성되있다고 뜬다  |
> | :----------------------------------------------------------: |
> | <img src="https://user-images.githubusercontent.com/26294469/62410660-a3d0ee00-b623-11e9-969b-502c682a2ea6.png"> |
>
>   `docker kill`을 실행 중이 아니라고 지워지지 않는다.
>
>   그러므로, `docker rm`을 하여 지운 후 다시 `docker run`
>
> |                          결과 화면                           |
> | :----------------------------------------------------------: |
> | <img src="https://user-images.githubusercontent.com/26294469/62410679-2063cc80-b624-11e9-81a9-d70e8df81cfd.png"> |



- ### 제대로 작동하고 있는지 확인, `curl localhost:port`
> |                          결과 화면                           |
> | :----------------------------------------------------------: |
> | <img src="https://user-images.githubusercontent.com/26294469/62410692-72a4ed80-b624-11e9-8a9e-ca913d689aeb.png"> |
>
> `hit`가 뜬다면 정상
>
> 만약 `curl: (56) Recv failure: Connection reset by peer`가 뜬다면,
> 포트(외부포트:컨테이너내부포트 둘중 하나가 맞지 않다는 뜻이다)

- 또는 `docker inspect taeuk-container` 를 쳐서 확인
>
> |                          결과 화면                           |
> | :----------------------------------------------------------: |
> | <img src="https://user-images.githubusercontent.com/26294469/62410711-f363e980-b624-11e9-8feb-71160aad3abb.png"> |
>

- 이후, 터널링
>
> ```bash
> ssh -l 9381:127.0.0.1:9381 root@101.101.164.175
> ```
    > 명령어 친후, 내 컴퓨터에서 `localhost:9381`로 접속하여 확인



- ### 실행 중인 컨테이너 내부 탐색
    > ```bash
    > docker exec -it taeuk-container bash
    > ```
    >
    > `-i`:  stdin을 오픈상태로 유지, 셸에 명령 입력시 필요
    >
    > `-t`: pseudo 터미널(TTY)을 할당한다.
    >
    > ```bash
    > ps aux
    > ```
    > 컨테이너 내부 프로세스 나열하기
    > |                          결과 화면                           |
    > | :----------------------------------------------------------: |
    > | <img src="https://user-images.githubusercontent.com/26294469/62412270-da663300-b63a-11e9-9dd5-3f6d448fabbe.png"> |
    >
    > 컨테이너 내부 프로세스와 호스트내부 프로세스 ID가 다르다.
    >
    > 컨테이너는 자체 PID 리눅스 네임스페이스를 사용
    >
    > 고유 시퀀스 번호를 갖는 완전히 분리된 프로세스 트리를 소유한다.

- ### 컨테이너 중지 및 제거
  > ```bash
  > docker stop taeuk-container
  > ```
  >
  > 실행중인 기본 프로세스 중지, 컨테이너 중지
  >
  > 컨테이너 자체는 `docker ps -a` 명령어를 통해 확인 가능
  >
  > ```bash
  > docker rm taeuk-container
  > ```
  >
  > 컨테이너가 삭제되고, 모든 내용이 제거되면 다시 시작할 수 없다.

- ### 이미지 레지스트리로 이미지 푸시
  > 지금까지 한 작업은 로컬에서만 사용가능하다. 다른 컴퓨터에서 빌드한 이미지를 실행하려면, 외부 이미지 레지스트리로 푸시해야한다. 공개적으로 이용가능한  레지스트리 중 하나인 [도커 허브](http://hub.docker.com)에 이미지 푸시, 다른 것으로는 `Quay.io`, `구글 컨테이너 레지스트리`가 있다.
  >
  > 이미지 푸시 전에, 도커 허브 RULE에 따라 이미지 태그부터 지정해야한다.
  > 도커 허브는 이미지의 소티리지 이름이 도커 허브 ID로 시작하면 이미지를 푸시할 수 있다.
  > 도커 허브 ID는 도커허브에서 등록하여 만든다.
  >
  > ```bash
  > docker tag taeuk-test kangtaeuk/taeuk-test
  > ```
  >
  > 이후, `docker images | head` 명령어로 태그가 생성된 것을 확인할 수 있다.
  >
  > |                          결과 화면                           |
  > | :----------------------------------------------------------: |
  > | <img src="https://user-images.githubusercontent.com/26294469/62412446-8f99ea80-b63d-11e9-97bf-c3412b203c7b.png"> |
  >
  > ```bash
  > docker login
  > ```
  >
  > 도커에 푸시하기전에, 로그인을 먼저 한다.
  >
  > ```bash
  > docker push kangtaeuk/taeuk-test
  > ```
  >
  > 도커에 이미지 푸시
  >
  > |                      터미널 - 결과 화면                      |
  > | :----------------------------------------------------------: |
  > | <img src="https://user-images.githubusercontent.com/26294469/62422249-4b185880-b6ea-11e9-8783-64df3b25e365.png"> |
  >
  > |                    도커사이트 - 결과 화면                    |
  > | :----------------------------------------------------------: |
  > | <img src="https://user-images.githubusercontent.com/26294469/62422260-66836380-b6ea-11e9-9013-5f12310c82a1.png"> |
  >
  > #### 이후, 다른 머신에서 이미지를 실행
  >
  > ```bash
  > docker run -p 9381:8080 -d kangtaeuk/taeuk-test
  > ```

## Docker의 장점

애플리케이션이이 언제 어디서나 동일한 환경을 유지한다는 것이다.

모든 컴퓨터 안 리눅스에서 정상 실행이 되기 때문에, 호스트 시스템에 `node.js`가 있는지는 걱정할 필요가 없어진다.