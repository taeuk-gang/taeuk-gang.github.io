```
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:41620
  name: kind-host
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://127.0.0.1:45027
  name: kind-kubesphere
contexts:
- context:
    cluster: kind-host
    user: kind-host
  name: kind-host
- context:
    cluster: kind-kubesphere
    user: kind-kubesphere
  name: kind-kubesphere
- context:
    cluster: kind-host
    user: oidc
  name: oidc@kind-host
current-context: kind-kubesphere
kind: Config
preferences: {}
users:
- name: kind-host
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: kind-kubesphere
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: oidc
  user:
    auth-provider:
      config:
        client-id: kubernetes
        client-secret: cefabde4-d71c-4ebd-a0e4-d613da8b2c24
        extra-scopes: groups
        id-token: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI5ZXhVUDVOWE9NQU1MbG9BREc3RjV2VEl6X1Y1QWxBdGU0ZzAyLXVGOWlnIn0.eyJleHAiOjE2MDQ4Mzc5ODQsImlhdCI6MTYwNDgzNzY4NCwiYXV0aF90aW1lIjowLCJqdGkiOiI2NDBlYjY2MS1hMjM4LTQwMWEtYjA2Yi00MDk1YTBmZjYxNzYiLCJpc3MiOiJodHRwczovLzEwLjIwLjIwMC4yMDA6ODQ0My9hdXRoL3JlYWxtcy9tYW50ZWNoIiwiYXVkIjoia3ViZXJuZXRlcyIsInN1YiI6IjM0NTQ3ZTVlLTA0ZTgtNDg2OC1hYTRkLTFkMGJjMzY4NWJjMSIsInR5cCI6IklEIiwiYXpwIjoia3ViZXJuZXRlcyIsInNlc3Npb25fc3RhdGUiOiIzMGJjMmRmNS1kODZjLTRlM2YtODViYy01ODA3ZTIwZjU4Y2IiLCJhdF9oYXNoIjoiYXVNQU1seU9Jdk12LTh4SDFReVc4QSIsImFjciI6IjEiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJqanBhcmsifQ.E0lelPKmcttW0ozYHwpXyByv8attpLvXwQRH3VW4EZ_jFva0z_riSXhC8wERMiu09mcn6_ZUIIBQ9QhuQsXXMCjEIsFdr_VhOpIWxjfdQPL1nsEDs3jHD5yMggqKBstEhBJmeLEOa7FUV8tiPXgNdIMnM0en2eOqGL2RmdhX5XWxIiv7v_G7wKRfiICIW1f-X8oHX5KbJRqIyYjSuZgtu_q0kjwLz1KdvA39SjoIdS-GuUTJk6wMQqgXxzg0l5hMVbyVBfz7Q9cSa5r6Q9iO4uP3WiU1zCyovzY_5b1UStp1uThZN6No8WZdcJjpq0hGrmmTX1NcLLLDjXQzMPHM5w
        idp-issuer-url: https://10.20.200.200:8443/auth/realms/kubernetes
        refresh-token: eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI5Y2RlOWU2Mi1jMDEyLTQxZjQtOWI3OC1mNThlYzUzZDE2MDgifQ.eyJleHAiOjE2MDQ4Mzk0ODQsImlhdCI6MTYwNDgzNzY4NCwianRpIjoiMTI1NjUwMmQtMzc1YS00OTY0LTlkMDktN2M0Nzg1MjFiNTEzIiwiaXNzIjoiaHR0cHM6Ly8xMC4yMC4yMDAuMjAwOjg0NDMvYXV0aC9yZWFsbXMvbWFudGVjaCIsImF1ZCI6Imh0dHBzOi8vMTAuMjAuMjAwLjIwMDo4NDQzL2F1dGgvcmVhbG1zL21hbnRlY2giLCJzdWIiOiIzNDU0N2U1ZS0wNGU4LTQ4NjgtYWE0ZC0xZDBiYzM2ODViYzEiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoia3ViZXJuZXRlcyIsInNlc3Npb25fc3RhdGUiOiIzMGJjMmRmNS1kODZjLTRlM2YtODViYy01ODA3ZTIwZjU4Y2IiLCJzY29wZSI6Im9wZW5pZCBwcm9maWxlIGVtYWlsIn0._u2xfgoA8FBrN9eitZGZYgAsTH0oNabIkUZB6dCRxSw
      name: oidc

```

