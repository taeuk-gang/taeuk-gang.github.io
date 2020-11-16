---
x5title: kind를 이용한 클러스터 설치
toc: true
date: 2020-11-15 23:43:18
tags:
    - Kubernetes
categories:
    - Kubernetes
---

# kind를 이용한 클러스터 설치

## Download

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.8.1/kind-$(uname)-amd64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind # e.g. mv ./kind /usr/local/bin/kind
```



## Basic command

기본값으로 `kind-kind` 의 이름으로 클러스터가 생성됨

```bash
kind create cluster
```

### 확인방법

```
kubectl config get-contexts
```

현재는 이미 생성되어있는 클러스터 목록들

![image-20201115234340989](https://i.loli.net/2020/11/15/zBgZDsH9qCwoLfM.png)



### config

```bash
kind create cluster --config config.yaml
```

config.yaml

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
networking:
  apiServerPort: 6443
  apiServerAddress: 0.0.0.0
nodes:
- role: control-plane
```



### 이름 설정

기본 명령어만 치면, kind라는 이름의 클러스터명으로만 생성이 되기 때문에

```bash
kind create cluster --naeme <클러스터명>
```

위의 명령어를 이용해 클러스터명을 지정할 수 있다.



## 결론

위와 같이 kind를 이용하여, 클러스터를 생성하고 관리하면 쿠버네티스 오브젝트를 독립적으로 관리할 수 있다.

예를 들면, `kubectl api-resources` 를 이용하여 확인 했을 경우, 클러스터 마다 다른 것을 알 수있다.

확인은 아래의 예시를 통해 알 수 있다.

### 예시1. kind-rancher 클러스터의 경우, 오브젝트 목록

![image-20201115234913462](https://i.loli.net/2020/11/15/Rqf7glkJU2WcvA3.png)

### 예시2. kind-kubesphere 클러스터의 경우, 오브젝트 목록

![image-20201115234956138](https://i.loli.net/2020/11/15/A2ijmK4aWO8Ulg1.png)