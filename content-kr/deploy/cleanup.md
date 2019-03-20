---
title: "애플리케이션 정리"
date: 2018-08-07T13:37:53-07:00
weight: 90
---

애플리케이션이 생성한 리소스를 삭제하려면 
해당 애플리케이션의 디폴로이먼트를 삭제해야 합니다.

애플리케이션 디폴로이먼트 삭제
```
cd ~/environment/ecsdemo-frontend
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-crystal
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-nodejs
kubectl delete -f kubernetes/service.yaml
kubectl delete -f kubernetes/deployment.yaml
```
