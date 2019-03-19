---
title: "워크스페이스에 IAM 역할 부여하기"
chapter: false
weight: 26
---

1. [이 딥 링크를 따라 Cloud9 EC2 인스턴스를 찾습니다.](https://console.aws.amazon.com/ec2/v2/home?#Instances:tag:Name=aws-cloud9-eksworkshop*;sort=desc:launchTime)
1. 인스턴스를 선택하고 **Actions / Instance Settings / Attach/Replace IAM Role** 순서대로 메뉴를 선택합니다.
![c9instancerole](/images/c9instancerole.png)
1. **IAM Role** 드랍 다운 리스트에서 **eksworkshop-admin**을 선택하고 적용합니다.
![c9attachrole](/images/c9attachrole.png)
