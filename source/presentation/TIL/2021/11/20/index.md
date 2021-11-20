---
title: Helm 사용자 시나리오
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
  line-height: 0%;
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

section {
  background-size: 108% !important;
}
</style>

![w:100 center](https://user-images.githubusercontent.com/26294469/142735881-d0ae765d-f409-486a-b7f5-5322768d2fac.png)

## Helm 사용해보기

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
</style>

---
<!-- header: 목차 -->
<!-- footer: 참고. [Helm QuickStart](https://helm.sh/ko/docs/intro/quickstart/) -->

1. [Helm이란](#3)
2. [Usage. Helm Chart 생성](#5)
3. [Usage. Helm Chart 삭제](#12)
4. [Usage. values.yaml으로 템플릿 문법 사용](#13)
5. [Usage. Release 템플릿 문법 사용](#17)
6. [Usage. 그 외 Helm 자주 사용하는 명령어](#22)
7. [Usage. values.yaml override](#23)
8. [Usage. Helm Chart 업그레이드](#28)
9. [Usage. Helm Rollback](#31)

---
<!-- header: 1. Helm이란 -->
<!-- footer: 참고. [Helm 공식사이트](https://helm.sh/ko/)  -->

![w:800 center](https://user-images.githubusercontent.com/26294469/142736328-9e42a5b0-f279-43d7-8957-e2c3567c12ff.png)

---

> 비유를 한다면 개발자가 Docker Registry에서 검색하여 쉽게 이미 셋팅된 컨테이너 환경을 실행하듯이 
Helm에서 검색하여 이미 셋팅된 쿠버네티스 환경을 실행한다라고 이해하면 되지 않을 듯 싶다.
ex. `Dockerhub`처럼 `ArtifactHub`에서 많은 템플릿이 등록되고 공유되고 있다.

컨테이너 환경을 `Dockerfile`로 구성 하는 것처럼 Helm도 `차트`로 구성을 한다.
- `차트` = 쿠버네티스 YAML을 템플릿으로 구성한 파일들

![w:1000 center](https://user-images.githubusercontent.com/26294469/142736520-489f1f42-d87c-4526-acc1-62a8db760aab.png)

---
<!-- header: 2. Usage. Helm Chart 생성 -->  
<!-- footer: 참고. [Helm 시작하기](https://helm.sh/ko/docs/chart_template_guide/getting_started/) -->

- Helm Chart를 생성하기 위해서는 위에서 설명한 구조의 파일들을 생성할 필요가 있음

- nginx 템플릿을 단계별로 구성하여 따라해보기

1. `/templates` 폴더 생성하여 배포할 쿠버네티스 YAML 작성

> deployment, service YAML은 뒤에 작성

```bash
.
└── templates
    ├── deployment.yaml
    └── service.yaml
```

---

### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-test
  labels:
    app: nginx-test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-test
  template:
    metadata:
      labels:
        app: nginx-test
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

---

### `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-test
spec:
  selector:
    app: nginx-test
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

---
<!-- footer: 참고 링크: [Chart.yaml](https://helm.sh/ko/docs/topics/charts/)-->

2. `Chart.yaml` 작성
- `Chart.yaml` = Helm 차트의 이름, 버전 등 메타데이터를 입력하는 파일

```yaml
apiVersion: v2 # 차트 API 버전 (필수)
name: nginx-test # 차트명 (필수)
version: 0.0.1 # SemVer 2 버전 (필수)
```

현재 파일 구성
```bash
.
├── Chart.yaml
└── templates
    ├── deployment.yaml
    └── service.yaml
```

---
<!-- footer: 참고. [values.yaml](https://helm.sh/ko/docs/chart_template_guide/values_files/)-->

3. `values.yaml` 작성
- `values.yaml` = `/templates` 내부에 쿠버네티스 YAML에 있는 값을 동적으로 지정할 때 사용하는 파일
- 일단은 파일이 비어있는 상태로도 설치가 가능하기 때문에 파일만 생성
```bash
touch values.yaml
```

```bash
.
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   └── service.yaml
└── values.yaml
```

---
<!-- footer: 참고. [Helm Install](https://helm.sh/ko/docs/helm/helm_install/) -->

4. Helm Chart 설치

```bash
helm install <이름> <차트_경로> -n <네임스페이스>
```

실행 결과
```bash
# helm install nginx-test . -n test

NAME: nginx-test
LAST DEPLOYED: Sun Nov 21 05:41:08 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1 # helm upgrade 반영 횟수
TEST SUITE: None
```

Helm 조회
```bash
# helm ls -n test
NAME      	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART           	APP VERSION
nginx-test	test     	1       	2021-11-21 05:41:08.564944731 +0900 KST	deployed	nginx-test-0.0.1
```

---

실제 쿠버네티스 배포현황 확인

```bash
# k get deploy,svc -n test
NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-test   1/1     1            1           4m3s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/nginx-test   ClusterIP   10.100.29.142   <none>        80/TCP    4m3s
```

---
<!-- header: 3. Usage. Helm Chart 삭제 -->

### 차트 삭제

```bash
helm delete <릴리즈된_차트명> -n <네임스페이스>
```

실행 결과
```bash
# helm delete nginx-test -n test

release "nginx-test" uninstalled
```

이후, `helm ls -n test` 명령어 실행시 차트가 삭제된 것을 확인할 수 있음

---
<!-- header: 4. Usage. values.yaml으로 템플릿 문법 사용 -->
<!-- footer: 참고. [values.yaml](https://helm.sh/ko/docs/chart_template_guide/values_files/) -->

> [p13.](#13)에서 "values.yaml = /templates 내부에 쿠버네티스 YAML에 있는 값을 동적으로 지정할 때 사용하는 파일" 라고 이야기된 단계에 관한 설명

- 위 과정을 하기 위해서는 [템플릿 문법](https://helm.sh/ko/docs/chart_template_guide/values_files/)을 사용해야함
- 예시로 `/templates` 내에 `deployment.yaml`에서 이미지 버전이 현재 `latest`로 되어있는 것을 외부에서 바꿀 수 있게 변경을 진행

---

1. `/templates/*.yaml` 수정

```yaml
...
    spec:
      containers:
      - name: nginx
        image: nginx:{{ .Values.version }}
        ports:
        - containerPort: 80
```

2. `values.yaml` 수정

```yaml
version: stable
```

---
<!-- footer: '' -->

3. Helm Chart 설치

```bash
helm install nginx-values-test .
```

실행 결과
```bash
NAME: nginx-values-test
LAST DEPLOYED: Sun Nov 21 06:11:26 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

배포 확인: 아래처럼 image version tag가 바뀐 것을 확인할 수 있다.

```bash
k get deploy nginx-test -o yaml
```

```bash
...
      - image: nginx:stable
```

---

### Helm Chart로 공개된 오픈소스 문법 참고

- [Nginx](https://artifacthub.io/packages/helm/bitnami/nginx)
- [Prometheus](https://artifacthub.io/packages/helm/prometheus-community/prometheus)
- [Grafana](https://artifacthub.io/packages/helm/grafana/grafana)

---
<!-- header: 5. Usage. Release 템플릿 문법 사용 -->
<!-- footer: 참고. [빌트인 객체](https://helm.sh/ko/docs/chart_template_guide/builtin_objects/) -->

기존 `.Values` 템플릿 문법으로는 `helm install <차트명>`에서 입력되는 `차트명`까지는 가져오지 못한다.
이러한 문제를 해결해주기 위한 템플릿 문법으로 `.Release`가 있다.

```yaml
# 1. /templates/deployment.yaml 릴리즈 템플릿 적용
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
  labels:
    app: {{ .Release.Name }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
...
```

---

```yaml
# 2. /templates/service.yaml 릴리즈 템플릿 적용
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}
spec:
  selector:
    app: {{ .Release.Name }}
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

---

Helm Chart 설치 (`.Release` 문법 적용)

```bash
helm install nginx-rename-test . -n test
```

실행 결과
```bash
# helm install nginx-rename-test . -n test
NAME: nginx-rename-test
LAST DEPLOYED: Sun Nov 21 06:48:04 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
# helm ls -n test
NAME             	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART           	APP VERSION
nginx-rename-test	test     	1       	2021-11-21 06:48:04.719317331 +0900 KST	deployed	nginx-test-0.0.1	           
nginx-values-test	test     	1       	2021-11-21 06:11:26.928779655 +0900 KST	deployed	nginx-test-0.0.1
```
---

> 아래 명령을 실행하면, Helm이 최종적으로 어떤 정보로 릴리즈를 했는지 알 수 있다.

Helm 명명된 릴리스에 대한 모든 정보 조회
```bash
helm get all <RELEASE_NAME>
```

---

실행 결과
```bash
# helm get all nginx-rename-test
NAME: nginx-rename-test
LAST DEPLOYED: Sun Nov 21 06:48:04 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
null

COMPUTED VALUES:
version: stable

HOOKS:
MANIFEST:
---
# Source: nginx-test/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-rename-test
spec:
  selector:
    app: nginx-rename-test
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
---
# Source: nginx-test/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-rename-test
  labels:
    app: nginx-rename-test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-rename-test
  template:
    metadata:
      labels:
        app: nginx-rename-test
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
        - containerPort: 80
```

---
<!-- header: 6. Usage. 그 외 Helm 자주 사용하는 명령어 -->
<!-- footer: 참고. [Helm 명령어](https://helm.sh/ko/docs/helm/helm_upgrade/) -->

- 릴리스 히스토리를 가져오기, 업그레이드 내역(revision) 확인 등 유용
```bash
helm history <릴리즈_차트명>
```

- Helm Chart 문법 검사
```bash
helm lint <차트_경로>
```

- Helm `/templates` + `values.yaml` 최종 결과를 보여준다.
```bash
helm template <차트명> <차트_경로>
```

- Helm 차트 릴리즈 업그레이드
> 추후 과정에서 더 자세히 설명
```bash
helm upgrade <릴리즈_차트명> <차트_경로>
```

---
<!-- header: 7. Usage. values.yaml override -->
<!-- footer: 참고. [values.yaml](https://helm.sh/ko/docs/chart_template_guide/values_files/) -->

- 보통의 경우에는 `values.yaml`을 직접 작성하지 않고, 남이 작성한 파일을 가져오기 때문에 override(덮어쓰기)하여 사용이 된다. 
- 아래의 2가지 방법이 있다.
  - `--set` 인자 사용
  - `-f` 옵션으로 파일을 작성

---

1. `--set` 인자 사용

```bash
helm install --set version=1.21.4 -n test nginx-set-test .
```

실행 결과
```bash
NAME: nginx-set-test
LAST DEPLOYED: Sun Nov 21 07:09:41 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

배포 결과 (`values.yaml` `version` 값이 `stable`이지만 1.21.4로 된 것을 알 수 있음)
```bash
# k get deploy -n test nginx-set-test -o yaml | grep image
...
      - image: nginx:1.21.4
```

---

2. `-f` 옵션으로 파일을 작성

```bash
helm install -f <파일경로> -n test nginx-file-test .
```

2-1. `override_values.yaml` 파일을 아래와 같이 작성
```bash
version: stable-alpine
```

차트 파일 구성
```bash
.
├── Chart.yaml
├── override_values.yaml # 추가됨
├── templates
│   ├── deployment.yaml
│   └── service.yaml
└── values.yaml

1 directory, 5 files
```

---

2-2. `-f` 옵션으로 Helm Chart 설치

```bash
helm install -f override_values.yaml -n test nginx-file-test .
```

실행 결과

```bash
NAME: nginx-file-test
LAST DEPLOYED: Sun Nov 21 07:16:31 2021
NAMESPACE: test
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

배포 결과
```bash
# k get deploy -n test nginx-file-test -o yaml | grep image
...
      - image: nginx:stable-alpine
```

---
<!-- footer: 참고. [nginx configuration](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/#configuration) -->

- 실제 override 할 때는 각 차트의 `configuration` 항목을 참조하여 변경 필요한 값만 지정하여 설치
- 예제. [nginx chart](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/#configuration)

![bg fit right](https://user-images.githubusercontent.com/26294469/142742446-d6feb539-e6fe-4e97-84ef-552482248ca6.png)

---
<!-- header: 8. Usage. Helm Chart 업그레이드 -->
<!-- footer: 참고. [Helm 업그레이드](https://helm.sh/ko/docs/helm/helm_upgrade/) -->

- 기존 릴리즈된 차트 내용을 변경 요구시 사용
- 이력 관리를 위해 사용

1. Helm Chart 기본 설치
```bash
helm install -n test nginx-test .
```
- 결과 확인 (revision)
```bash
helm get all nginx-test
```

![bg fit right 70%](https://user-images.githubusercontent.com/26294469/142742623-62f7f6c4-d87b-46bb-b899-7232f44814cf.png)

---

2. Helm Chart 업그레이드
```bash
helm upgrade --set version=stable-alpine -n test nginx-test .
```

![image](https://user-images.githubusercontent.com/26294469/142742713-afdd2673-2daa-4252-aaa7-8c515c93b82f.png)

- `REVISION`이 올라가고 이미지 배포 버전이 바뀐 것을 알 수 있음
- `values.yaml`을 변경하고 업그레이드를 진행하여도 변경이 됨

---

### 업그레이드시 주의사항

- Helm 업그레이드시 Pod가 재기동됨(RollingUpdate)

```bash
# 1
helm upgrade --set version=stable-alpine -n test nginx-test .

# 2
helm upgrade -n test nginx-test .
```
위 과정을 진행하면, helm 버전에 따라 `values.yaml`참고가 다르다.

- version3: 이전 `revision`을 그대로 따라 버전을 유지
- version2: 이전 `revision` 버전을 유지하지 않는다. (첫 설치 `values.yaml`)

---
<!-- header: 9. Helm Rollback -->
<!-- footer: 참고. [Helm Rollback](https://helm.sh/ko/docs/helm/helm_rollback/) -->

```bash
helm rollback <릴리즈_차트명> <REVISION>
```

실행 결과
```bash
[root@acc-master nginx-chart]# helm rollback -n test nginx-test 1
Rollback was a success! Happy Helming!
[root@acc-master nginx-chart]# helm ls
NAME      	NAMESPACE	REVISION	UPDATED                                	STATUS  	CHART           	APP VERSION
nginx-test	test     	3       	2021-11-21 07:47:41.211560477 +0900 KST	deployed	nginx-test-0.0.1	           
[root@acc-master nginx-chart]# k get deploy -n test nginx-test -o yaml | grep image
                f:image: {}
                f:imagePullPolicy: {}
      - image: nginx:stable
        imagePullPolicy: IfNotPresent
```

- 롤백은 한다고 해도 `revision`은 이력관리를 위해 증가한다.
- 버전이 최초 설치와 동일한 `stable`로 변경된 것을 알 수 있다.