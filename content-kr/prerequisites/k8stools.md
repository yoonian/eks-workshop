---
title: "쿠버네티스 도구 설치"
chapter: false
weight: 22
---
Amazon EKS 클러스터에는 쿠버네티스 클러스터에 대한 IAM 인증을 허용하기 위해 `kubectl` 및 `kubelet` 바이너리와 `aws-iam-authenticator` 바이너리가 필요합니다.

{{% notice tip %}}
이번 워크샵에서는 Linux 바이너리를 다운로드하는 명령어를 제공합니다. Mac OSX / Windows를 사용하는 경우 [다운로드 링크는 공식 EKS 문서를 참조](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html)하십시오.
{{% /notice %}}

#### `kubectl` 설정을 저장하기 위한 기본 `~/.kube` 디렉토리 생성
```
mkdir -p ~/.kube
```

#### `kubectl` 설치
```
sudo curl --silent --location -o /usr/local/bin/kubectl "https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/kubectl"
sudo chmod +x /usr/local/bin/kubectl
```

#### AWS IAM Authenticator 설치
```
go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
sudo mv ~/go/bin/aws-iam-authenticator /usr/local/bin/aws-iam-authenticator
```

#### 설치 확인
```
kubectl version --short --client
aws-iam-authenticator help
```

#### JQ 설치
```
sudo yum -y install jq
```
