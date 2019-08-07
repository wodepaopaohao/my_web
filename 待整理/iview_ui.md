> iView 是一套基于 Vue.js 的开源 UI 组件库，主要服务于 PC 界面的中后台产品。
- 安装
```
npm install iview --save
```
- 一般在 webpack 入口页面 main.js 中如下配置：

```
import Vue from 'vue';
import iView from 'iview';
import 'iview/dist/styles/iview.css';

Vue.use(iView);
```
- 按需引用
> 借助插件 babel-plugin-import可以实现按需加载组件，减少文件体积。首先安装，并在文件 .babelrc 中配置：

```
npm install babel-plugin-import --save-dev

```

```
// .babelrc
{
  "plugins": [["import", {
    "libraryName": "iview",
    "libraryDirectory": "src/components"
  }]]
}
```
> 然后这样按需引入组件，就可以减小体积了：

```
import { Button, Table } from 'iview';
Vue.component('Button', Button);
Vue.component('Table', Table);
```
> 按需引用仍然需要导入样式，即在 main.js 或根组件执行 import 'iview/dist/styles/iview.css';
- 定制主题
> 如果你的项目使用了 webpack 工程，可以通过变量覆盖的方式来实现主题定制。

> 首先在项目中先建一个目录，比如 my-theme，然后在 my-theme 下建立一个 less 文件 index.less，并写入下面内容：

```
@import '~iview/src/styles/index.less';

@primary-color: #8c0776;//主题色
```
完整的变量列表可以查看[默认样式变量](https://github.com/iview/iview/blob/2.0/src/styles/custom.less)
> 然后在入口文件 main.js 内导入这个 less 文件即可：

```
import Vue from 'vue';
import iView from 'iview';
import '../my-theme/index.less';

Vue.use(iView);
```
