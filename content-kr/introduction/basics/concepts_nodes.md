---
title: "쿠버네티스 노드"
date: 2018-10-03T10:15:55-07:00
draft: false
weight: 40
---

쿠버네티스 클러스터를 구성하는 머신들은 **노드(node)** 라고 불립니다.

쿠버네티스 클러스터 내 노드들은 물리적이거나 가상 머신일 수 있습니다.

노드에는 두가지 종류가 있습니다:

* [콘트롤 플레인(Control Plane)](../../architecture/architecture_control)을 형성하며, 클러스터의 "두뇌"로 역할하는 마스터 노드(Master-node).

* [데이터 플레인(Data Plane)](../../architecture/architecture_worker)을 형성하며, 파드(pod)들을 통해 실제 컨테이너 이미지들을 작동시키는 워커 노드(Worker-node).

우리는 프레젠테이션을 통해 어떻게 노드들이 서로 상호작용하는지 자세히 알아볼 것입니다.
