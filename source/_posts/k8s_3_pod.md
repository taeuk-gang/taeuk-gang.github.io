---
title: 3장 파드 - 쿠버네티스에서 컨테이너 실행
toc: true
date: 2020-11-08 22:33:18
tags:
    - Kubernetes
categories:
    - Kubernetes
---
> 개인이 "쿠버네티스 인 액션" 책을 읽고 학습한 내용으로, 틀린 내용이 있을 수 있습니다.

# 3장 파드: 쿠버네티스에서 컨테이너 실행

> 주요 내용
>
> - 파드 생성, 실행, 정지
> - 파드, 다른 리소스를 라벨로 조직화
> - 특정 라벨을 가진 모든 파드에서 작업 수행
> - 네임스페이스로 파드 나누기
> - 특정 형식을 가진 워커노드에 파드 배치

## 3.1 파드 소개

Q: 파드가 뭔가요?

A: 쿠버네티스의 기본 단위로, **컨테이너를 개별적으로 배포하기보다** 일반적으로 한개의 **컨테이너를 가진 파드를 배포**하여 운영합니다. 특정 상황에서는 두개 이상이 될 수도 있겠지만, 일반적으로 하나의 컨테이너만 포함한 상태로 배포를 합니다.
중요한 것은 **항상 하나의 워커 노드에서 실행**이 된다는 것입니다.

### 3.1.1 파드 필요 이유

Q: 왜 파드가 필요한가요?
or 컨테이너를 왜 직접 사용하지 않나요?
or 왜 일반적으로 여러 컨테이너를 같이 실행하지 않나요?
or 모든 프로세스를 단일 컨테이너에 넣으면 안되나요?

A: 컨테이너는 단일 프로세스를 실행하는 것을 목적으로 설계되었고, 그 이유는 관련 없는 다른 프로세스들 을 모두 집어넣었을 경우, 동일한 표준 출력으로 로그를 기록하기 때문에 **각 로그가 누가 남긴 로그인지 분별하기 힘들다**.

> 단일 책임 원칙과 비슷한 원리로 이해됨, 상세 이유는 그 뒷장에서 소개됨
>
> 간단히 더 이야기하면, 컨테이너 그룹(파드)를 쪼갤 수록 노드의 파드들을 균형있게 분배할 수 있으며, 한쪽으로 자원이 넘쳐 실행되지 않을 것이다.

### 3.1.2 파드 이해

(질문 이어서)

A: **여러 프로세스를 단일 컨테이너로 묶지 않기 때문에**, 컨테이너를 묶을 단위가 필요하여 나타난게 **파드**이다.

쿠버네티스는 파드 안에 모든 컨테이너가 네임스페이스가 아닌 동일한 리눅스 네임스페이스를 공유하도록 도커를 설정함



Q: 설정 내용이 뭐가 있나요?

A: 파드 내의 컨테이너들은 같은 호스트이름과 네트워크 인터페이스를 공유하고 IPC를 통해 서로 통신합니다.
그래서 파드 내 컨테이너는 동일한 IP주소와 포트를 공유합니다.



Q: 그럼 부분 격리라고 하였는데, 현재 공유되는 내용만 이야기되었는데 격리되는 내용은 무엇인가요?

A: 기본값으로 파일시스템은 다른 컨테이너와 완전히 분리됩니다. 볼륨에서 설정을 통해야만 파일을 공유할 수 있습니다.



Q: 파드간 네트워크 IP주소와 포트는 다르다는 이야기로 들리는데, 파드간에는 어떻게 통신하나요?

A: 파드간 다른 IP주소를 통해 접근을 하지만, 둘 사이에는 NAT(network address translation)이 없어서, LAN간 통신처럼 통신할 수 있습니다. 이것을 플랫 네트워크라고 표현합니다. 이는 소프트웨어 네트워크 계층으로 이루어져있습니다.

### 3.1.3 파드와 컨테이너 구성

Q: 프론트 서버와 백엔드(DB) 컨테이너를 어떻게 구성하면 좋을까요?

A: 앞에 이야기한 내용에서 일반적으로 한개의 컨테이너당 하나의 파드로 분할하여 실행시키는 것이 좋습니다. 만약 둘이 하나의 파드로 묶여 관리된다면 둘은 항상 같은 노드에서 실행되어야하며, 만약 두개의 노드가 있다면 1개는 유휴 상태가 될 것 입니다.

A2: 또한 두 컨테이너를 하나의 파드에서 관리하면 안되는 이유로는 스케일링 입니다. 파드가 스케일링의 기본 최소 단위이기 때문에 쿠버네티스는 개별 컨테이너를 수평으로 확장하지 못합니다. 그래서 전체 파드를 수평으로 확장해야합니다. 그런데 프론트와 백엔드는 서로 다른 스케일링 요구사항을 가지고있기 때문에 나누는 것이 좋습니다.



Q: 그럼에도 불구하고, 예외적으로 하나의 파드에서 여러 컨테이너를 사용해야하는 경우가 있을까요?

A: 여러 컨테이너를 단일 파드에 넣는 주요 경우는 (주 컨테이너 + 보조 컨테이너들) 을 이루는 경우인데요.

예를 들어, 
주 컨테이너 = 웹 서버
보조 컨테이너 = 주기적으로 컨텐츠를 웹서버 디렉토리에 저장
혹은 로그 수집, 데이터 프로세서, 통신 어댑터 등등

을 사유로하는 컨테이너들이 함께 실행되야하는 경우입니다.

그래서 여러 컨테이너를 단일 파드에서 실행해야한다면 다음과 같은 조건을 생각해야합니다.

1. 컨테이너를 함께 실행시켜야하는가?
2. 여러 컨테이너가 결국 하나의 구성요소인가?
3. 컨테이너가 함께 스케일링 되어야하는가?

## 3.2 YAML 또는 JSON으로 파드 생성

Q: YAML으로 작성하면 장점이 무엇이 있나요?

A: `kubectl run`으로 간단하게 리소소를 생성 가능하지만, 제한된 속성만 생성할 수 있으며, YAML을 사용할 경우 **버전 관리가 가능**해지는 장점이 있습니다.

YAML의 작성은 [해당 링크](https://v1-18.docs.kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/)를 참조하면 됩니다. (하루종일 보고 사는 링크...;;)

### 3.2.1 기존 파드 YAML 살펴보기

```bash
kubectl get pod <파드명> -o yaml
```

위의 명령어를 입력하면 YAML 정의를 볼 수 있다.

막상 보면 너무 긴 내용이 있을텐데, 주요 파트는 3개이다.

- metadata: 이름, 네임스페이스, 라벨, 파드에 관한 정보
- Spec: 파드 컨테이너, 볼륨, 파드 실제 명세 정보
- status: 파드 상태, 각 컨테이너 설명, 상태, 파드 내부IP 등 현재 실행중인 파드 정보
  파드를 생성할 때는 작성할 필요없는 항목

### 3.2.2 파드 정의 YAML 작성

kubia-manual.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
	name: kubia-menual
spec:
  containers:
  - image: luksa/kubia
  	name: kubia
  	ports:
  	-	containerPort: 8080
  		protocol: TCP
```

포트를 지정해둔 것은 단지 정보에 불과, 이를 생략해도 다른 클라이언트에서 포트를 통해 파드에 연결할 수 있는지에 영향 미치지 않음. 0.0.0.0 주소에 열어둔 포트를 통해 접속을 허용시에 파드 스펙과 별개로 항상 해당 파드에 접속이 가능하기 때문

Q: 그럼 왜 명시하나요?

A: 포트를 명시적으로 정의하면, 클러스터를 사용하는 모든 사람이 볼 수 있고, 포트에 이름을 지정해 편리하게 사용 가능

### 기타: 유용한 명령어 `kubucetl explain`

```bash
kubectl explain pods
```

![image-20201108175732508](https://i.loli.net/2020/11/08/wtCPd6khueFbqms.png)

```bash
kubectl explain pod.spec
```

![image-20201108175825146](https://i.loli.net/2020/11/08/ZGXSAFoVjTndCkJ.png)

### 3.2.3 `kubectl create`

음... 리소스를 만들기 전에 현재 어느 네임스페이스에서 만드는지는 확인하는 것이 좋다

```bash
k config get-contexts
# or
k config view --minify | grep namespace:
```

![image-20201108180326200](https://i.loli.net/2020/11/08/1LTtx5UHuV4Qj8E.png)

클러스터 c201에서 test2 네임스페이스를 사용하는 것을 확인할 수 있음

#### 생성

```bash
k create -f kubia-manual.yaml -o yaml
```

![image-20201108180852090](https://i.loli.net/2020/11/08/5umetJ6CElbP1Qq.png)

#### 생성 확인

```bash
k get pods
```

![image-20201108181004010](https://i.loli.net/2020/11/08/wltGNscUE67zLiJ.png)

### 3.2.4 애플리케이션 로그 보기

컨테이너의 애플리케이션은 보통 로그를 파일에 쓰기보다는 표준 출력과 표준 에러에 로그를 남기는 것이 일반적

컨테이너 런타임(ex. 도커)는 이러한 스트림을 파일로 전달하고 다음 명령을 통해 로그를 가져옴



쿠버네티스를 사용하지 않았다면, ssh로 파드가 실행 중인 노드에 접속해 `docker logs <컨테이너ID>`를 치겠지만,

```bash
kubectl logs kubia-manual
```

쿠버네티스는 해당 명령을 제공한다.

![image-20201108181515961](https://i.loli.net/2020/11/08/rdRpCn8H2yiLWvj.png)

#### 주의 사항

**컨테이너 로그는 하루 단위 또는 로그 파일이 10MB에 도달할 때마다 순환함**

**kubectl logs는 마지막으로 순환된 로그만 보여줌**

**또한 파드가 삭제되면 로그도 삭제됨**

**파드가 삭제되도 로그를 확인하고 싶다면 클러스터 전체의 중앙집중식 로깅을 설정해야함(17장)**



#### 여러 컨테이너를 가진 파드의 로그 확인

```bash
# kubectl logs <파드명> -c <컨테이너명>
kubectl logs kubia-manual -c kubia
```

### 3.2.5 파드에 요청 보내기

파드의 실제 동작을 보는 방법 (`kubectl expose`를 사용하지 않고)

파드에 테스트와 디버깅 목적으로 사용, 포트포워딩

```bash
kubectl port-forward kubia-manual 8888:8080
```

![image-20201108182156830](https://i.loli.net/2020/11/08/QW8ic6l4kr13h29.png)

```bash
curl localhost:8888
```

`curl`을 통해 접근이 가능

![image-20201108182422839](https://i.loli.net/2020/11/08/8s5brpld2v1zc6S.png)

## 3.3 라벨을 이용한 파드 구성

실제 애플리케이션들이 배포된 클러스터 내에는 매우 많은 파드들이 존재, 그러므로 파드를 정리하는 매커니즘이 필요

### 3.3.1 라벨 소개

위의 이유로 라벨은 리소스에 키-값으로 첨부할 수 있다. 해당 리소스내에서 고유하다면, 원하는 만큼 추가할 수 있다.

예를 들면, `app=ui`, `app=as`, `app=pc` 등으로 애플리케이션 라벨을 붙이고, `rel=stable`, `rel=beta`, `rel=alpha`로 버전을 관리할 수 있다.

### 3.3.2 파드 생성시 라벨 지정

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-menual
  labels:
    app: test
    rel: beta
spec:
  containers:
  - image: luksa/kubia
    name: kubia
    ports:
    - containerPort: 8080
      protocol: TCP
```

#### 파드 라벨 보기

```bash
kubectl get pod --show-labels
```

![image-20201108184538134](https://i.loli.net/2020/11/08/Yht1nmSdaj9KA7l.png)

#### 파드 특정 라벨만 보기

```bash
kubectl get pod -L app
```

![image-20201108184629324](https://i.loli.net/2020/11/08/S6RFvwdjXlDCgLW.png)

### 3.3.3 기존 파드 라벨 수정

#### 파드에 새로운 라벨 추가

```bash
kubectl label po kubia-manual added_label=0
```

![image-20201108185237231](https://i.loli.net/2020/11/08/phAWVZw8Y2oByFI.png)

#### 파드에 기존 라벨 수정

```bash
kubectl label po kubia-manual added_label=1
```

![image-20201108185303697](https://i.loli.net/2020/11/08/LFmABDTg3h2jNqn.png)

## 3.4 라벨 셀렉터로 파드 부분 집합 나열

라벨의 중요성은 라벨 셀렉터를 함께 사용할때 알 수 있음

예를 들어,

- 특정 키를 포함하거나 포함하지않는 라벨의 리소스
- 특정 키와 값을 가진 라벨의 리소스
- 특정 키를 가지고있지만, 다른 값을 라벨의 리소스

### 3.4.1 라벨 셀렉터로 파드 나열

#### 특정 키와 값 라벨 셀렉터 조건

```bash
kubectl get po -l app=test -L app
```

![image-20201108185938590](https://i.loli.net/2020/11/08/2Yd5CRl4ZVqO163.png)

#### 값은 상관하지않는 라벨 셀렉터 조건

```bash
kubectl get po -l app -L app
```

#### 값을 가지고있지 않는 라벨 셀렉터 조건

```bash
kubectl get po -l '!app'
```

#### 값이 아닌 라벨 셀렉터 조건

```bash
kubectl get po -l app!=test
```

#### 값이 A 또는 B 인 조건

```bash
kubectl get po -l env in (test, rel)
```

#### 값이 둘다 아닌 것

```bash
kubectl get po -l env notin (test, rel)
```



### 3.4.2 라벨 셀렉터에서 조건 사용

책에 자세한 내용 X

> [해당 링크](https://kubernetes.io/ko/docs/concepts/overview/working-with-objects/labels/) 참조

## 3.5 라벨 셀렉터로 파드 스케줄링

쿠버네티스는 기본적으로 무작위로 워커 노드에 스케줄링한다. 모든 노드를 하나의 대규모 배포 플랫폼으로 생각하고 어떤 노드에 스케줄링은 중요하지 않기 때문이다.

그러나, 하드웨어 인프라가 동일하지 않은 상황일 경우, ex- 워커 노드 중 일부는 ssd, 다른 일부는 hdd, 거나 GPU 가속 제한 등은 특정 파드를 스케줄링 해주어야 할 것이다.

정확한 노드를 지정하는 대신, 노드의 필요 요구사항을 기술하고 만족하는 노드를 선택하도록 한다.
이를 노드 라벨과 라벨 셀렉터를 통해 할 수 있다.

### 3.5.1 워커 노드 분류에 라벨 사용

라벨은 모든 리소스에 적용 가능

```bash
kubectl label node <노드명> gpu=true
```

### 3.5.2 특정 노드에 파드 스케줄링

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-gpu
spec:
  nodeSelector:
    gpu: "true"
  containers:
  - image: luksa/kubia
    name: kubia
```

spec.nodeSelector를 사용하여 파드 스케줄링

### 3.5.3 하나의 특정 노드로 스케줄링

각 노드에는 `kubernetes.io/hostname=<호트트네임>`으로 고유 라벨이 존재하기 때문에 특정 하나의 노드로 스케줄링이 가능하다.

그러나, 해당 노드가 오프라인일 경우, 스케줄링이 되지 않을 수 있다.

## 3.6 파드에 어노테이션 달기

어노테이션 또한 라벨처럼 모든 리로스에 추가할 수 있다. 그러나 차이점으로, 식별 정보를 가지고 있지 않다.(셀렉터가 없음)

그러면 어디다 쓰지? 라벨보다 많은 정보를 가질 수 있다. (256kb까지 허용) 

주로 쿠버네티스의 새기능이 추가하기 전에 어노테이션에서 시험적으로 적용되고, 이후 결정되면 필드로 도입된다.

유용하게 사용되는 경우는 오브젝트를 만든 사람을 어노테이션으로 지정해두면, 협업자들이 더 쉽게 알 수 있다.

### 3.6.1 오브젝트 어노테이션 조회

`k describe` or `k get po -o json` 으로 조회

### 3.6.2 어노테이션 추가 및 수정

```bash
kubectl annotate pod kubia-manual company.com/annotation="test data"
```

어노테이션의 경우 `--overwrite`없이 덮어쓰기 때문에 위의 형식처럼 입력된다.

## 3.7 네임스페이스로 리소스 그룹화

**쿠버네티스 네임스페이스**는 프로세스를 격리하는데 사용하는 **리눅스 네임스페이스**와는 다르다.

오브젝트 그룹이 서로 겹칠 수 있기 때문에 한번에 하나의 그룹에서만 작업하고 싶을 때 사용

### 3.7.1 네임스페이스 필요성

여러 네임스페이스를 사용하면, 멀티테넌트 환경처럼 리소스를 분리하는데 사용된다. (다른 네임스페이스 간에는 동일한 리소스명을 가질 수 있다)

예를 들어, 프로덕션, 개발, QA 환경 등으로 나누어 사용할 수 있다.

또한 네임스페이스는 리소스를 격리하는 것외에도 특정 사용자가 지정된 리소스에만 접근할 수 있도록 하고, 컴퓨팅 리소스르 ㄹ제한하는데도 사용된다.

### 3.7.2 네임스페이스 조회

```bash
# 네임스페이스 목록 조회
kubectl get ns

# 특정 네임스페이스에 속한 파드 목록 조회
kubectl get po -n kube-system
```

### 3.7.3 네임스페이스 생성

#### Yaml로 생성

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: custom-ns
```

```bash
k create -f create-ns.yaml
```



#### 명령어로 생성

```bash
kubectl create ns custom-ns
```

대부분의 오브젝트는 RFC1035 규칙 - 문자, 숫자, 대시, 점을 표현할 수 있지만, 네임스페이스의 경우 점(.)을 포함하지 못한다.

### 3.7.4 다른 네임스페이스 오브젝트 관리

```bash
kubectl create -f <YAML파일명>.yaml -n <파드를 생성할 네임스페이스명>
```

#### 현재 네임스페이스 빠르게 변경하는 alias

`~/.bashrc`에 추가

```bash
alias kcd='kubectl config set-context $(kubectl config current-context) --namespace '
```

### 3.7.5 네임스페이스 격리 이해

네임스페이스를 사용하면 오브젝트를 별도 그룹으로 분리해 작업할 수 있게 하지만, 실행 중인 오브젝트에 대한 격리는 제공하지 않는다.

예를 들어, 다른 사용자들이 서로 다른 네임스페이스에 파드를 배포할 때 해당 파드가 서로 격리되어 통신할 수 없는 것이 반드시 이루어지지는 않는다. 해당 쿠버네티스 네트워킹 솔루션에 따라 다르다.

## 3.8 파드 중지 제거

### 3.8.1 이름으로 파드 삭제

```bash
# kubectl delete po <삭제할 파드명>
kubectl delete po kubia-gpu

# 여러개 제거시
kubectl delete po pod1 pod2
```

파드를 삭제하면, 해당 파드 안에 있는 모든 컨테이너를 종료하도록 지시한다. 과정은 아래와 같다.

1. 쿠버네티스는 프로세스에 SIGTERM 신호를 보냄 (기본값 30초 동안 기다림)
2. 종료되지 않으면 SIGKILL 신호를 통해 종료 (정상 종료는 SIGTERM)

### 3.8.2 라벨 셀렉터로 파드 삭제

```bash
kubectl delete po -l rel=beta
```

해당 라벨에 관련된 파드가 제거됨

### 3.8.3 네임스페이스로 파드 제거

```bash
kubectl delete ns custom-ns
```

네임스페이스 전체를 삭제하고, 해당 내에 파드는 자동으로 삭제된다.

### 3.8.4 네임스페이스 유지하면서 안에 모든 파드 삭제

```bash
kubectl delete po --all
```

현재 네임스페이스 내의 모든 파드를 제거

> 만약 파드를 삭제하더라도 계속 생성이 된다면, 레플리케이션컨트롤러가 계속 재생성하는 것으로 레플리케이션컨트롤러를 삭제해야한다.

### 3.8.5 네임스페이스 모든 리소스 삭제

```bash
kubectl delete all --all
```

현재 네임스페이스에 모든 리소스 제거 명령

## 3.9 요약

다 읽고 할 수 있어야 하는 것들

- 특정 컨테이너를 파드로 묶어야 하는지 결정
- 파드로 여러 프로세스를 실행
- YAML 또는 JSON으로 파드 작성, 정의, 상태 확인
- 라벨과 라벨 셀렉터로 파드를 그룹화하고 한번에 여러 파드를 작업
- 노드 라벨과 셀렉터로 특정 노드에 파드를 스케줄링
- 어노테이션을 이용하여 더 큰 데이터 파드에 달기
- 네임스페이스로 팀들 간 동일한 클러스터를 각자 작업
- Kubectl explain 명령으로 리소스 정보를 찾기

