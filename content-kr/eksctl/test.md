---
title: "클러스터 테스트"
date: 2018-08-07T13:36:57-07:00
weight: 30
---

노드 확인:

```bash
kubectl get nodes
```

실습에 사용할 역할 이름을 환경변수로 설정하세요.

```bash
INSTANCE_PROFILE_PREFIX=$(aws cloudformation describe-stacks | jq -r '.Stacks[].StackName' | grep eksctl-eksworkshop-eksctl-nodegroup)
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep $INSTANCE_PROFILE_PREFIX)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
echo "export ROLE_NAME=${ROLE_NAME}" >> ~/.bash_profile

```

#### 축하합니다!

이제 사용할 준비가 완료된 Amazon EKS 클러스터를 갖게 되었습니다!
