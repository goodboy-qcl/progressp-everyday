# Array Api实现

## forEach

```js
function forEach (arr, fn) {
  if (typeof fn !== 'function') return throw new Error(`${fn} 不是一个函数`)
  for (let i = 0; i < arr.length; i++) {
    fn.call(this, arr[i], i, arr)
  }
}

let arr = [1,2,3,4,5]
forEach(arr, (e, i, arr) => console.log(e, i, arr))
```