# 模拟实现call

1. 判断当前this是否为函数，防止Function.prototype.myCall() 直接调用
2. context 为可选参数，如果不传的话默认上下文为 window
3. 为context 创建一个 Symbol（保证不会重名）属性，将当前函数赋值给这个属性
4. 处理参数，传入第一个参数后的其余参数
5. 调用函数后即删除该Symbol属性

```javaScript
Function.prototype.myCall = function (context = window, ...args) {
  if (this === Function.prototype) {
    return undefined; // 用于防止 Function.prototype.myCall() 直接调用
  }
  context = context || window;
  const fn = Symbol();
  context[fn] = this;
  const result = context[fn](...args);
  delete context[fn];
  return result;
} 
```

# 模拟实现apply

apply实现类似call，参数为数组

```javaScript
Function.prototype.myApply = function (context = window, args) {
  if (this === Function.prototype) {
    return undefined; // 用于防止 Function.prototype.myCall() 直接调用
  }
  const fn = Symbol();
  context[fn] = this;
  let result;
  if (Array.isArray(args)) {
    result = context[fn](...args);
  } else {
    result = context[fn]();
  }
  delete context[fn];
  return result;
}
```

# 模拟实现bind

1. 处理参数，返回一个闭包
2. 判断是否为构造函数调用，如果是则使用new调用当前函数
3. 如果不是，使用apply，将context和处理好的参数传入
```javaScript
Function.prototype.myBind = function (context,...args1) {
  if (this === Function.prototype) {
    throw new TypeError('Error')
  }
  const _this = this
  return function F(...args2) {
    // 判断是否用于构造函数
    if (this instanceof F) {
      return new _this(...args1, ...args2)
    }
    return _this.apply(context, args1.concat(args2))
  }
}
```
