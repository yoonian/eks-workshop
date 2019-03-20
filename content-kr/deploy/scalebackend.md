---
title: "백엔드 서비스 스케일링"
date: 2018-09-18T17:40:09-05:00
weight: 50
---

서비스 시작시에 각각의 컨테이너를 딱 1개씩만 런칭했었습니다. 
이는 운영 중인 파드를 살펴보면 확인할 수 있습니다.
```
kubectl get deployments
```

이제 백엔드 서비스를 스케일 업 합니다.
```
kubectl scale deployment ecsdemo-nodejs --replicas=3
kubectl scale deployment ecsdemo-crystal --replicas=3
```
디폴로이먼트를 다시 확인해봅니다.
```
kubectl get deployments
```

또한 브라우저에서 실행 중인 우리 애플리케이션을 확인합니다. 
이제는 여러 백엔드 서비스로 트래픽이 흐르는 것을 볼 수 있어야 합니다.
