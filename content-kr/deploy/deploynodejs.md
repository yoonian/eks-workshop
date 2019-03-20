---
title: "NodeJS 백엔드 API 배포"
date: 2018-09-18T17:39:30-05:00
weight: 10
---

NodeJS 백엔드 API를 올려 봅시다!

다음의 명령어를 귀하의 클라우드9 워크스페이스에 복사/붙여넣기 합니다.

```
cd ~/environment/ecsdemo-nodejs
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
```

디폴로이먼트 상황을 확인하여 배포 진행 과정을 지켜 볼 수 있습니다.
```
kubectl get deployment ecsdemo-nodejs
```
