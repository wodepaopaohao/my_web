在大型项目中我们肯定会用到spa单页应用的。要做单页应用我们需要前端路由。
>正常用vue-cli创建项目时候会提醒是否安装vue-router

所以基础配置就不再赘述了。
[官方文档](https://router.vuejs.org/zh-cn/installation.html)

#### 1. 多人开发的时候同时修改router/index.js 配置文件导致的冲突的解决方法

在index.js中
```
import Vue from 'vue'
import Router from 'vue-router'
import NAV1 from './NAV1.js'
import NAV2 from './NAV2.js'

Vue.use(Router)

const router = new Router({
  routes: [
    NAV1,
    NAV2,
  ]
})
```

在NAV1.js中
```
const DataPanel = () => import('../views/DataPanel/DataPanel.vue')
const DriverShow = () => import('../views/DataPanel/driverShow/driverShow.vue')
export default {
    path: '/datapanel',
    component: DataPanel,
    meta: { requireAuth: true },
    children: [
        { path: '', component: DriverShow, meta: { requireAuth: true } },
    ]
}

```
> 这样就把每个人负责的模块单独配置自己的路由。在导入到index.js中。

也许你会发现 引入模块的方式和你看看的方式不一样
```
import Md1 from './md1.vue' // 你看到的方式

const Md1 = () => import('./md1.vue') //上面写的方式
```
当打包构建应用时，Javascript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

>结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。


>注意：如果您使用的是 Babel，你将需要添加 syntax-dynamic-import 插件，才能使 Babel 可以正确地解析语法。
```
npm install --save-dev babel-plugin-syntax-dynamic-import
```
在根目录下有个.babelrc文件（babel的配置文件）
```
{
  "plugins": [...,"syntax-dynamic-import"]
}
```
这样就可以像上面那样引入文件了。

#### 2. 登录路由拦截功能（就是说再跳路由前判断。是否有权跳路由）

> 1. 首先在配置路由时候添加一个字段
```
const routes = [
    {
        path: '/',
        name: '/',
        component: Index
    },
    {
        path: '/repository',
        name: 'repository',
        meta: {
            requireAuth: true,  // 添加该字段，表示进入这个路由是需要登录的
        },
        component: Repository
    },
    {
        path: '/login',
        name: 'login',
        component: Login
    }
];
```
> 2. 定义完路由后，我们主要是利用vue-router提供的钩子函数beforeEach()对路由进行判断。

```
router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth) {  // 判断该路由是否需要登录权限
        if (store.state.token) {  // 通过vuex state获取当前的token是否存在
            next();
        }else {
            next({
                path: '/login',
                query: {redirect: to.fullPath}  // 将跳转的路由path作为参数，登录成功后跳转到该路由
            })
        }
    }else {
        next();
    }
})
```
这样就可以了，[具体细节参考](https://github.com/superman66/vue-axios-github/blob/master/src/router.js)

> 还有一种axios拦截器，将在后面介绍。
