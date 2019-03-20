---
title: "Prerequisites"
date: 2018-08-07T13:31:55-07:00
weight: 10
---

[eksctl](https://eksctl.io/) 바이너리를 다운로드 하세요 :
```
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv -v /tmp/eksctl /usr/local/bin
```

eksct 명령어 동작을 확인하세요 :
```
eksctl version
```
