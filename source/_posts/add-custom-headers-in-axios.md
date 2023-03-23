---
title: 在 Axios 中添加自定义请求头
date: 2023-03-23 22:12:51
tags: vue, 前端, typescript, axios, upgrade
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
