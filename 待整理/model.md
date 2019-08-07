#### v-model(重点)
> 用途: 用 v-model 指令在表单 \<input\> 及 \<textarea\> 元素上创建双向数据绑定(本质是一种语法糖)

```
//正常版
<input type="text" :value='value1' @input="value1 = $event.target.value" />
//语法糖版
<input type="text" v-model="value2"/>

//在组建中也是如此啊。最后用this.$emit("XXX",XX)就可以暴露出来。在进行绑定就可以了。

```
> v-model 会**忽略**所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。

> 对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 **input 事件**。
##### 单选按钮

> 单选按钮在单独使用时，不需要v-model，直接使用v-bind绑定一个布尔类型的值，为真时候选中，为否时候不选

```
<input type="radio" :checked="aaa"/>
```
> 如果组合使用来实现互斥选择的效果，就需要v-model配合value来使用。

```
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>

  new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```
picked是个数组当被选中时候。会把被选中的input对应的value 放到picked中。

##### 复选框

> 单独使用的时候

```
<input type="checkbox" :checked="aaa"/>
//或
<input type="checkbox" v-model="aaa"/>
```
> 组合使用的时候。

```
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>

new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```
被选中的会到checkedNames数组中。

##### 值绑定

 > 有时我们可能想把值绑定到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，并且这个属性的值可以不是字符串。

###### 单选框

```
<input type="radio" v-model="pick" v-bind:value="a">

// 当选中时
vm.pick === vm.a
```

###### 复选框

```
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>

// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```
> 这里的 true-value 和 false-value 特性并不会影响输入控件的 value 特性，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(比如“yes”或“no”)，请换用单选按钮。

##### 修饰符
 > 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步：
1. .lazy
> 自动将用户的输入值转为数值类型
2. .number
> 自动过滤用户输入的首尾空白字符
3. .trim

#### v-model 在组件中的使用

> 在组件中使用时，它相当于下面的简写：

```
<custom-input
  v-bind:value="something"
  v-on:input="something = arguments[0]">
</custom-input>
```
