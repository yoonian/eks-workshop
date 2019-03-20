---
title: "서비스(service) 주소 찾기"
date: 2018-09-18T17:40:09-05:00
weight: 40
---

이제`type : LoadBalancer`로 실행중인 서비스(service)가 있으니, ELB의 주소를 알아봐야 합니다. 
이를 위해 kubectl로 `get services` 하면 됩니다.

```
kubectl get service ecsdemo-frontend
```

위 명령어 출력은 ELB의 FQDN을 보여줄 만큼 필드가 길지 않습니다. 
아래 명령으로 출력 형식을 조정할 수 있습니다.
```
kubectl get service ecsdemo-frontend -o wide
```

이 데이터를 프로그램적으로 사용하기 원한다면, json 형식으로 출력할 수 있습니다. 
다음은 json 출력을 사용하는 예제 입니다.
```
ELB=$(kubectl get service ecsdemo-frontend -o json | jq -r '.status.loadBalancer.ingress[].hostname')

curl -m3 -v $ELB
```
{{% notice tip %}}
ELB가 준비되고 트래픽을 프론트엔드 파드로 전송하기까지는 몇십초가 걸립니다.
{{% /notice %}}

loadBalancer 호스트네임을 웹 브라우저에 카피/붙여넣기해서 우리의 애플리케이션이 실행중인지 확인합니다. 
다음 페이지에서 서비스를 스케일 업할 때까지 이 탭을 열어둡니다.
