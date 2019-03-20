---
title: "EC2 워커 추가하기 - 온디맨드 그리고 스팟 EC2"
date: 2018-08-07T11:05:19-07:00
weight: 10
draft: false
---

이미 EKS 클러스터와 작업자 노드가 있지만 작업자로 구성된 일부 스팟 인스턴스가 필요합니다.
또한 지능적인 스케줄링 결정을 내릴 수 있도록 스팟(Spot)과 주문형(On-Demand)을 식별하는 노드 레이블링 전략이 필요합니다.
[AWS CloudFormation](https://aws.amazon.com/cloudformation/)을 사용하여 EKS 클러스터에 연결할 새 작업 노드를 시작합니다.

This template will create a single ASG that leverages the latest feature to mix multiple instance types and purchase as a single K8s nodegroup.
이 템플릿은 여러 인스턴스 유형으로 구성된 단일 k8s 노드 그룹을 구매하는 최신 기능을 최대한 활용하는 ASG(Auto Scaling Group)를 생성합니다.
다음 블로그를 확인하세요: [New – EC2 Auto Scaling Groups With Multiple Instance Types & Purchase Options](https://aws.amazon.com/tw/blogs/aws/new-ec2-auto-scaling-groups-with-multiple-instance-types-purchase-options/) for details.

#### 워커의 역할 이름 조회

먼저 EKS 워커 노드에서 사용중인 역할 이름을 가져옵니다.

```bash
echo $ROLE_NAME
```

다음 단계에서 매개 변수로 사용할 역할 이름을 복사하세요.
오류 또는 빈 응답이 표시되면 아래의 단계를 펼쳐서 명령을 수행하고 다시 역할 이름을 가져옵니다.

{{%expand "역할 이름을 환경변수로 설정하려면 여기를 클릭하여 펼치세요" %}}

```bash
INSTANCE_PROFILE_PREFIX=$(aws cloudformation describe-stacks | jq -r '.Stacks[].StackName' | grep eksctl-eksworkshop-eksctl-nodegroup)
INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep $INSTANCE_PROFILE_PREFIX)
ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
echo "export ROLE_NAME=${ROLE_NAME}" >> ~/.bash_profile
```

{{% /expand %}}

```text
# Example Output
eksctl-eksworkshop-eksctl-nodegro-NodeInstanceRole-XXXXXXXX
```

#### 보안 그룹 이름 조회
또한 기존 워커 노드와 함께 사용되는 보안 그룹의 ID를 수집해야합니다.

```bash
STACK_NAME=$(aws cloudformation describe-stacks | jq -r '.Stacks[].StackName' | grep eksctl-eksworkshop-eksctl-nodegroup)
SG_ID=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME --logical-resource-id SG | jq -r '.StackResources[].PhysicalResourceId')
echo $SG_ID
```

```text
# Example Output
sg-0d9fb7e709dff5675
```

#### CloudFormation 스택 실행

CloudFormation 템플릿을 새로운 작업자 노드 집합으로 실행하지만 **eksctl** 도구로 만든 노드 그룹의 CloudFormation 스택을 업데이트 할 수도 있습니다.

**Launch** 버튼을 클릭하여 AWS Management Console에서 CloudFormation 스택을 생성합니다.

| Launch template |  |  |
| ------ |:------:|:--------:|
| EKS Workers - Spot and On Demand |  {{% cf-launch "amazon-eks-nodegroup-with-mixed-instances.yml?stackName=eksworkshop-nodegroup-0" %}} | {{% cf-download "amazon-eks-nodegroup-with-mixed-instances.yml" %}}  |

{{% notice tip %}}
클러스터가 있는 위치에 맞는 리전인지 확인하세요.
Confirm the region is correct based on where you've deployed your cluster.
{{% /notice %}}
콘솔이 열리면 매개 변수를 입력해야합니다. 아래 표를 참조하십시오.
Once the console is open you will need to configure the missing parameters. Use the table below for guidance.

| Parameter | Value |
|-----------|-------|
|Stack Name: | eksworkshop-spot-workers |
|Cluster Name: | eksworkshop-eksctl (or whatever you named your cluster) |
|ClusterControlPlaneSecurityGroup: | Select from the dropdown. It will contain your cluster name and the words **'ControlPlaneSecurityGroup'** |
|NodeInstanceRole: | Use the role name that copied in the step above. (e.g. eksctl-eksworkshop-eksctl-nodegro-NodeInstanceRole-XXXXX)
|UseExistingNodeSecurityGroups: | Leave as **'Yes'** |
|ExistingNodeSecurityGroups: | Use the SG name that copied in the step above. (e.g. sg-0123456789abcdef)
|NodeImageId: | Visit this [**link**](https://docs.aws.amazon.com/eks/latest/userguide/eks-optimized-ami.html) and select the non-GPU image for your region - **Check for empty spaces in copy/paste**|
|KeyName: | SSH Key Pair created earlier or any valid key will work |
|NodeGroupName: | Leave as **spotworkers** |
|VpcId: | Select your workshop VPC from the dropdown |
|Subnets: | Select the 3 **private** subnets for your workshop VPC from the dropdown |
|BootstrapArgumentsForOnDemand: | `--kubelet-extra-args --node-labels=lifecycle=OnDemand` |
|BootstrapArgumentsForSpotFleet: | `--kubelet-extra-args '--node-labels=lifecycle=Ec2Spot --register-with-taints=spotInstance=true:PreferNoSchedule'` |

#### 부트 스트랩 인자에 대한 이해

EKS Bootstrap.sh 스크립트는 우리가 사용하고있는 EKS Optimized AMI에 패키지되어 있으며 **EKS 클러스터 이름**과 같은 단일 입력 만 필요합니다.
부트스트랩 스크립트는 런타임에 `kubelet-extra-args`를 설정하는 것을 지원합니다. 쿠버네티스가 프로비저닝한 노드의 유형을 알 수 있도록 **노드 레이블**을 구성했습니다.
노드의 **수명주기**(레이블의 키값)를 온디맨드와 EC2스팟으로 설정했습니다.
스팟 인스턴스에 포드가 스케줄되지 않는 것을 선호하기 때문에 [테인트](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)를 **PreferNoSchedule**으로 설정합니다.
이것은 **NoSchedule**의 "소프트" 버전입니다. 시스템은 해당 노드에 파드를 위치시키는 것을 최대한 피하지만, 다른 곳에 위치시킬 수 없는 경우에는 위치시킬 수 있습니다.

나머지 기본 매개 변수는 그대로 두고 CloudFormation 화면을 계속 진행합니다.
**AWS CloudFormation이 IAM 리소스를 생성 할 수 있음을 확인합니다** 옆의 체크박스를 선택하고 **Create**를 클릭하십시오.

{{% notice info %}}
The creation of the workers will take about 3 minutes.
{{% /notice %}}

#### 노드 확인

새 노드가 클러스터에 올바르게 결합되었는지 확인하세요. 클러스터에 추가 된 2-3 개의 노드가 추가로 표시되어야합니다.

```bash
kubectl get nodes
```

![All Nodes](/images/spotworkers/spot_get_nodes.png)
노드 레이블을 사용하여 노드의 수명주기를 식별 할 수 있습니다.

```bash
kubectl get nodes --show-labels --selector=lifecycle=Ec2Spot
```

이 명령으로 2개의 노드 정보가 출력되어야 합니다. 노드 출력의 끝에서 노드 레이블 **lifecycle=Ec2Spot**을 볼 수 있습니다.

![Spot Output](/images/spotworkers/spot_get_spot.png)


이제 **lifecycle=OnDemand**로 모든 노드를 보여줍니다. 이 명령의 출력은 CloudFormation 템플릿에 구성된 대로 노드 1개를 반환해야합니다.

```bash
kubectl get nodes --show-labels --selector=lifecycle=OnDemand
```

![OnDemand Output](/images/spotworkers/spot_get_od.png)

특정 spot 노드 중 하나를 `kubectl describe nodes` 명령어를 이용하여 EC2 스팟 인스턴스에 적용된 taints를 확인할 수 있습니다.
![Spot Taints](/images/spotworkers/instance_taints.png)
