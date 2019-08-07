> 本文只介绍那些不常用的到，但是还是用起来还很舒服的东西。

##### 1. v-once
> 用途: 用于渲染一次用的。（可以用于优化更新性能）

```
<span v-once>这个将不会改变: {{ msg }}</span>

new Vue({
    data:{
        msg:"哈哈"
    }
})

//结果
<span>这个将不会改变:哈哈</span>//只渲染一次。msg改变也不会在渲染了。

```
##### 2. v-pre
> 用途: 用于跳过渲染直接展示{{XXX}}

```
<span v-pre>{{ this will not be compiled }}</span>

//结果
<span>{{ this will not be compiled }}</span>
```
##### 3. v-if v-else v-else-if
> 条件语句，满足条件在dom中渲染或销毁元素/组件

> 在同级下多个元素/组件同时判断条件。可以包裹\<template v-if="XXX"\>进行统一判断。

> Vue在渲染元素的时候，出于效率考虑，会尽可能的复用已有的元素而并非重新渲染。在切换的时候内部元素会被复用，有的时候并不符合我们的预期。可以加入**key值**进行独立开来

```
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>

```

这样 加上唯一key的input元素不会被复用。而label元素会被复用。

==v-if与v-for一起使用的时候，v-for的优先级比v-if高，可用\<template\>来隔离==
##### 4. v-show

1. 只是css属性的display的改变
2. v-show不能在\<template\>上使用，也不支持 v-else。

> v-if 适合于条件不经常变化的场景。而v-show适合于频繁切换条件。

##### 5. v-for
> 使用的时候记得提供key

> v-for可以遍历数组，对象，也可以迭代整数

- 数组更新检查
> Vue的核心是数据与视图双向绑定，当我们修改数组时，Vue会检测到数据变化，所以用v-for渲染的视图也会立即更新。

###### 1. 变异方法，可以触发视图更新
```
例如：push()、pop()、unshift()、 shift() 、splice() 、sort() 、reverse()
```

###### 2. 非变异方法，无法触发视图更新
```
例如：filter()、concat()、slice()等不会改变原数组而是返回个新数组。

解决：覆盖变量。app.a = app.a.filter((item)=>{return item > 10})

```
###### 3. 通过索引直接设置项，不会触发视图更新。


```
例如：app.book[3]={...}

解决：（1）通过vue.set(vm.items,indexofItem,newVal)或$set()方法。
      （2）通过vm.items.splice(indexofItem,1,newVal)
```


###### 4. 修改数组的长度
```
例如：app.book.length = 1

解决：vm.items.splice(newLength)
```
- vue不能检测对象属性的添加或删除
###### 1. 对于单个属性的添加

```
例如：obj.name = "wang"

解决：Vue.$set(obj,name,"wang")
```
###### 2. 对于多个属性添加

```
不能：Object.assign(vm.obj,{a:1,b:2})

应该：vm.obj = Object.assign({},vm.obj,{a:1,b:2})
```
- 过滤与排序
> 当你不想改变原数组，想通过一个数组的副本来做过滤或者排序的显示时，可以用**计算属性**来返回过滤或者排序后的数组。

##### 方法与事件
> 如果handleClick方法没有参数，$event 自动传入。

```
<div @click="handleClick">点击</div>

methods: {
    handleClick(e) {
        console.log(e.target.innerHtml)//点击
    }
}
```
> 如果handleClick方法有参数，手动传入$event。

```
<div @click="handleClick(aa,bb,$event)">点击</div>

methods: {
    handleClick(aa,bb,e) {
        console.log(e.target.innerHtml)//点击
    }
}
```
- 修饰符
1. .stop   ---阻止单击事件冒泡
2. .prevent  ---阻止默认事件
3. .capture  ---使用事件捕获模式
4. .self  --- 只当事件在该元素本身(而不是子元素)触发时触发回调
5. .once  --- 只触发一次。（组件同样适用）
6. .passive  --- 不等回调立即触发（scroll,用于提升移动端性能）

还有些按键的修饰符。[自行参看文档](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)
