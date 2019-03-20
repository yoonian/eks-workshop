---
title: "스팟에 애플리케이션 배포하기"
date: 2018-09-18T17:40:09-05:00
weight: 30
draft: false
---

마이크로서비스 예제를 다시 설계합니다. 스팟 인스턴스가 사용 가능하다면 프론트앤드 서비스를 스팟에 배포하도록 합니다.
이 설정을 위해서 매니패스트 파일에 노드 어피니티(Node Affinity) 설정을 이용할 것입니다.


#### 노드 어피니티 및 톨러레이션 설정

Cloud9 에디터에서 디플로이먼트 메니페스트 파일을 엽니다. **~/environment/ecsdemo-frontend/kubernetes/deployment.yaml**

스펙을 편집하여 노트 어피니티를 스팟 인스턴스를 **필수(require)**가 아닌 **선호(prefer)**하도록 구성하십시오.
이렇게 하면 사용 가능한 스팟 인스턴스가 없거나 올바르게 레이블 된 경우 온디맨드 노드에 파드를 스케줄 할 수 있습니다.

또한 포드가 EC2 스팟 인스턴스에서 구성한 테인트를 "허용"할 수 있도록 톨러레이션(허용 범위)을 구성하려고 합니다.

노드 어피니티 예제는 다음 [**링크**](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity)를 확인하세요.

테인트와 톨러레이션 예제는 다음 [**링크**](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)를 확인하세요.

#### 도전

**어피니티와 톨러레이션 설정**

{{% expand "솔루션을 보려면 여기를 펼치세요"%}}
아래의 코드를 디플로이먼트 파일의 spec.template.spec 아래에 추가하세요.

```yaml
      affinity:
        nodeAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 1
            preference:
              matchExpressions:
              - key: lifecycle
                operator: In
                values:
                - Ec2Spot
      tolerations:
      - key: "spotInstance"
        operator: "Equal"
        value: "true"
        effect: "PreferNoSchedule"
```

{{% notice tip %}}
아래에 솔루션 파일이 있습니다. 직접 작성한 코드와 비교해보세요.
{{% /notice %}}

{{% /expand %}}

{{%attachments title="Related files" pattern=".yml"/%}}

#### 프론트앤드 서비스 스팟에 재배포

먼저 스팟 인스턴스에 배포된 모든 파드를 확인해보겠습니다.

```bash
 for n in $(kubectl get nodes -l lifecycle=Ec2Spot --no-headers | cut -d " " -f1); do kubectl get pods --all-namespaces  --no-headers --field-selector spec.nodeName=${n} ; done
```

이제 편집된 프론트앤드 메니페스트로 마이크로서비스를 재배포합니다.

```bash
cd ~/environment/ecsdemo-frontend
kubectl apply -f kubernetes/service.yaml
kubectl apply -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-crystal
kubectl apply -f kubernetes/service.yaml
kubectl apply -f kubernetes/deployment.yaml

cd ~/environment/ecsdemo-nodejs
kubectl apply -f kubernetes/service.yaml
kubectl apply -f kubernetes/deployment.yaml
```

스팟 인스턴스에 배포된 모드 파드를 확인할 수 있으며, 프론트앤드 파드가 스팟 인스턴스 위에서 실행되는 것을 볼 수 있습니다.

```bash
 for n in $(kubectl get nodes -l lifecycle=Ec2Spot --no-headers | cut -d " " -f1); do kubectl get pods --all-namespaces  --no-headers --field-selector spec.nodeName=${n} ; done
```
