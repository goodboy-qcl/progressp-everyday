# new 是什么

> `mdn` 上解释：`new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

## new 的作用

先通过两个例子来了解一下 `new` 的作用

```js
function Person (name, age) {
  this.name = name
  this.age = age
}

Person.prototype.sayHi = function () {
  console.log(`对${this.name}说say Hi`)
}

const zhangSan = new Person("张三", '18')

console.log(zhangSan.name) // 张三
zhangSan.sayHi() // 对张三说say Hi
```

从上面这个例子我们可以得出一些结论：

- `new` 通过构造函数 `Person` 创建出来的实例可以访问到构造函数中属性
- `new` 通过构造函数 `Person` 创建出来的实例可以访问到构造函数原型链中的属性，也就是说通过 `new` 操作符，实例与构造函数通过原型链连接了起来

再来看一个例子

```js

// 返回值为原始类型
function Animal(name) {
  this.name = name
  return name
}
const cat = new Animal('cat')
console.log(cat.name) // cat
console.log(cat) // Animal {name: 'cat'}

// 返回值为对象
function Animal(name) {
  this.name = name
  console.log(this) // Animal { name: 'dog' }
  return { age: 26 }
}
const dog = new Animal('dog')
console.log(dog) // { age: 26 }
console.log(dog.name) // 'undefined'
```

通过这个例子可以发现，如果返回值是原始类型的话，这个返回值就没有被返回；如果返回值为对象的的话，虽然内部的`this`还是在正常工作，但是对象的返回值是正常继续返回的。

所以我们可以得出以下结论： 

- 构造函数如果返回原始值，那么这个返回值毫无意义
- 构造函数如果返回值为对象，那么这个返回值会被正常使用

了解了 `new` 操作符是什么之后，我们看一下 `new` 操作符得执行过程

## new 做了什么

当我们用 `new` 操作符创建一个对象的话，它会进行如下操作：

1. 创建一个新对象 `obj`
2. 为新对象 `obj` 创建__proto__属性，并且将改属性链接至构造函数的原型对象
3. 将构建函数中的 `this` 绑定到新建的新对象 `obj` 上
4. 如果改构造函数返回值是对象，就返回该对象，否则返回 `this`

## 实现 new 操作符

根据 `new` 操作符得执行过程我们可以自己实现一下 `new`

```js
function create (fn, ...args) {
  // 创建对象
  let obj = {}
  // 将对象的原型与构造函数链接
  obj.__proto__ = fn.prototype  // or Object.setPrototypeOf(obj, fn.prototype) 
  //  执行函数并绑定this
  const res = fn.apply(obj, args)
  // 判断返回值
  return res instanceof Object ? res : obj
}
```

> 也可以使用 `Object.setPrototypeOf` 将两者联系起来。这段代码等同于 obj.__proto__ = obj.prototype