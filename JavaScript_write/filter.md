# Array.prototype.filter()

> `filter() ` 方法创建给定数组一部分的浅拷贝，其包含通过所提供函数实现的测试的所有元素。

为数组中的每个元素调用一次 `callbackFn` 函数，并利用所有使得 `callbackFn` 返回 `true` 或等价于 `true` 的值的元素创建一个新数组。

# 语法

```js
// 箭头函数
filter((element) => { /* … */ } )
filter((element, index) => { /* … */ } )
filter((element, index, array) => { /* … */ } )

// 回调函数
filter(callbackFn)
filter(callbackFn, thisArg)

// 内联回调函数
filter(function(element) { /* … */ })
filter(function(element, index) { /* … */ })
filter(function(element, index, array){ /* … */ })
filter(function(element, index, array) { /* … */ }, thisArg)
```

## 参数

`callbackFn`

  用来测试数组中每个元素的函数。返回 true 表示该元素通过测试，保留该元素，false 则不保留。它接受以下三个参数：

  `element`
  数组中当前正在处理的元素。

  `index`
  正在处理的元素在数组中的索引。

  `array`
  调用了 filter() 的数组本身。

`thisArg` 可选

  执行 `callbackFn` 时，用于 `this` 的值。

## 返回值

一个新的、由通过测试的元素组成的数组，如果没有任何数组元素通过测试，则返回空数组。

# 示例

在数组中搜索
下例使用 filter() 根据搜索条件来过滤数组内容。

```js
const fruits = ['apple', 'banana', 'grapes', 'mango', 'orange'];

/**
 * 根据搜索条件（查询）筛选数组项
 */
function filterItems(arr, query) {
    return arr.filter((el) => el.toLowerCase().includes(query.toLowerCase()));
}

console.log(filterItems(fruits, 'ap')); // ['apple', 'grapes']
console.log(filterItems(fruits, 'an')); // ['banana', 'mango', 'orange']
```

# 实现

```js
Array.prototype.myfilter = function(callback, thisArg) {
  // 调用的数组是否存在
  if (this == undefined) {
    throw new TypeError('this is null or not undefined');
  }
  // 判断callback是否为函数
  if (typeof callback !== 'function') {
    throw new TypeError(callback + 'is not a function');
  }
  const res = [];
  // 让O成为回调函数的对象传递（强制转换对象）
  const O = Object(this);
  // >>>0 保证len为number，且为正整数
  const len = O.length >>> 0;
  for (let i = 0; i < len; i++) {
    // 检查i是否在O的属性（会检查原型链）
    if (i in O) {
      // 回调函数调用传参
      if (callback.call(thisArg, O[i], i, O)) {
        res.push(O[i]);
      }
    }
  }
  return res;
}
```