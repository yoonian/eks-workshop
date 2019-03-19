---
title: "데이터 플레인"
date: 2018-10-03T10:18:27-07:00
draft: false
weight: 110
---

{{<mermaid>}}
graph TB
internet((internet))
    subgraph worker1
      kubelet1(kubelet)
      kube-proxy1(kube-proxy)
      subgraph docker1
        subgraph podA
          containerA[container]
        end
        subgraph podB
          containerB[container]
        end
      end
    end

  internet-->kube-proxy1
  api-->kubelet1
  kubelet1-->containerA
  kubelet1-->containerB
  kube-proxy1-->containerA
  kube-proxy1-->containerB

  classDef green fill:#9f6,stroke:#333,stroke-width:4px;
  classDef orange fill:#f96,stroke:#333,stroke-width:4px;
  classDef blue fill:#6495ed,stroke:#333,stroke-width:4px;
  class api blue;
  class internet green;
  class kubectl orange;
{{< /mermaid >}}

* 작업 노드로 구성

* kubelet: API 서버와 노드 간에 연결 역할

* kube-proxy: IP 변환과 라우팅 관리

[쿠버네티스 공식 문서](https://kubernetes.io/docs/concepts/overview/components/#node-components)에서 데이터 플레인에 대해 더 깊이 있는 설명을 확인할 수 있습니다.