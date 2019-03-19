---
title: "워크스페이스를 위한 IAM 역할 생성"
chapter: false
weight: 25
---


1. [이 딥 링크를 따라 관리자 권한으로 IAM 역할을 만듭니다.](https://console.aws.amazon.com/iam/home#/roles$new?step=review&commonUseCase=EC2%2BEC2&selectedUseCase=EC2&policies=arn:aws:iam::aws:policy%2FAdministratorAccess)
1. **AWS Service** 및 **EC2**가 선택되었는지 확인한 다음 **Next**를 클릭하여 권한을 봅니다.
1. **AdministratorAccess**가 선택되어 있는지 확인한 다음 **Next**를 클릭하여 검토합니다.
1. 이름을 **eksworkshop-admin**라고 입력하고, **Create Role**을 클릭하여 생성합니다.
![createrole](/images/createrole.png)
