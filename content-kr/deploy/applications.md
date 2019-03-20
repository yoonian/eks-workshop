---
title: "예제 애플리케이션 배포"
date: 2018-09-18T16:01:14-05:00
weight: 5
---

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ecsdemo-nodejs
  labels:
    app: ecsdemo-nodejs
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ecsdemo-nodejs
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: ecsdemo-nodejs
    spec:
      containers:
      - image: brentley/ecsdemo-nodejs:latest
        imagePullPolicy: Always
        name: ecsdemo-nodejs
        ports:
        - containerPort: 3000
          protocol: TCP
```
위에 예제 파일에는 서비스(service)와 이것이 *어떻게* 배포되는지를 기술되어 있습니다.
이 내용을 kubectl을 이용하여 쿠버네티스 API에 쓰면, 
쿠버네티스는 응용 프로그램이 배포될 때에 우리의 설정에 충족시킬 것입니다.

컨테이너는 3000번 포트로 수신 대기하고, 
네이티브 서비스 디스커버리는 운영중인 컨테이너를 찾고 해당 컨테이너와 통신하는데 사용합니다.
