# 实现flat

## 基本实现

1. 第一种
```js
function flat (arr) {
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flat(arr[i]));
    } else {
      result.push(arr[i])
    }
  }
  return result
}
```

2. 第二种
```js
function flat (arr) {
  let isArray
  let i = 0
  while(i<arr.length) {
    isArray = arr[i] instanceof Array
    if(isArray) break
    i++
  }
  if(!isArray) return arr
  const res = Array.prototype.concat.apply([],arr)
  return flat(res)
}
```

3. 第三种
```js
function flat (arr) {
  const isArray = arr.some(item => item instanceof Array)
  // 如果没有直接返回
  if(!isArray) return arr
  const res = Array.prototype.concat.apply([],arr)
  // 递归继续摊平
  return flat(res)
}
```

4. 第四种
```js
function flat (arr) {
  return eval("[" + `${arr}`.replace(/\[|\]/g,'') + "]")
}
```

## 深度控制

1. 
```js
function flat (arr, depth = 1) {
  const result = []; // 缓存递归结果
  (function flat(arr, depth) {
    arr.forEach((item) => {
      // 控制递归深度
      if (Array.isArray(item) && depth > 0) {
        flat(item, depth - 1)
      } else {
        result.push(item)
      }
    })
  })(arr, depth)
  return result;
}
```
2. 
```js
function flat (arr, depth = 1) {
    if (depth <= 0) return arr;
    let res = []
    res = arr.reduce((res, cur) => res.concat(Array.isArray(cur) ? flat(cur, depth - 1) : cur), [])
    return res
}
```