---
title: 在 Axios 中添加自定义请求头
date: 2023-03-23 22:12:51
tags: [vue, 前端, typescript, axios, upgrade]
---

今天开始写毕业设计的前端了，然后开始封装 Axios，代码如下：

```typescript
import axios, { AxiosRequestConfig } from "axios"
import { ElMessage } from "element-plus"
import { useDefaultStore } from "@/stores/DefaultStore"

const axiosInstance = axios.create({
  baseURL: "http://192.168.2.51:9080",
  timeout: 10_000
})

const defaultStore = useDefaultStore()

axiosInstance.interceptors.request.use((config: AxiosRequestConfig<unknown>) => {
  const token = defaultStore.clientToken
  config.headers = config.headers ?? {}
  config.headers["X-AUTH-KEY"] = token
  return config
}, (error) => {
  return Promise.reject(error)
})

axiosInstance.interceptors.response.use((response) => {
  const resp = response.data

  if (response.headers["x-auth-key"]) {
    console.log(response.headers["x-auth-key"])
    defaultStore.changeClientToken(response.headers["x-auth-key"])
  }

  if (resp.code >= 400 && resp.code < 600) {
    ElMessage({
      type: "error",
      message: resp.message
    })
  }
  return resp
}, (error) => {
  return Promise.reject(error)
})

export default axiosInstance
```

结果，就在写的时候，发现了个大问题，在以往最常用的设置请求头的方法（也就是代码第15行），提示了两个错误，截图如下：

![2023-03-23 22.22.39](https://dist.cq.vorbote.cn/image/jpeg/EbzbJm-1679581395.jpg)

大概翻译一下就是：

1. `config.headers` 这玩意儿可能为 `null` 或者 `undefined`，所以你记得处理它；
2. 设置数据的时候无法设置上了，直接报类型错误。

我就纳闷了，没记错的话好像之前也是这样写的哈？于是我就去翻了一下以前的代码，确实是这样写的没错，那么问题到底在哪里呢？仔细看了一下，因为这是属于 TS 的报错，于是就怀疑是不是 TS 升级导致的，把这个项目的 TS代码给降级了回去，结果完全没用。

再仔细检查，发现 axios 版本从0.x 升级到了 1.2.x，于是我当即就怀疑是 axios 升级做了重大修改导致，就开始在GitHub 上搜迁移指南（这玩意儿通常叫 MIGRATION_GUIDE 或 UPGRADE_GUIDE）。

好消息是，找到了这个文件：

![image-20230323222854339](https://dist.cq.vorbote.cn/image/png/xqwHsA-1679581734.png)

然后我高兴的打开一看：

![image-20230323223127299](https://dist.cq.vorbote.cn/image/png/pfCUCX-1679581887.png)

好家伙，就这么一点点？然后就在 Discussion 里面找到了这个：

![image-20230323223312290](https://dist.cq.vorbote.cn/image/png/c6lFVf-1679581992.png)

是的你们没看错，作者**已经把文件夹建好了**。

没办法，只能降级到 0.x 先用着了，等迁移公告出来再考虑升级 1.x 吧～

BTW，如果有人知道在 1.x 版本里面添加自定义请求头（我拒绝使用 `// @ts-ignore` 方式解决报错！），请务必告诉我，谢谢！
