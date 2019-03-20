---
title: "스팟 인터럽트 핸들러 배포"
date: 2018-08-07T12:32:40-07:00
weight: 20
draft: false
---

이 섹션에서는 클러스터가 스팟 중단에 대처하도록 준비해볼 예정입니다.
특정 인스턴스 유형의 사용 가능한 온 디맨드 용량이 고갈되면 스팟 인스턴스에 2분 전에 중단 알림이 보내져 정상적으로 마무리됩니다.
각 스팟 인스턴스에 애플리케이션을 탐지하고 클러스터의 다른 곳에 재배포하는 파드를 배포할 것입니다.

우리가 해야 할 첫 번째 일은 각 스팟 인스턴스에 스팟 인터럽트 핸들러를 배치하는 것입니다. 이 핸들러는 중단 알림을 위해 인스턴스에서 EC2 메타 데이터 서비스를 모니터링합니다.

워크플로우를 정리해보면:

* 스팟 인스턴스가 반환이 요청되었는지 확인합니다.
* 노드의 종료를 준비하기 위해 2분 알림 화면을 이용합니다.
* 새로운 파드가 노드에 위치하지 않도록 테인트 설정을 하고 저지선을 구축합니다.
* 실행중인 파드에 대한 연결이 하나씩 빠집니다.
* 원하는 개수를 유지하기 위해서 남아있는 노드에 파드가 재배포됩니다.

여기에 쿠버네티스 데몬셋 예제가 있습니다. 데몬셋은 노드당 하나의 파드를 실행합니다.


```bash
mkdir ~/environment/spot
cd ~/environment/spot
wget https://eksworkshop.com/spot/managespot/deployhandler.files/spot-interrupt-handler-example.yml
```
적혀있는대로, 매니페스트는 할 필요가 없는 온디맨드를 포함한 모든 노드에 파드를 배치합니다.
스팟 인스턴스에만 배포되도록 데몬셋을 편집합니다. 레이블을 사용하여 올바른 노드를 식별해보세요.

`노드셀렉터`를 사용하여 배치를 스팟 인스턴스로 제한하십시오. 자세한 내용은  [**링크**](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/)를 참조하세요.

#### 도전

**노드셀렉터를 사용하여 스팟 핸들러 구성**
{{% expand "솔루션을 보려면 여기를 펼치세요"%}}
아래 코드를 Spec.Template.Spec.nodeSelector 아래의 데몬셋 매니페스트 끝에 작성합니다.

```bash
      nodeSelector:
        lifecycle: Ec2Spot
```

{{% /expand %}}

데몬셋을 배포합니다.

```bash
kubectl apply -f ~/environment/spot/spot-interrupt-handler-example.yml
```

{{% notice tip %}}
DaemonSet을 배포하는 중 오류가 발생하면 YAML 파일에 작은 오류가 있을 수 있습니다.
이 페이지 하단에 있는 솔루션 파일과 비교해 보세요.
{{% /notice %}}

파드를 확인해보세요. 각 스팟 노드 당 하나씩 있어야 합니다.

```bash
kubectl get daemonsets
```

{{%attachments title="Related files" pattern=".yml"/%}}
