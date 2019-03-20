---
title: "EKS 실행"
date: 2018-08-07T13:34:24-07:00
weight: 20
---


{{% notice warning %}}
사용중인 Cloud9 IDE에서 [IAM 역할을 확인](/prerequisites/workspaceiam/#iam-역할-확인)하지 않았다면 **더 이상 진행하면 안됩니다**.
EKS 클러스터가 IAM 역할을 사용하여 빌드되지 않은 경우 나중 모듈에서 필요한 kubectl 명령을 실행할 수 없습니다.
{{% /notice %}}

#### 도전:
**워크스페이스에서 IAM 역할을 확인하려면 어떻게하나요?**

{{%expand "솔루션을 보려면 여기를 펼치세요" %}}
Run `aws sts get-caller-identity` and validate that your _Arn_ contains `eksworkshop-admin` or `modernizer-workshop-cl9` (or the role created when starting the workshop) and an Instance Id.
`aws sts get-caller-identity`를 실행하여 _Arn_ 에`eksworkshop-admin` 또는 `modernizer-workshop-cl9` (또는 워크숍을 시작할 때 생성 된 역할)과 인스턴스 Id가 들어 있는지 확인하십시오.
```output
{
    "Account": "123456789012", 
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef", 
    "Arn": "arn:aws:sts::123456789012:assumed-role/eksworkshop-admin/i-01234567890abcdef"
}
or
{
    "Account": "123456789012", 
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef", 
    "Arn": "arn:aws:sts::123456789012:assumed-role/modernizer-workshop-cl9/i-01234567890abcdef"
}
```

올바른 역할이 아니라면 [IAM 역할을 확인](/prerequisites/workspaceiam/#iam-역할-확인)으로 돌아가서 문제를 해결하세요.

올바른 역할이 표시되면 다음 단계로 진행하여 EKS 클러스터를 만듭니다.
{{% /expand %}}

### EKS 클러스터 만들기

기본 EKS 클러스터를 생성하려면 다음을 실행하세요 :
```
eksctl create cluster --name=eksworkshop-eksctl --nodes=3 --node-ami=auto --region=${AWS_REGION}
```
{{% notice info %}}
EKS와 모든 의존관계들의 실행은 약 15 분이 소요됩니다.
{{% /notice %}}
