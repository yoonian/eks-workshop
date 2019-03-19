---
title: "워크스페이스 생성하기"
chapter: false
weight: 10
---

{{% notice warning %}}
Cloud9 워크스페이스는 루트 계정 사용자가 아닌 관리자 권한을 가진 IAM 사용자가 작성해야합니다. 루트 계정 사용자가 아닌 IAM 사용자로 로그인했는지 확인하십시오.
{{% /notice %}}

<!---
{{% notice info %}}
This workshop was designed to run in the **Oregon (us-west-2)** region. **Please don't
run in any other region.** Future versions of this workshop will expand region availability,
and this message will be removed.
{{% /notice %}}
-->

{{% notice tip %}}
광고 차단기, 자바 스크립트 비활성화 도구 및 추적 차단기를 비활성화 해야하며, 그렇지 않으면 워크스페이스 연결에 영향을 줄 수 있습니다.
Cloud9은 `third-party-cookies`가 필요합니다. [특정 도메인]( https://docs.aws.amazon.com/cloud9/latest/user-guide/troubleshooting.html#troubleshooting-env-loading)을 허용 리스트로 관리할 수 있습니다.
{{% /notice %}}

### 가까운 리전에서 Cloud9 실행하기:
{{< tabs name="Region" >}}
{{{< tab name="Singapore" include="ap-southeast-1.md" />}}
{{{< tab name="Oregon" include="us-west-2.md" />}}
{{{< tab name="Ohio" include="us-east-2.md" />}}
{{{< tab name="Ireland" include="eu-west-1.md" />}}
{{< /tabs >}}

- **Create environment** 버튼을 선택합니다.
- 이름을 **eksworkshop**으로 입력하고, 나머지 설정은 기본값으로 진행합니다.
- 실행이 완료되면, **welcome tab**과 **하단 작업 영역**을 닫고 새로운 **터미널** 화면을 메인 작업 영역에 열어둡니다.

![c9before](/images/c9before.png)

- 아래의 화면과 같아야 합니다:
![c9after](/images/c9after.png)

- 이 테마가 마음에 들면 Cloud9 작업 공간 메뉴에서 View / Themes / Solarized / Solarized Dark를 선택하여 직접 선택할 수 있습니다.
