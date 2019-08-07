#### 计算属性
> 如果在模板中使用太过复杂的表达式，这时候我们可以使用计算属性。

```
<div :style='{background:type===1?"red":type===2?"green":"black",height:"20px"}' ></div>

//用计算属性

<div :style='{background:type1,height:"20px"}' ></div>


computed: {
	type1() {
		if(this.type === 1) {
			return "red"
		}else if(this.type === 2) {
			return "green"
		}else {
			return "black"
		}
	}
}

```
> 有的时候是通过通过改变的数据不是data中的数据，而是v-for出来的我们可以用methods
```
<div :style='{background:handleType(type),height:"20px"}' ></div>

methods: {
	handleType(type) {
		if(type === 1) {
			return "red"
		}else if(type === 2) {
			return "green"
		}else {
			return "black"
		}
	},
}
```
> 计算属性可以进行get ,当然也可以进行set


```
data() {
    return {
        a:1
    }
}
computed:{
    haha:{
        get() {
            return this.a +1
        },
        set(value) {
            this.a = value - 1;
        }
    }
}

this.a     // 1
this.haha  // 2
this.haha = 3
this.a     // 2
```
> 绝大多数情况下我们只需要getter方法。很少用到setter


##### 计算属性的使用场景：
1. 简单的文本插入
2. 动态设置样式名称class和内联样式style
3. 当使用组件时候，计算属性也经常用来动态传递props
4. 计算属性可以依赖其他的计算属性
5. 计算属性不仅可以依赖当前Vue实例的数据，还可以依赖其他实例数据。


```
//对应场景5进行简单的demo
var vm1 = new Vue({
    el:"#app1",
    data:{
        text:111,
    }
})
var vm2 = new Vue({
    el:"#app2",
    computed:{
        addOne() {
            return vm1.text + 1;
        }
    }
})
//后续补充为什么这样用。
```
> 计算属性（computed）相对于方法（methods）来说好处在于依赖缓存。也就是说，只要data中的数据不发生变化。计算属性不重新计算。而方法每次都要计算。


#### 侦听器（watcher）

> 正常书写：

```
watch:{
    a:function(newVal,oldVal) {
        /*...*/
    }
}

```
> 用个方法名

```
watch:{
    a:"method1"
}
```
> 深度监听：
- 为了发现对象内部值的变化，可以在选项参数中指定 deep: true 。注意监听数组的变动不需要这么做。

```
watch:{
    a:{
        handler:function(newVal,oldVal) {
            /*....*/
        },
        deep:true
    }
}
```
>  该回调将会在侦听开始之后被立即调用(初始化数据的时候也调用)：

```
watch: {
    a:{
        handler: function(newVal, oldVal) {
            /* ... */
        },
        immediate: true
    }
}
```
> 执行一系列监听函数

```
watch: {
    a:[
        function handle1(val, oldVal) {
            /* ... */
        },
        function handle2(val, oldVal) {
            /* ... */
        }
    ],
}
```
> 监听对象下的某个属性：

```
watch: {
    'e.f': function(val, oldVal) {
        /* ... */
    }
}
```

> 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时**执行异步**或**开销较大的操作**时，这个方式是最有用的。

#### filter 过滤器
> 用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式 (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：

```
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>

// 你可以在一个组件的选项中定义本地的过滤器：
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
//或者在创建 Vue 实例之前全局定义过滤器：
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```
