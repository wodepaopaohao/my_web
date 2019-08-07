#### class
#####  1. 对象语法：

```
<div v-bind:class="{ "active": isActive }"></div>
```
> active 的类名是否添加，取决于isActive的值是否为真

- 绑定的数据对象不必内联定义在模板里：

```
<div v-bind:class="classObject"></div>

data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}

```
- 我们也可以在这里绑定一个返回对象的**计算属性**。这是一个常用且强大的模式：

```
<div v-bind:class="classObject"></div>

data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
#####  2. 数组语法：

```
<div v-bind:class="[activeClass, errorClass, 'aa']"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
//结果：
<div class="active text-danger aa"></div>
```
- 带表达式的

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
- 当有多个条件 class 时上面这样写有些繁琐。所以在数组语法中也可以使用对象语法：

```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>

```

##### 3. 在组件上使用：
> 一个自定义组件上使用 class 属性时，这些类将被添加到该组件的**根元素**上面。这个元素上已经存在的类不会被覆盖。


#### style

##### 1. 对象语法：

```
//用驼峰式 (camelCase) 或短横线分隔 (kebab-case)
<div v-bind:style="{ marginLeft: marginLeft, 'font-size': fontSize + 'px' }"></div>

data: {
  marginLeft: 0,
  fontSize: 30
}
//直接绑定到一个样式对象通常更好，这会让模板更清晰：
<div v-bind:style="styleObject"></div>

data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}

```
> 计算属性同样适用。
##### 2. 数组语法：
> 和class 类似  

> 自动添加前缀：当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

##### 3. 多重值
> 从 2.3.0 起你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
> 这样写只会渲染数组中最后一个被浏览器支持的值。在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。
