---
title: 添加私有 (p)npm 仓库
date: 2023-04-02 18:30:42
copyright: GitHub@xxxzl-JerryM
copyright_author: Github@xxxzl-JerryM
copyright_href: https://github.com/xxxzl-JerryM
tags: Frontend, Npm Registry, Private Npm Registry
---

# 环境定义

```
Windows 11 22H2 x64
Node.js 16.17.1
npm 8.15.0 | pnpm 7.29.0
```

# 文档定义

- 对于所有的 Shell 代码，均使用 `[expression]` 表示该表达式为可选变量。
- 对于所有的 Shell 代码，均使用 `<expression>` 表示该表达式为必填变量。

## 一、命令行修改
```shell
npm config set @<my_scoped>:registry <registry_url>
```
使用上述代码请注意：

如下图所示，可以看到获取属性值能获取到便是运行成功

![image (1)](https://dist.cq.vorbote.cn/picgo/image%20(1).png)


## 二、直接修改文件
使用命令行修改config配置其实就是在修改`~/.npmrc`文件，所以我们直接使用文本编辑器打开该文件
添加如下数据之后保存关闭即可

```
@my-scoped:registry=http://www.baidu.com
```
需要注意的是：

- `my-scoped`只是一个代称，请将其替换成真实的名称，例如`@my-npm:register`
- `http://www.baidu.com`只是一个代称，请将其替换成真实的仓库地址，例如`http://172.16.1.111:9999/npm/registry`
### 检测文件修改是否是否成功
如下图所示，可以看到获取属性值能获取到便是运行成功
![image (2)](https://dist.cq.vorbote.cn/picgo/image%20(2).png)

## pnpm配置
根据 `pnpm` 的官方文档：[配置|pnpm](https://pnpm.io/zh/configuring)  和 [pnpm config|pnpm](https://pnpm.io/zh/cli/config)
`pnpm` 首先使用 `npm` 的配置文件作为自己的配置来源，其次是 `pnpm`专门独属的配置文件
因此如果我们在 `npm` 的配置文件中配置了私有仓库
即使不做任何配置，`pnpm` 也可以直接进行私有仓库依赖的下载操作

## 运行一下
在下载本地私有仓库的依赖的时候，加上本地私有仓库所对应的`scoped`
例如：

```shell
npm install @my-npm/temp

pnpm add @my-npm/temp
```
示例：
![image (3)](https://dist.cq.vorbote.cn/picgo/image%20(3).png)
通过私有仓库`@jxlocal`实现下载`common-ui`到项目中

