# 学习知识点汇总

## 对象和数组遍历兼容

```js
function forEach(ele,fn) {
  if(typeof ele !== 'object'&&ele) {
    throw new TypeError('first value must be Array or Object')
  }
  Object.keys(ele)
    .map((item)=> {
      fn(ele[item],item,ele)
    })
}
let obj = {
  a:1,
  b:2
}
let arr = [2,3,4]
forEach(arr,(item,i,eles) => {
  console.log('item :', item);
  console.log('i :', i);
})
```
