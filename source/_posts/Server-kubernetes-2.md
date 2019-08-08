---
title: Kubernetes 학습 기록 (2) - Kubernetes
toc: true
date: 2019-08-04 21:45:42
tags:
    - Server
    - Kubernetes
categories:
    - Server
    - Kubernetes
---

"Kubernetes in Action" 책으로 학습한 내용입니다.
1장에 이어서... 2장 시작

## 쿠버네티스 클러스터 설정

1장에서 도커 이미지에 패키지를 만들고 도커 허브를 통해 사용할 수 있게 됬다.
이제 도커에서 직접 실행하는 대신, 쿠버네티스 클러스터에 배포를 하고 실행해보자.

소개하는 3가지 방법

* 로컬에서 단일 노드 쿠버네티스 클러스터 실행
* 구글 쿠버네티스 엔진에서 호스트받은 클러스터에서 실행 (추후 작성)
* `kubeadm` 툴을 사용해 설치하는 방법 (추후 작성)

<br />

### 미니큐브를 사용해 로컬 단일 노드 쿠버네티스 실행

미니큐브 = 쿠버네티스를 실행하고 로컬에서 어플리케이션을 개발하는데 유용한 도구
여러노드를 관리할 수는 없음, 단일 노드 개발에 특화

#### 미니큐브 설치

OSX, 리눅스, 윈도우 지원 [(설치법)](https://github.com/kubernetes/minikube) 

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.23.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62431658-23fe6d00-b764-11e9-8ad1-e376a0a316e2.png"> |

#### 미니큐브 시작

```bash
minikub start
```

클러스터를 시작하는데 1분 이상 소모되므로, 대기



#### 쿠버네티스 설치

OSX 설치 (리눅스 또는 윈도우는 `darwin`을 `linux` 또는 `windows`로 변경)

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```



#### 클러스터 작동 확인 / `kubectl`로 명령
```bash
kubectl cluster-info
```
|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62424131-9b50e400-b705-11e9-9340-718d76feeadb.png"> |



#### (선택) `alias`와 `kubectl` 설정
`kubectl`을 자주 치기 귀찮을 때, 설정법

로컬내에 `/root/.bashrc` 파일에 `alias k='kubectl'`을 추가하여 사용
이후 `k cluster-info`등으로 `kubectl`을 대체가능하다.



#### 클러스터 노드 목록 확인
```bash
k get nodes
```
|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62424496-08ff0f00-b70a-11e9-9510-ccdffca1feb9.png"> |


## 쿠버네티스에서 Node App 실행

### Node.js App 배포
```bash
k run taeuk-test --image=kangtaeuk/taeuk-test --port=9381 --generator=run-pod/v1
```
`run`은 deprecated 된 듯... , 대신 `create` 또는 `--generator=run-pod/v1` 삽입
`--port=9381`은 앱이 포트 9381에서 수신 대기하고 있음을 알려준다



### 포드에 관하여...
쿠버네티스는 개별 컨테이너는 직접 취급하지 않고, 여러 위치 배치되면 컨테이너 그룹 개념을 사용한다.
이러한 컨테이너 그룹을 `pod`라고 한다.

포드는 동일한 리눅스 네임스페이스와 동일한 워커 노드에서 항상 함께 실행된다.

각 포드는 애플리케이션을 실행하는 자체 IP, Host, Process 등이 있는 별도 논리적 시스템이다.
(이 부분은 완벽히 이해가 되질 않는군...)



### 포드 나열하기
```bash
kubectl get pods
```
|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62424827-4b771a80-b70f-11e9-84ca-0fc7ab81da36.png"> |

이미지가 다운로드 중이라면, 상태가 `PENDING`으로 뜸
다운로드 완료시, `RUNNIG`으로 전환됨

계속 안된다면, `k describe pods taeuk-test` 실행하여 확인



![쿠버네티스에서 컨테이너 이미지 실행과정](https://user-images.githubusercontent.com/26294469/62424702-66488f80-b70d-11e9-89fd-486427eed45b.png)



### 포드 삭제하기
```bash
k delete po 이름
```

포드를 잘못 만들었을 경우나, 무한 wait시 삭제하고 다시 run

강제 삭제 명령어
`k delete pods taeuk-test --grace-period=0 --force`



### 웹 애플리케이션 접근
각 포드의 자체 IP가 존재하지만, 이 주소는 클러스터 내부에 존재하며 외부에서 접근 불가능함

외부에서 액세스하려면, 서비스 객체를 통해 IP를 노출해야함

이를 위해, `LoadBalancer` 형태의 특별한 서비스가 필요함 (로드밸런서의 공용IP로 포드에 연결가능)

#### 서비스 객체 생성

```bash
k expose rc taeuk-test --type=LoadBalancer --name taeuk-http
```

#### 서비스 목록 보기
```bash
k get services
```

바로 생성했다면, 아직 외부 IP주소가 없는 것을 확인.
클라우드 인프라에서 로드 밸런서를 생성하는데 시간이 걸리기 때문. 외부 IP가 생길 때까지 대기



### (이슈해결) 쿠버네티스에서 삭제해도, 계속 실행중일시
`k delete pods name`을 해도 뒤의 다른 이름을 달고 계속 생성될 때가 있다.

|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62463561-4d9bb080-b7c5-11e9-991b-6e208cd86d73.png"> |

이것은 지워도 `deployment`와 `rc`(레플리카컨트롤러) 에서 계속 생성하기 때문이다.

그러므로, `k get deployment` 또는 `k get rc`를 하여 목록을 확인하고,

|                          결과 화면                           |
| :----------------------------------------------------------: |
| <img src="https://user-images.githubusercontent.com/26294469/62463694-9fdcd180-b7c5-11e9-970d-c7ec41afac77.png"> |

`k delete deployment taeuk-test` 또는 `k delete rc taeuk-test`를 하여 계속 실행되는 것을 방지