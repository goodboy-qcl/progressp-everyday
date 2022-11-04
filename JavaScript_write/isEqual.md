# 判断两个对象是否相等

都知道 `js` 比对对象的时候是通过引用地址去比对得，那么如果引用地址不同，那他就不相等。

看个例子

```js
const a = {a: 1}
const b = a
const c = {a: 1}

b === a // true
c === a // false
```

那如果我们想要判断两个对象，如果拥有相同的 `key` 和 `value`，那我们就认为他们相同。要怎么实现呢

## 实现

实现之前我们先来撸一下思路：

1. 首先我们需要判断对比的对象是否是对象，不是则直接比较

2. 再判断传入得对象是否是同一个，如果是，则不用判断直接返回 `true`

3. 然后判断可以判断两个对象的key值得个数是否相等，不相等则直接返回 `false`

4. 最后再以其中一个对象为基准，与另一个对象递归比较。比较他们当前的 `key` 得 `val` 是否相同

开始手写：

```js

// 判断是否是对象 工具函数
function isObject (obj) {
  return typeof obj === "object" && obj !== null
}

function isEqual (obj1, obj2) {
  // 1. 两个对象是否是 object
  if (!isObject(obj1) || !isObject(obj2)) {
    // 注意NaN
    if(Number.isNaN(obj1) && Number.isNaN(obj2)) {
      return NaN !== NaN // return true
    }
    return obj1 === obj2
  }
  // 2. 对象是否是同一个
  if (obj1 === obj2) return true
  
  // 3. 判断两个对象的key值得个数
  const obj1Keys = Object.keys(obj1)
  const obj2Keys = Object.keys(obj2)
  if (obj1Keys.length !== obj2Keys.length) return false

  // 4. 以 obj1 为基准，和 obj2 一起递归比较
  for (let key in obj1) {
    const res = isEqual(obj1[key], obj2[key])
    if (!res) return false
  }

  return true
}
```