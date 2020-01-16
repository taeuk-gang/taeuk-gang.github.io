---
title: 쿠버네티스 + 젠킨스 설치해보기
toc: true
date: 2020-01-16 17:15:23
tags:
    - Kubernetes
	- Jenkins
categories:
    - Kubernetes
---

## 쿠버네티스 + 젠킨스 설치해보기

[예제 따라 진행](https://cloud.google.com/solutions/jenkins-on-kubernetes-engine-tutorial?hl=ko)

### 1. Kubernetes 클러스터 만들기

GCP에서 쉽게 GUI로 클러스터 만들기 가능

### 2. cloud shell 활성화

윈도우에서 진행하기에 어려움이 많아, gcp에서 제공하는 cloud shell을 사용하여 진행

#### config

```bash
gcloud config set project taeuk-project
```

![image](https://user-images.githubusercontent.com/26294469/72502741-5e287a80-387d-11ea-93df-b23d67e228cc.png)

```bash
git clone https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes.git
```

![image](https://user-images.githubusercontent.com/26294469/72502807-8adc9200-387d-11ea-8772-6181818bf06a.png)

### 3. 클러스터 실행 확인 (확인할 것!)

```bash
# 실행 확인
gcloud container clusters list

# 연결 가능한지 확인
kubectl cluster-info
```

### 4. Helm 설치

#### Helm 바이너리 파일 다운로드

```bash
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.14.1-linux-amd64.tar.gz
```

#### 압축풀기

```bash
tar zxfv helm-v2.14.1-linux-amd64.tar.gz
cp linux-amd64/helm .
```

#### 젠킨스 클러스터 관리자 추가

```bash
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin \
        --user=$(gcloud config get-value account)
```

#### 서버 Tiller에 `cluster-admin` 역할 부여

```bash
kubectl create serviceaccount tiller --namespace kube-system
kubectl create clusterrolebinding tiller-admin-binding --clusterrole=cluster-admin \
    --serviceaccount=kube-system:tiller
```

#### Helm 초기화 & 업데이트

```bash
./helm init --service-account=tiller
./helm repo update
```

#### 설치 확인

```bash
./helm version
```

![image](https://user-images.githubusercontent.com/26294469/72504837-f58fcc80-3881-11ea-93ce-8d53fdf75036.png)

### 5. Jenkins 설치

#### Helm을 이용하여 설치

```bash
./helm install -n cd stable/jenkins -f jenkins/values.yaml --version 1.2.2 --wait
```

#### 실행 확인

```bash
kubectl get pods
```

#### Jenkins UI 포트 설정(8080 포트로 연결)

``` bash
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
```

#### Jenkins 서비스 생성 확인

```bash
kubectl get svc
```

![image](https://user-images.githubusercontent.com/26294469/72505358-1f95be80-3883-11ea-99de-0560849b5976.png)

### Jenkins 연결

#### 관리자 비밀번호 검색

```bash
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

#### 웹 미리보기를 통해, 젠킨스 출력 확인

![image](https://user-images.githubusercontent.com/26294469/72505406-389e6f80-3883-11ea-9e87-79a2ecc8735f.png)

![image](https://user-images.githubusercontent.com/26294469/72505465-54a21100-3883-11ea-84d3-1155e6df996c.png)



-----------



### 6. Jenkins + Bitbucket 연결

#### 1. 블루오션 접속 후, `파이프라인 생성` 버튼 클릭

#### 2. 소스관리를 BitBucket으로 설정 후 연결

![71635673-c9f9c900-2c69-11ea-8f9f-180f07c87f8c](https://user-images.githubusercontent.com/26294469/71635840-7d16f200-2c6b-11ea-9c1a-1075e65b09ca.png)



### 참고링크

[Jenkins를 사용하여 Google Kubernetes Engine에 지속적으로 배포](https://cloud.google.com/solutions/continuous-delivery-jenkins-kubernetes-engine?hl=ko)

[Kubernetes Engine에서 Jenkins 사용](https://cloud.google.com/solutions/jenkins-on-kubernetes-engine?hl=ko)

[Google Kubernetes Engine에서 Jenkins 설정](https://cloud.google.com/solutions/jenkins-on-kubernetes-engine-tutorial?hl=ko)

