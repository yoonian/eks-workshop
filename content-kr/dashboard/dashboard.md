---
title: "쿠버네티스 공식 대시보드 배포"
date: 2018-08-07T08:30:11-07:00
weight: 10
---

쿠버네티스 공식 대시보드는 기본으로 배포되지 않고, 설치 방법이
[공식 문서](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)에 있습니다.

다음 명령어로 대시보드를 배포할 수 있습니다.
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```

이것이 개인 클러스터에 배포되기 때문에 프락시를 통해서 접근해야 합니다.
kube-proxy는 대시보드 서비스로 요청을 프락시할 수 있습니다. 
귀하의 워크스페이스에서 다음 명령어를 실행하세요.
```
kubectl proxy --port=8080 --address='0.0.0.0' --disable-filter=true &
```

이러면 프락시를 시작하고 8080번 포트로 모든 인터페이스에서 접속 대기하며 
비로컬 호스트 요청을 필터링을 사용하지 않도록 설정합니다.

이 명령은 현재 터미널 세션에서 백그라운드에서 계속 실행됩니다.

{{% notice warning %}}
XSRF 공격을 방어하는 보안 기능인 요청 필터링을 비활성화 하였습니다.
이것은 프로덕션 환경에서 권장하지 않지만 개발 환경에서는 유용합니다.
{{% /notice %}}
