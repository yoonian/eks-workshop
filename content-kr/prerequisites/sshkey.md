---
title: "SSH 키 생성하기"
chapter: false
weight: 21
---

{{% notice info %}}
여기에서부터, 아래처럼 박스에 보이는 명령어들을 Cloud9 IDE에 입력하게 됩니다.
** 클립 보드에 복사 ** 기능 (오른쪽 위 모서리)을 사용하여 간단하게 복사하여 Cloud9에 붙여 넣을 수 있습니다. 붙이기 위해서는 Windows의 경우 Ctrl + V를, Mac의 경우 Command + V를 사용할 수 있습니다.
{{% /notice %}}

Cloud9에서 SSH 키를 생성하려면 아래 명령을 실행하세요. 필요한 경우 이 키는 작업자 노드 인스턴스에서 ssh 액세스를 허용하는 데 사용됩니다.

```bash
ssh-keygen
```

{{% notice tip %}}

`enter`키를 세 차례 누르게 되면 기본 옵션으로 키가 생성됩니다.
{{% /notice %}}

공개 키를 사용중인 EC2 리전에 업로드하세요:

```bash
aws ec2 import-key-pair --key-name "eksworkshop" --public-key-material file://~/.ssh/id_rsa.pub
```