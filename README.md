<p align="center">
    <img alt="logo" src="https://www.anyup.cn/static/anyup/images/logo.png" width="120" style="margin-bottom: 10px;">
</p>

<h1 align="center">uni-http</h1>

<p align="center">🔥 一个基于 promise 的，轻量且强大的 http 网络库</p>

<p align="center">
  🔥 <a href="https://www.anyup.cn/site/zh/uni-http/guide/introduce.html">文档网站</a>
</p>

## 一. 简介

🚀 基于 `Promise` 的 `uni.request` ，轻量且强大的 `http` 网络库，特性：

1. 提供统一的 `Promise API`
2. 基于 `uni.request`，支持多种运行环境，浏览器 H5、小程序、APP 等
3. 支持发起 `GET`、`POST`、`PUT`、`DELETE`、`upload` 请求
4. 支持请求／响应拦截器
5. 支持请求的 `header`
6. 支持处理请求的 `loading` 状态
7. 支持对请求结果进行统一处理
8. 支持链式调用
9. 支持批量生成 API 请求，简化代码量 99.9%，一行代码搞定繁琐的请求 function

## 二. 安装

```bash
// 安装
npm install @anyup/uni-http -S

// 更新
npm update @anyup/uni-http
```

## 三. 快速上手

```javascript
import { Http, HttpHeader } from '@anyup/uni-http'

const http = new Http()
// 仅为示例 API 域名，使用时替换自己的接口域名即可
const baseURL = 'https://demo.api.com'
// 设置header
const header = { 'Content-Type': 'application/json;charset=UTF-8' }

// 设置 baseURL 和 header，支持链式调用
http.setBaseURL(baseURL).setHeader(header)

// 设置请求拦截器
http.interceptors.request.use(
  request => {
    if (request.loading) {
      // 如果配置了loading，请求开始需要显示
    }
    // 设置请求header
    request.header['Authorization'] = ''
    return request
  },
  error => Promise.resolve(error)
)
// 设置响应拦截器
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
      // 如果配置了loading，请求完成需要关闭
    }
    // 其他业务处理
    console.log('complete', complete)
  }
)

// 登录请求示例，并配置请求时显示loading
function requestLogin(username, password) {
  http
    .request(
      '/api/login',
      { username, password },
      { method: 'POST', loading: true }
    )
    .then(res => {
      // 处理 response 响应
      if (res.status === 1) {
        console.log(res)
      }
    })
}

// 直接使用 get|post|put|delete 方式请求
function requestLogin1(username, password) {
  // 也可以直接使用 post 方法
  http
    .post('/api/login', { username, password }, { loading: true })
    .then(res => {
      // 处理 response 响应
      if (res.status === 1) {
        console.log(res)
      }
    })
}

// es6 await 风格 登录请求示例
async function requestLogin2(username, password) {
  const res = await http.request(
    '/api/login',
    { username, password },
    { method: 'POST', loading: true }
  )
  // 处理 response 响应
  if (res.status === 1) {
    console.log(res)
  }
}
```

## 四. 进阶使用

> 工厂模式按配置表批量生成 API 方法库，体验飞一般的感觉

### 1. 初始化 Http 类

```javascript
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

### 2. 定义接口配置

```javascript
// api.js
import http from './http'

const urls = {
  // 用户
  login: { url: '/api/login', method: 'POST', loading: true } // 用户登录
}

export default http.dispatch(urls)
```

### 3. 在代码中使用

```javascript
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

## 五. 贡献代码

使用过程中发现任何问题都可以提 [Issue](https://gitee.com/anyup/uni-http/issues) 给我，当然，我们也非常欢迎你给我们发 [PR](https://gitee.com/anyup/uni-http/pulls)。

## 六. 浏览器支持

支持现代浏览器以及 Android >= 4.0、iOS >= 8.0。

## 七. 开源协议

本项目基于 MIT 协议，请自由地享受和参与开源。

## 八. 联系我

如果使用过程中有任何问题，可以联系我，如下方式：

- 微信号：anyupxing
- 微信公众号：前端梦工厂
- 掘金专栏：[uni-app 前端开发](https://juejin.cn/column/7070045934851194911)
