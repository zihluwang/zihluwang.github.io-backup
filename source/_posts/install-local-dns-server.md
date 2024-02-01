---
title: 安装本地 DNS 服务器
date: 2023-06-28 10:45:56
tags: [运维, 开发]
---

# 安装本地 DNS 服务器的好处

通常，需要为本地服务器配置域名都是以修改 `hosts` 文件进行的，但是在手机上并不方便修改 `hosts` 文件。

要在无法修改 `hosts` 文件的情况下做到域名解析，就只能依靠 `DNS` 服务。

# 采用的方案

- DNSMasq

# 安装步骤

## 检查环境

由于 DNSMasq 预编译版本仅提供 Linux 环境下的二进制包，因此如果选择在 Windows 系统上安装这个环境，`Docker` 就成了最佳实践方案。

首先，你需要先找到你想使用的 Docker 镜像，作者以 `jpillora/dnsmasq` 这个镜像为例。

## 下载 Docker 镜像

在 PowerShell 中运行命令 
```powershell
docker pull jpillora/dnsmasq
```

## 准备文件与路径

在一个你喜欢的地方（后续以 `<path/to/dnsmasq/home>` 代替），新建**文件夹** `conf.d` 和**文件** `dnsmasq.conf`。

## 配置 DNSMasq

在 `<path/to/dnsmasq/home>/dnsmasq.conf` 文件夹中进行配置：

```text
# dnsmasq config, for a complete example, see:
# http://oss.segetech.com/intra/srv/dnsmasq.conf
# 不加载本地的 /etc/hosts 文件
no-hosts

# 本地缓存时间
local-ttl=3600

# 配置公共 DNS 服务地址
# 格式
# server=<public dns ip>
# 需要几条就配置几条
server=8.8.8.8

# 开启日志选项
log-queries

# 异步log,缓解阻塞，提供性能
log-async=100

# 最大缓存条数
cache-size=1000000

# DNS转发最大值
dns-forward-max=1000000

# 需局解析的域名配置
# 格式：
# address=/<域名>/<对应的 IP 地址>
# 需要一条就加一条
address=/local.net/192.168.0.1
```

## 运行 DNSMasq

在 PowerShell 中运行如下指令：

```shell
docker run --name <name for dnsmasq> -d -p 53:53/udp -v <path/to/dnsmasq/home>\dnsmasq.conf:/etc/dnsmasq.conf -v <path/to/dnsmasq/home>\conf.d:/etc/dnsmasq/conf.d --log-opt "max-size=100m" -e "HTTP_USER=<Web username>" -e "HTTP_PASS=<Web password>" --restart always jpillora/dnsmasq
```

# 配置计算机/手机的 DNS 服务器

配置好手机和电脑的 DNS 服务器，重启路由器或正在开发的 App 即可。

## 电脑验证方式

```shell
nslookup <配置好的域名>

# Output:
# Server:  UnKnown
# Address:  127.0.0.1
#
# Name:    <上面的域名>
# Address:  <配置的 IP 地址>
```

## 手机验证方式

在电脑上打开 Docker Desktop 对应容器的日志。

在手机上打开浏览器，在地址栏输入你配置的域名并跳转，如果看到日志中有类似内容就是完成了配置

`query[<type>] <domain_name_you_entered> from <docker host ip>`