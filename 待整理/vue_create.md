面对着日益变化的大前端时代。每天感觉都会有很多变化。在这不断变化的世界中。我们该如何自处呢？有位名人说过一句话'世界上唯有变是不变的'看起开很绕。但简单的来想。也是能解释的通的。废话少说。开始正题。

[Vue官方文档](https://cn.vuejs.org/v2/guide/installation.html)

### 一. vue-cli 官方脚手架

>官方脚手架还是非常贴心的。用起来也非常方便。
```
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev

```
>但是还是有些不足。需要自己动手来配置下。

#### 1. 比如所你使用less或sass等css预处理语言需要自己下载并配置：

> - 如果你使用的是less
```
npm install --save-dev less-loader
npm install --save-dev less
```
在build/webpack.base.conf.js中进行配置下：

```
...
module: {
    rules: {
        ...
      {
        test: /\.less$/,
        loaders: ["style", "css", "less"]
      },
        ...
    }
}

```

> 如果你使用scss
```
npm install --save-dev node-sass
npm install --save-dev sass-loader
```
在build/webpack.base.conf.js中进行配置下：

```
...
module: {
    rules: {
        ...
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },
        ...
    }
}
```
> 现在就可以开心的使用less/sass了
```
<style lang='less/scss' scoped>
    ...
</style>
```
#### 2. 再就是我们往往会涉及到跨域问题，
> 请求接口时候出现如下错误信息：
```
No ‘Access-Control-Allow-Origin’ header is present on the requested resource.
```
> 解决方法有好多种。我们来说下webpack-dev-server的代理：

> 1. 首先在config目录下新建一个proxyConfig.js文件
```
module.exports = {
  proxy: {
        '/apis': {    //将www.exaple.com印射为/apis
            target: 'http://dsp.xiaojukeji.com',  // 接口域名
            changeOrigin: true,  //是否跨域
            pathRewrite: {
                '^/apis': ''   //需要rewrite的,
            }              
        },

  }
}
```
> 2. 在config/index.js中

```
const proxyConfig = require('./proxyConfig.js')
...
    dev: {
        ...
        proxyTable: proxyConfig.proxy
        ...
    }
...
```
> 然后就ok了。

下面我们就可以开开心心的开发了。
