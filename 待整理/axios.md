我们需要 http 请求，所以选择 axios。

> Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

[中文文档](https://www.kancloud.cn/yunye/axios/234845)
[官方文档](https://github.com/axios/axios)

#### 1.axios 的基础用法

- 安装

```
npm install axios -S
```

- 创建自定义配置新建一个 axios 实例
  > 新建个 service.js 文件

```
import axios from "axios"
//判断是开发环境还是上线环境
let baseUrl ;
if (process.env.NODE_ENV == 'development') {
    baseUrl = '/apis/';//代理地址
}else{
    baseUrl = 'http://10.93.24.18:8080/';//上线地址
}
//自定义axios实例名为service
const service = axios.create({
    baseURL: baseUrl, // api的base_url
})
export {baseUrl}
export default service
```

> 有时候有个这样的需求就是每个请求都要带上 token 参数，或者请求头的配置。这个时候可以用到 axios 拦截器

```
// 添加请求拦截器
service.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    //每个请求添加token
    //登录拦截
    console.log(config)
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});

// 添加响应拦截器
service.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
});
```

- get 请求

```
import service from "./service.js"

export const getXxx(xx1,xx2) {
   return service({
        url: "/xxx/xxx",
        method: "get",
        params:{
            param1:xx1,
            param2:xx2,
        }
    })
}

```

- post 请求

```
import service from "./service.js"

export const postXxx(xx1,xx2) {
   return service({
        url: "/xxx/xxx",
        method: "post",
        data:{
            param1:xx1,
            param2:xx2,
        }
    })
}

```

- 通过 es7 的 async/await 来进行异步解决方案

```
import {getXxx} from "./xxx"
async function login(username,passward) {
    let data = await getXxx(username,passward);
    let info = data.data.info ? JSON.parse(data.data.info) : [];
    //可以使用info了。
    ...
}
```

> [了解更多请参考](https://segmentfault.com/a/1190000007535316)
