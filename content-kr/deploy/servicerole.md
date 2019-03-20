---
title: "ELB 서비스 롤 존재 확인"
date: 2018-09-18T17:40:09-05:00
weight: 29
---

이전에 로드 밸런서를 생성하지 않은 AWS 계정인 경우, 
ELB의 서비스 롤(role)이 아직 존재하지 않을 수 있습니다.

롤을 확인하고 누락된 경우에는 만듭니다.

다음의 명령어를 귀하의 클라우드9 워크스페이스에 복사/붙여넣기 합니다.

```
aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"
```
