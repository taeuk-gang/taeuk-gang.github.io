---
title: TIL 10월 11일 (월)
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

## TIL: 10월 11일 (월)

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
<!-- header: K8S Ingress -->
### K8S Ingress 

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

> 참고 자료: https://arisu1000.tistory.com/27840

## 인그레스 정의

인그레스(ingress)는 클러스터 외부에서 내부로 접근하는 요청들을 어떻게 처리할지 정의해둔 규칙들의 모음

## 할 수 있는 것들

- 외부에서 접근가능한 URL을 사용할 수 있게 함
- 트래픽 로드밸런싱
- SSL 인증서 처리
- 도메인 기반으로 가상 호스팅을 제공

> 이런 규칙들을 실제로 동작하게 해주는게 인그레스 컨트롤러(ingress controller)

---

1. 클라우드 서비스는 별다른 설정없이 자사의 로드밸런서 서비스들과 연동해서 인그레스를 사용

2. 직접 쿠버네티스 클러스터를 구축해서 사용하는 경우라면 인그레스 컨트롤러를 직접 인그레스와 연동

3. 이때 가장 많이 사용되는건 쿠버네티스에서 제공하는  ingress-nginx(https://github.com/kubernetes/ingress-nginx)

---
## YAML 예제
foo.bar.com으로 요청이 들어오더라도 뒷부분의 경로에 따라 /foos1이면 서비스 s1으로 연결되고 /bars2이면 서비스 s2쪽으로 연결

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: test
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foos1
        backend:
          serviceName: s1
          servicePort: 80
      - path: /bars2
        backend:
          serviceName: s2
          servicePort: 80
  - host: bar.foo.com
    http:
      paths:
      - backend:
          serviceName: s2
          servicePort: 80
```

<style scoped>
  pre {
    font-size: 0.5rem;
  }
</style>

---
<!-- header: Cert-manager -->
### Cert-manager

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
<!-- footer: 참고 링크: [Cert-manager Github](https://github.com/jetstack/cert-manager) -->
### 설명

- cert-manager는 k8s에서 자동으로 TLS 인증서를 갱신해주는 쿠버네티스 에드온

- [kube lego](https://github.com/jetstack/kube-lego) 프로젝트에 기초

- 유사 프로젝트로 [kube-cert-manager](https://github.com/PalmStoneGames/kube-cert-manager)가 존재

- 외부 Issuers를 사용하거나 self-signed 방식으로 자동 갱신

> openssl로 안하고 이걸 사용하는 이유는 인증서 자동 갱신 및 관리?

![bg fit right](https://camo.githubusercontent.com/94e6e2096b0bc286c36b61494276534d8f70f5e7e6171587c65832f2c621f688/68747470733a2f2f636572742d6d616e616765722e696f2f696d616765732f686967682d6c6576656c2d6f766572766965772e737667)

---
<!-- footer: 참고 링크: [Cert-manager Official Installation](https://cert-manager.io/docs/installation/) -->

### 설치

`cert-manager` 네임스페이스를 사용

```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.yaml
```

---

### 설치 확인

```bash
[root@stg-accordion1 twkang] k get all -n cert-manager 
NAME                                           READY   STATUS    RESTARTS   AGE
pod/cert-manager-7c6f78c46d-t9j4r              1/1     Running   0          100s
pod/cert-manager-cainjector-668d9c86df-lwhzv   1/1     Running   0          100s
pod/cert-manager-webhook-979688b46-zffvw       1/1     Running   0          100s

NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/cert-manager           ClusterIP   10.98.36.159    <none>        9402/TCP   101s
service/cert-manager-webhook   ClusterIP   10.102.126.21   <none>        443/TCP    101s

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cert-manager              1/1     1            1           101s
deployment.apps/cert-manager-cainjector   1/1     1            1           101s
deployment.apps/cert-manager-webhook      1/1     1            1           101s

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/cert-manager-7c6f78c46d              1         1         1       101s
replicaset.apps/cert-manager-cainjector-668d9c86df   1         1         1       101s
replicaset.apps/cert-manager-webhook-979688b46       1         1         1       101s
```

---
<!-- footer: 참고 링크: [Cert-manager를 이용해 selfsigned 인증서 생성하기](https://ooeunz.tistory.com/143)-->

- Cert-manager는 기본적으로 외부 Issuer(let's enscypt)가 아닌, 
Cluster 내부에서 사용할 수 있는 자체적으로 서명된 self-signed issuer를 생성

> Issuer 뜻 = CA라고 칭하는 서명할 수 있는 주체를 지칭, Certificate(인증서)를 생성할 수 있는 발급기관

- cluster 내부에서 사용하거나 테스트 용도로 사용하는 것이라면 self-signed 인증서를 사용하는 것도 좋은 방법

---

### 시스템 구성도

- cert-manager를 이용해서 클러스터 전역에서 사용할 수 있는 Cluster Issuer를 생성

- 해당 Issuer를 이용해서 각각의 Namespace 별로 Certificate를 생성

> 클러스터/네임스페이스 스코프별로 `ClusterIssuer`, `Issuer` 리소스 따로 존재


![bg fit right 80%](https://user-images.githubusercontent.com/26294469/136773635-974cfd3b-c3ef-4140-8a9c-d89b0423e9a9.png)

---

### Usage

1. Issuer 생성

```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: selfsigned-issuer
spec:
  selfSigned: {}
```

결과

```bash
[root@stg-accordion1 twkang] k get clusterissuers.cert-manager.io -A
NAME                READY   AGE
selfsigned-issuer   True    4s
```

---

2. Certificate 생성

- 생성된 `ClusterIssuer`를 사용해서 self-signed Certificate를 생성
- 해당 Certificate는 속해있는 Namespace 내의 모든 서비스가 사용할 수 있는 인증서가 됨
- 해당 인증서가 생성됨과 동시에 Certificate는 Kubernetes내에서 사용할 수 있도록 Public key, Secret key와 같은 데이터를 가진 secret 리소스가 생성

---
<!-- footer: 참고 링크: [Certificate Official Usage](https://cert-manager.io/docs/usage/certificate/) -->
아래 yaml을 apply 하면 certificate와 secret 리소스가 함께 생성 (v1 기준으로 작성: v1beta1은 yaml 스펙이 다름)

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: selfsigned-cert
  namespace: <네임스페이스명>
spec:
  secretName: selfsigned-cert-tls # Certificate와 동시에 함께 생성되는 secret의 이름
  duration: 2880h # 120d, 인증서의 유효기간
  renewBefore: 360h # 15d, 자동으로 인증서를 갱신할 때를 지정
  commonName: example.com # host name, dnsName 옵션과 함게 사용가능
  # commonName이 설정되지 않았을 경우에는 dnsName의 가장 첫번째 값을 commonName이 기본값
  isCA: false # CA서명이 유효하도록 하는 옵션
  privateKey:
    algorithm: RSA
    encoding: PKCS1
    size: 2048  
  usages: # 참고 https://cert-manager.io/docs/reference/api-docs/#cert-manager.io/v1alpha2.KeyUsage
    - digital signature
    - key encipherment
    - server auth
  issuerRef:
    name: selfsigned-issuer
    kind: ClusterIssuer
    group: cert-manager.io
```

<style scoped>
  pre {
    font-size: 0.6rem;
  }
</style>

---

결과

```yaml
[root@stg-accordion1 twkang] k get certificate -n docs selfsigned-cert 
NAME              READY   SECRET                AGE
selfsigned-cert   True    selfsigned-cert-tls   9s
```

---

3. 생성되는 secret 데이터 확인

- ca.crt: public certificate file
- tls.crt: Public Key
- tls.key: Private Key

```yaml
[root@stg-accordion1 twkang] k get secrets -n docs 
NAME                       TYPE                                  DATA   AGE
selfsigned-cert-tls        kubernetes.io/tls                     3      10m
```

---
<!-- footer: 참고 링크: [Securing Ingress Resources](https://cert-manager.io/docs/usage/ingress/) -->

4. 생성된 TLS Secret을 Ingress에 적용

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    # 1. add an annotation indicating the issuer to use.
    cert-manager.io/cluster-issuer: selfsigned-issuer
  name: myIngress
  namespace: myIngress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - pathType: Prefix
        path: /
        backend:
          service:
            name: myservice
            port:
              number: 80
  tls: # < 2. placing a host in the TLS config will determine what ends up in the cert's subjectAltNames
  - hosts:
    - example.com
    secretName: selfsigned-cert-tls # < 3. cert-manager will store the created certificate in this secret.
```
