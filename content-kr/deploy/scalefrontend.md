---
title: "프론트엔드 스케일링"
date: 2018-09-18T17:40:09-05:00
weight: 60
---

또한 프론트엔드 서비스도 같은 방식으로 스케일링 합니다.
```
kubectl get deployments
kubectl scale deployment ecsdemo-frontend --replicas=3
kubectl get deployments
```

브라우저에서 실행 중인 우리 애플리케이션을 확인합니다. 
이제는 여러 프론트엔드 서비스로 트래픽이 흐르는 것을 볼 수 있어야 합니다.
