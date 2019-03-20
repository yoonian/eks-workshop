---
title: "서비스(service) 종류 확인"
date: 2018-09-18T17:40:09-05:00
weight: 25
---

프론트엔드 서비스를 올리기 전에 
우리가 사용할 서비스(service) 종류를 살펴봅니다.
이것은 `kubernetes/service.yaml`으로 프론트엔드 서비스를 위한 설정입니다.
```
apiVersion: v1
kind: Service
metadata:
  name: ecsdemo-frontend
spec:
  selector:
    app: ecsdemo-frontend
  type: LoadBalancer
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 3000
```
`type: LoadBalancer`을 주목하십시요. 이는 ELB를 설정하여 서비스로 들어오는 
트래픽을 처리하게 합니다.

백엔드 서비스를 위한 `kubernetes/service.yaml`과 비교합니다.
```
apiVersion: v1
kind: Service
metadata:
  name: ecsdemo-nodejs
spec:
  selector:
    app: ecsdemo-nodejs
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 3000
```
프론트엔드와는 달리 특정 서비스 종류를 설정하지 않았습니다. 
[쿠버네티스 공식 문서](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)에 따르면 기본 서비스 종류는 `ClusterIP` 입니다. 
이는 클러스터 내부 IP를 서비스로 노출한다. 이 값을 선택하면 클러스터 내부에서만 해당 서비스에 접근할 수 있습니다.
