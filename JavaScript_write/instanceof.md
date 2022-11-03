# instanceof

## 什么是 instanceof

`instanceof` 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

```js
function Person (name, age) {
  this.name = name
  this.age = age
}

const zhangsan = new Person('张三','18')
 
console.log(zhangsan instanceof Person) // true

console.log(zhangsan instanceof Object) // true

```

语法就是： 

```js
object instanceof constructor
```

其中，`object` 表示某个实例对象，`constructor` 表示某个构造函数。

作用就是用来检测 `constructor.prototype` 是否存在于参数 object 的原型链上

## 需要注意

- 如果想要检测对象不是某个构造函数得实例时，你可能会这么写

```js
if(!obj instanceof Person) {

}
```

> 这么写的话，这段代码永远都是 `false`，因为再执行 `instanceof` 之前 `!obj` 就已经被处理，所以你总是在验证一个布尔值是不是 `Person` 的实例

你应该这么写

```js
if(!(obj instanceof Person)) {
  // XXXXXXX
}
```

借助括号来加强 `instanceof` 得运算优先级，先执行运算。

- `instanceof` 运算符不会永远返回一个值，要注意原型链是否给改变

借用mdn上的说法

> 需要注意的是，如果表达式 `obj instanceof Foo` 返回 `true`，则并不意味着该表达式会永远返回 `true`，因为 `Foo.prototype` 属性的值有可能会改变，改变之后的值很有可能不存在于 `obj` 的原型链上，这时原表达式的值就会成为 `false`。另外一种情况下，原表达式的值也会改变，就是改变对象 obj 的原型链的情况，虽然在目前的 ES 规范中，我们只能读取对象的原型而不能改变它，但借助于非标准的 `__proto__` 伪属性，是可以实现的。比如执行 `obj.__proto__ = {}` 之后，`obj instanceof Foo` 就会返回 `false` 了。

## 实现 instanceof

实现思路
1. 根据 `instanceof` 作用及语法，我们可以知道首先左边得是一个实例对象，右边得是一个构造器函数。
2. 接下去就是获取到类型的原型和对象的原型
3. 再判断对象的 `__proto__` 是不是能在 `prototype`上找到，找不到就顺着脸型连继续网上找，找到顶层还是没有返回 `false`，找到就返回 `true`

```js
function myinstanceof (left, right) {
    // 首先判断left是不是对象类型
    if (typeof left !== 'object' || left === null || typeof right !== 'function') {
      return false
    }
    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while (true) {
      // 如果找到null，代表到顶了返回 false
    	if (left === null) return false
      // 找到返回 true
    	if (prototype === left) return true
      // 一直顺着 原型链 往上找
    	left = left.__proto__
    }
}
```