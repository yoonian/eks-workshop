---
title: "워크스페이스의 IAM 설정 업데이트"
chapter: false
weight: 30
---

{{% notice info %}}
Cloud9는 일반적으로 IAM 자격 증명을 동적으로 관리합니다.
현재 aws-iam-authenticator 플러그인과 호환되지 않으므로 이를 비활성화하고 대신 IAM 역할을 사용합니다.
{{% /notice %}}

- 작업 공간으로 돌아가서 우측상단의 톱니바퀴 버튼을 클릭하거나 새 탭을 실행하여 환경설정 탭을 엽니다.
- **AWS SETTINGS**을 선택합니다.
- **AWS managed temporary credentials** 설정을 끕니다.
- 환경설정 탭을 닫습니다.

![c9disableiam](/images/c9disableiam.png)

- 임시 자격 증명이 없는지 확실히 하기 위해 기존의 자격 증명 파일도 제거합니다 :
```
rm -vf ${HOME}/.aws/credentials
```

- 현재 리전을 기준으로 `aws-cli`를 구성해야 합니다 :
```
export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
echo "export AWS_REGION=${AWS_REGION}" >> ~/.bash_profile
aws configure set default.region ${AWS_REGION}
aws configure get default.region
```

### IAM 역할 확인

[GetCallerIdentity](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html) CLI 명령을 사용하여 Cloud9 IDE가 올바른 IAM 역할을 사용하는지 확인합니다.

먼저 AWS CLI에서 IAM 역할 이름을 가져옵니다.
```bash
INSTANCE_PROFILE_NAME=`basename $(aws ec2 describe-instances --filters Name=tag:Name,Values=aws-cloud9-${C9_PROJECT}-${C9_PID} | jq -r '.Reservations[0].Instances[0].IamInstanceProfile.Arn' | awk -F "/" "{print $2}")`
aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME --query "InstanceProfile.Roles[0].RoleName" --output text
```

역할 이름이 출력됩니다.

```output
eksworkshop-admin
or
modernizer-workshop-cl9
```

그 결과를 다음과 비교해보세요.

```bash
aws sts get-caller-identity
```

#### 유효한 경우

_Arn_ 이 위의 역할 이름과 인스턴스 ID를 포함하는 경우 계속 진행할 수 있습니다.

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

#### 문제가 있는 경우

_Arn_ 이 `TeamRole`, `MasterRole`을 포함하거나 역할 이름과 일치하지 않으면 <span style="color: red;">**더 이상 진행하지 마세요**</span>. 돌아가서 이 페이지의 지시사항을 다시 확인하십시오.

```output
{
    "Account": "123456789012", 
    "UserId": "AROA1SAMPLEAWSIAMROLE:i-01234567890abcdef", 
    "Arn": "arn:aws:sts::123456789012:assumed-role/TeamRole/MasterRole"
}
```
