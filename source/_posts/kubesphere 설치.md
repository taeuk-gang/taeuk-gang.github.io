# Kubesphere 설치

kind로 하면 더 좋음

https://kubesphere.com.cn/forum/d/1673-kind-kubernetes-kubesphere



![image-20201110110621709](https://i.loli.net/2020/11/10/4MTxPDGgSNaFusm.png)

```
admin
P@88w0rd
```

## 첫화면

![image-20201110110709752](https://i.loli.net/2020/11/10/f3bIsWqpzZmadED.png)



200번 서버

Console: http://172.20.0.3:30880

Account: admin

Password: P@88w0rd

Internal error occurred: failed calling webhook "validating-user.kubesphere.io": Post "https://ks-controller-manager.kubesphere-system.svc:443/validate-email-iam-kubesphere-io-v1alpha2-user?timeout=30s": x509: certificate relies on legacy Common Name field, use SANs or temporarily enable Common Name matching with GODEBUG=x509ignoreCN=0

```
kubectl delete validatingwebhookconfigurations.admissionregistration.k8s.io users.iam.kubesphere.io
```



```

# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: iam.kubesphere.io/v1alpha2
kind: User
metadata:
  annotations:
    iam.kubesphere.io/password-encrypted: "true"
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"iam.kubesphere.io/v1alpha2","kind":"User","metadata":{"annotations":{"iam.kubesphere.io/password-encrypted":"true"},"name":"admin"},"spec":{"email":"admin@kubesphere.io","password":"$2a$10$zcHepmzfKPoxCVCYZr5K7ORPZZ/ySe9p/7IUb/8u./xHrnSX2LOCO"},"status":{"state":"Active"}}
  creationTimestamp: "2020-11-10T04:32:21Z"
  finalizers:
  - finalizers.kubesphere.io/users
  generation: 8
  name: admin
  resourceVersion: "6237"
  selfLink: /apis/iam.kubesphere.io/v1alpha2/users/admin
  uid: b68ca4fe-23db-416d-b7d4-4cdfc14f263e
spec:
  email: admin@kubesphere.io
  password: $2a$10$zcHepmzfKPoxCVCYZr5K7ORPZZ/ySe9p/7IUb/8u./xHrnSX2LOCO
status:
  lastLoginTime: "2020-11-10T04:51:32Z"
  state: Active

```

```
Accordion1
$2a$10$vu/KX6leYBJ6ksy2Qf1aJOlHuedpb4K498e/YCQ.aptGk1wnkYbz6
```

