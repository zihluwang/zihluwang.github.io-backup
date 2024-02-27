---
title: Mac 终端的主机名显示10，172或192
date: 2024-02-27 15:35:12
tags: [Mac, macOS]
---

按理来说，Mac 中的终端开头在默认情况下应显示为 yourname@computer-name，但是经常会变成 10、172 或 192 的情况。这是因为你没有设置电脑的主机名时，会自动使用 DNS 或 DHCP 的名称来作为电脑的主机名。解决这个问题有两种处理方法。

1. 使用公共 DNS 服务。在 Mac 的网络配置中使用公网中的 DNS 地址即可解决该问题。
2. 如果你是开发人员，需要自定义一些 DNS 解析策略，可以采用下面的命令来解决该问题：
    ```shell
    sudo scutil --set HostName $(scutil --get ComputerName)
    ```
    `$(scutil --get ComputerName)` 处也可以换成 `'任意你喜欢的名称'`。
