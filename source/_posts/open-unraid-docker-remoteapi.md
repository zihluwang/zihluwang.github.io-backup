---
title: 打开 unRaid 系统中 Docker 的 Remote API
date: 2023-03-22 20:34:18
tags: [O&M, 运维]
---

今天需要将一个 Java 项目部署到 Docker 上，奈何本地 Docker 是运行在 unRaid 系统上的。

通过查阅[资料](https://www.cnblogs.com/hongdada/p/11512901.html)，发现开启方法可以使用在 `daemon.json` 中设置，于是便前往修改 `\\$NAS_IP\flash\config\go` 文件如下：

```shell
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'

{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn/"
  ],
  "hosts": [
    "tcp://0.0.0.0:2375",
    "unix:///var/run/docker.sock"
  ]
}

EOF
```

这样即可打开 Docker 的 2375 Remote API 端口。

> 2375 Remote API 端口是不需要用户验证的，千万**不要在公网环境下打开这个端口**！

