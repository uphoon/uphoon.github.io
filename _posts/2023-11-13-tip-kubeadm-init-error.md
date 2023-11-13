---
layout: post
title: Mater Node  설정 중 에러[ERROR CRI] 해결
tags:
  - 쿠버네티스
  - kubernetes
  - 마스터노드
  - kubeadm
  - TIP
  - error_cri
---

>*쿠버네티스 설치하고 `Kubeadm 초기화` 중에 아래와 같은 에러 발생한다.*

![error_cri](https://github.com/uphoon/uphoon.github.io/releases/download/posts/error_cri.png "ERROR CRI")

```bash
[ERROR CRI]: container runtime is not running: output: time="2023-11-13T10:37:30+09:00" level=fatal msg="validate service connection: CRI v1 runtime API is not implemented for endpoint \"unix:///var/run/containerd/containerd.sock\": rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService"
, error: exit status 1
```

## 원인

1. "container runtime is not running": 컨테이너 런타임이 실행되고 있지 않다는 오류
2. "validate service connection: CRI v1 runtime API is not implemented for endpoint "unix:///var/run/containerd/containerd.sock"": CRI v1 런타임 API가 "unix:///var/run/containerd/containerd.sock" 소켓 경로에 대해 구현되지 않았다는 것
3. "rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService": CRI v1 런타임 서비스가 알 수 없는 서비스(runtime.v1.RuntimeService)로 구현되지 않았다는 오류


## 해결

>*Kubernetes 설정에서 CRI 설정을 올바른 소켓 경로 수정 및 API 버전을 사용*

1. 아래의 경로 파일 열기
```bash
   vim /etc/containerd/config.toml
```

2. config.toml 에 파일 수정 (내용 추가)
```bash

enabled_plugins = ["cri"]
[plugins."io.containerd.grpc.v1.cri".containerd]
  endpoint = "unix:///var/run/containerd/containerd.sock"

```

![config_toml](https://github.com/uphoon/uphoon.github.io/releases/download/posts/config_toml.png "config toml")

3. 컨테이너 데몬 재시작
```bash
sudo systemctl restart containerd
```


>*kubeadm 초기화 다시 실행 하였다*

```bash
# sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --v=5
```

#### 참조 : (해결을 위한)

See https://github.com/containerd/containerd/issues/8139

