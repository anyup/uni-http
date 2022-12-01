<p align="center">
    <img alt="logo" src="https://www.anyup.cn/static/anyup/images/logo.png" width="120" style="margin-bottom: 10px;">
</p>

<h1 align="center">anyup design</h1>

<p align="center">一个基于 promise 的，轻量且强大的 http 网络库</p>

<p align="center">
  🔥 <a href="https://www.anyup.cn/docs/zh/js/http.html">文档网站</a>
</p>

## 简介

🚀 基于 `promise` 的 `uni.request` ，轻量且强大的 `http` 网络库：

1. 提供统一的 `Promise API`。
2. 浏览器环境下，轻量且非常轻量 。
3. 基于`uni.request`，支持多种运行环境。
4. 支持请求／响应拦截器。
5. 自动转换 `JSON` 数据。
6. 支持批量生成API请求，简化代码量99%，一行代码搞定

## 安装

```bash
// 安装
npm install @anyup/uni-http -S

// 更新
npm update @anyup/uni-http
```

## 一、快速上手

```js
import { Http } from '@anyup/uni-http'

const http = new Http().setBaseURL('').setHeader({ 'Content-Type': 'application/json;charset=UTF-8' })

// 请求示例
requestLogin(username, password) {
	http.request('/login', { username, password }).then(res => {

	})
}

// es6 await风格
async requestLogin(username, password) {
	await http.request('/login', { username, password })
}

// 上传文件示例，上传文件封装的uni.uploadFile
async uploadFile() {
	await http.upload('/upload', { filePath: 'file', , name: 'fileName', formData: {} } )
}
```

## 二、进阶，按需批量生成API，体验飞一般的感觉

### 1.初始化 Http 类

```js
// http.js
import { Http } from '@anyup/uni-http'

const header = {}
const baseURL = ''
const http = new Http().setBaseURL(baseURL).setHeader(header)

// 请求拦截器
http.interceptors.request.use(
  request => {
    if (request.loading) {
      // 如果配置了loading，显示
    }
    // 设置请求header
    request.header['Authorization'] = ''
    return request
  },
  error => Promise.resolve(error)
)
// 响应拦截器
http.interceptors.response.use(
  response => {
    // 请求成功
    if (!response.data) {
      return Promise.reject(new Error('接口请求未知错误'))
    }
    // 其他业务处理
    return Promise.resolve(response)
  },
  error => {
    // 请求失败，业务处理
    return Promise.reject(error)
  },
  complete => {
    // 请求完成
    if (complete.request.loading) {
      // 如果配置了loading，关闭
    }
    // 其他业务处理
    console.log('complete', complete)
  }
)

export default new Http.Builder(http)
```

### 2.定义接口配置

```js
// api.js
import http from './http'

const urls = {
  // 用户
  login: { url: '/api/login', method: 'POST', loading: true } // 用户登录
}

export default http.dispatch(urls)
```

### 3.使用方式

```vue
// demo.vue
<script>
import api from './api'
export default {
  methods: {
    login() {
      api.login().then(res => {
        // 请求成功
      })
    }
  }
}
```

## 贡献代码

使用过程中发现任何问题都可以提 [Issue](https://gitee.com/anyup/uni-http/issues) 给我们，当然，我们也非常欢迎你给我们发 [PR](https://gitee.com/anyup/uni-http/pulls)。

## 浏览器支持

支持现代浏览器以及 Android >= 4.0、iOS >= 8.0。

## 开源协议

本项目基于 MIT 协议，请自由地享受和参与开源。
