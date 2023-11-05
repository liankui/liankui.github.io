---
title: 容器A在宿主机中启动容器B的方式
layout: post
nav_order: 1
parent: Docker
---

# docker容器内启动宿主机中另一个容器的2种方式
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

## 1. 命令行方式
1. 配置docker将监听0.0.0.0:2375和unix:///var/run/docker.sock地址，允许通过网络连接（请注意这可能会存在安全风险，因为Docker API会以明文传输）以及通过Unix套接字进行通信。

```bash
vi /lib/systemd/system/docker.service

# 更新ExecStart，添加$DOCKER_NETWORK_OPTIONS -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
...
ExecStart=/usr/bin/dockerd --log-level=error $DOCKER_NETWORK_OPTIONS -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock...
``` 
2. 重新加载Docker服务以使更改生效
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```
## 2.docker SDK
