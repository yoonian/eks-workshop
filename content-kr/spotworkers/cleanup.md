---
title: "깔끔하게 제거하기"
weight: 50
draft: false
---

마이크로서비스 디플로이먼트를 모두 제거합니다.

```bash
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

스팟 핸들러 데몬셋을 제거합니다.

```bash
kubectl delete -f ~/environment/spot/spot-interrupt-handler-example.yml
```

이번 모듈에서 생성된 워커를 모두 제거하기 위해서, 아래의 명령어를 실행합니다.

EKS에서 워커 노드 모두 제거하기:

```bash
aws cloudformation delete-stack --stack-name "eksworkshop-spot-workers"
```
