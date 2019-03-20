---
title: "대시보드 사용"
date: 2018-08-07T08:30:11-07:00
weight: 30
---

이제 쿠버네티스 대시보드를 사용할 수 있습니다.

1. 귀하의 클라우드9 환경에서 **Tools / Preview / Preview Running Application** 을 클릭합니다.
2. **the end of the URL** 를 찾아서 다음을 추가합니다.

```
/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
```

터미널 탭을 열고 다음을 입력합니다.
```
aws-iam-authenticator token -i eksworkshop-eksctl --token-only
```

이 명령의 출력을 복사하고 *Token* 옆에 라디오버튼을 *클릭*하고,
아래 텍스트 필드에 복사한 마지막 명령의 출력을 붙여넣습니다.

![Token 페이지](/images/dashboard-connect.png)

그리고 *Sign In*을 누릅니다.

전체 탭에서 대시보드를 보고 싶으면, 아래처럼 **Pop Out** 버튼을 클릭하세요.
![popout](/images/popout.png)
