---
title: TIL 10월 10일 (일)
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20210919162643014.png')
marp: true
secret: true
---
<style>
ul, ol, p {
    font-size: 0.75rem;
}
img[alt~="center"] {
  display: block;
  margin: 0 auto;
}

header {
  color: #fff;
}

table {
  font-size: 0.6rem;
}

pre > code {
  color: initial;
}

h1 {
  font-size: 1.2rem;
}

h2 {
	font-size: 1rem;
}

h3 {
  font-size: 0.8rem;
}

h4 {
  font-size: 0.6rem;
}

h5 {
  font-size: 0.4rem;
}
</style>

## TIL: 10월 10일 (일)

![w:300 center](https://user-images.githubusercontent.com/26294469/136711516-d933a81d-6b83-4bdb-a0b3-bb46c76c2a4b.gif)

<style scoped>
h2, h3 {
  /* display: flex;
  justify-content: center;
  align-items: center; */
  text-align: center;  
  line-height: 0.8rem;
  color: #000;
}

h2 {
  font-size: 1.8rem;
}

h3 {
  font-size: 1.2rem;
}

strong {
  color: #fff;
}

p {
  color: #fff;
}
</style>

---
### 1. impersonation에 대해서 잘 몰라서 정리

<style scoped>
section {
    display: flex;
    align-items: center;
    justify-content: center;
}

h3 {
    font-size: 1rem;
}
</style>


---

> 임퍼스네이션에 대해서 잘 몰라서 정리
> 참고자료: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#user-impersonation

## User impersonation

- 임퍼스네이션 헤더를 통해 다른 사용자 역할을 수행 가능
- authenticates을 override를 하여 재정의
- 예를 들면, admin이 디버그를 위해 임시적으로 다른 유저를 가장함

### 동작 순서
- credentials과 impersonation headers과 함께 API 요청
- API 서버는 사용자를 인증 (Impersonation 권한을 체크함)

---

### HTTP 헤더
- `Impersonate-User`: 유저명
- `Impersonate-Group`: 유저 그룹, 여러개 가능.
- `Impersonate-Extra-( extra name )`: 추가 커스텀 헤더
- `Impersonate-Uid`: UID, k8s 1.22.0 버전 이상에서 지원

### 사용 예시

일반
```bash
kubectl drain mynode
```

Impersonation 사용 예시
```bash
kubectl drain mynode --as=superman --as-group=system:masters
```

---

### 사용 조건

`ClusterRole`에 `impersonate` verbs 필요

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: impersonator
rules:
- apiGroups: [""]
  resources: ["users", "groups", "serviceaccounts"]
  verbs: ["impersonate"]
```

-------------

다양한 예시

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: limited-impersonator
rules:
# Can impersonate the user "jane.doe@example.com"
- apiGroups: [""]
  resources: ["users"]
  verbs: ["impersonate"]
  resourceNames: ["jane.doe@example.com"]

# Can impersonate the groups "developers" and "admins"
- apiGroups: [""]
  resources: ["groups"]
  verbs: ["impersonate"]
  resourceNames: ["developers","admins"]

# Can impersonate the extras field "scopes" with the values "view" and "development"
- apiGroups: ["authentication.k8s.io"]
  resources: ["userextras/scopes"]
  verbs: ["impersonate"]
  resourceNames: ["view", "development"]

# Can impersonate the uid "06f6ce97-e2c5-4ab8-7ba5-7654dd08d52b"
- apiGroups: ["authentication.k8s.io"]
  resources: ["uids"]
  verbs: ["impersonate"]
  resourceNames: ["06f6ce97-e2c5-4ab8-7ba5-7654dd08d52b"]
```

<style scoped>
  pre {
    font-size: 0.5rem;
  }
</style>

---

API 접근 확인

```bash
kubectl auth can-i create deployments --namespace dev
```
결과는 다음과 같이 출력
```
yes
```

- kubectl은 API 인증 계층을 신속하게 쿼리하기 위한 "auth can-i" 하위 명령어를 제공
- 이 명령은 현재 사용자가 지정된 작업을 수행할 수 있는지 여부를 알아내기 위해 SelfSubjectAccessReview API를 사용
- 사용되는 인가 모드에 관계없이 작동

관리자는 이를 사용자 가장(impersonation)과 병행하여 다른 사용자가 수행할 수 있는 작업을 결정 가능

```
kubectl auth can-i list secrets --namespace dev --as dave
```

---

- SelfSubjectAccessReview는 authorization.k8s.io API 그룹의 일부로서 API 서버 인가를 외부 서비스에 노출

- 이 그룹의 기타 리소스에는 다음이 포함
  - `SubjectAccessReview`
    - 현재 사용자뿐만 아니라 모든 사용자에 대한 접근 검토
    - API 서버에 인가 결정을 위임하는 데 유용
    - 예를 들어, kubelet 및 확장(extension) API 서버는 자신의 API에 대한 사용자 접근을 결정하기 위해 해당 리소스를 사용
  - `LocalSubjectAccessReview`
    - SubjectAccessReview와 비슷하지만 특정 네임스페이스로 제한
  - `SelfSubjectRulesReview`
    - 사용자가 네임스페이스 안에서 수행할 수 있는 작업 집합을 반환하는 검토
    - 사용자가 자신의 접근을 빠르게 요약해서 보거나 UI가 작업을 숨기거나 표시하는 데 유용

---

이러한 API는 반환된 오브젝트의 응답 "status" 필드가 쿼리의 결과인 일반 쿠버네티스 리소스를 생성하여 쿼리 가능

```yaml
kubectl create -f - -o yaml << EOF
apiVersion: authorization.k8s.io/v1
kind: SelfSubjectAccessReview
spec:
  resourceAttributes:
    group: apps
    resource: deployments
    verb: create
    namespace: dev
EOF
```

> 다음 페이지 계속

---

생성된 SelfSubjectAccessReview 는 다음과 같다.

```yaml
apiVersion: authorization.k8s.io/v1
kind: SelfSubjectAccessReview
metadata:
  creationTimestamp: null
spec:
  resourceAttributes:
    group: apps
    resource: deployments
    namespace: dev
    verb: create
status:
  allowed: true
  denied: false
```

---

음... `can-i` 명령을 이용하여 모든 리소스 권한을 알 수는 없을까?

아래의 명령을 이용하면 가능
```
kubectl auth can-i --list
```
![bg fit right](https://user-images.githubusercontent.com/26294469/136702969-67e508db-7f14-45d9-8e8d-ce26703a3f29.png)

---
### 2. 쿠버네티스 API 헬스 엔드포인트 (/readyz)

<style scoped>
section {
    display: flex;
    align-items: center;
    justify-content: center;
}

h3 {
    font-size: 1rem;
}
</style>

---

쿠버네티스 API 서버는 현재 상태를 나타내는 세 가지 API 엔드포인트(healthz, livez 와 readyz)를 제공

- healthz 엔드포인트는 사용 중단(deprecated)됨 (쿠버네티스 v1.16 버전 이후)
- -> 대신 보다 구체적인 livez 와 readyz 엔드포인트를 사용

- `livez`
  - livez 엔드포인트는 --livez-grace-period 플래그 옵션을 사용하여 시작 대기 시간을 지정 가능

- `readyz`
  - /readyz 엔드포인트는 --shutdown-delay-duration 플래그 옵션을 사용하여 정상적(graceful)으로 셧다운 가능

- API 서버의 health/livez/readyz 를 사용하는 머신은 HTTP 상태 코드에 의존

- 모든 엔드포인트는 `verbose` 파라미터를 사용하여 검사 항목과 상태를 출력 가능
(API 서버의 현재 상태를 디버깅하는데 유용)
```
curl -k https://localhost:6443/livez?verbose
```

---

- 인증을 사용하는 원격 호스트에서 사용할 경우에는 다음과 같이 수행
```
kubectl get --raw='/readyz?verbose'
```

```
[+]ping ok
[+]log ok
[+]etcd ok
[+]poststarthook/start-kube-apiserver-admission-initializer ok
[+]poststarthook/generic-apiserver-start-informers ok
[+]poststarthook/start-apiextensions-informers ok
[+]poststarthook/start-apiextensions-controllers ok
[+]poststarthook/crd-informer-synced ok
[+]poststarthook/bootstrap-controller ok
...
[+]poststarthook/apiservice-openapi-controller ok
healthz check passed
```

---

- 또한 쿠버네티스 API 서버는 특정 체크를 제외 가능
```
curl -k 'https://localhost:6443/readyz?verbose&exclude=etcd'
```

```yaml
[+]ping ok
[+]log ok
...
[+]etcd excluded: ok
healthz check passed
```

<style scoped>
  pre [class*="hljs-"]:nth-child(n+8):nth-child(-n+10) {
    background-color: yellow;
  }
</style>

---

## 개별 헬스 체크 (`FEATURE STATE: Kubernetes v1.22 [alpha]`)

- 각 개별 헬스 체크는 HTTP 엔드포인트를 노출하고 개별적으로 체크가 가능
- 개별 체크를 위한 스키마는 `/livez/<healthcheck-name>`
- livez 와 readyz 는 API 서버의 활성 상태 또는 준비 상태인지를 확인할 때 사용
- `<healthcheck-name>` 경로 위에서 설명한 verbose 플래그를 사용

```
curl -k https://localhost:6443/livez/etcd
```

---
### 3. 파라렐즈 리눅스 외부에서도 접속 가능하게 하기

<style scoped>
section {
    display: flex;
    align-items: center;
    justify-content: center;
}

h3 {
    font-size: 1rem;
}
</style>

---

> 참고자료: https://khmirage.tistory.com/382

### 원인

- Parallels를 이용해서 운영체제를 설치하게되면 기본적으로 네트워크가 NAT모드로 설정되어서 Guest OS의 IP 대역이 10.x.x.x로 잡히게 됨
10.x.x.x 대역으로 IP가 설정이 되버리면 Parallels가 설치되어있는 컴퓨터가 아닌 공유기 아래의 다른 컴퓨터에서는 저 가상머신에 접근할수가 없게 됨

---

### 해결

1. Parallels의 VM설정에서 Hardware -> Network1 에서 Type을 Shared Network에서 Bridge 모드의 Default Adapter를 선택
![bg fit right](https://user-images.githubusercontent.com/26294469/136706790-cefe0b29-ee01-44f0-adb7-8e41f4d9b1fc.png)

--- 

2. Ubuntu Server를 재부팅
```
shutdown -h now
```

- IP 변경됬는지 확인
![bg fit right](https://user-images.githubusercontent.com/26294469/136707142-66c43158-3f0d-41fc-8fe9-cdf44eb0818b.png)
- 이후 ssh 연결시 정상 접속되는 것을 확인
- 공유기 설정을 통해 공인IP까지 맞추면 외부 접속 가능할 듯?
