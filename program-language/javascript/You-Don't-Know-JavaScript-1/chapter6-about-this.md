## 第6章 关于this

### 1. 为什么要用this？

``` javascript
function identify() {
  return this.name.toUpperCase()
}

const me = {
  name: 'Demon'
}

identify.call(me) // DEMON
```

如果不使用this，就需要显式的传入一个上下文对象，这样会让代码变得越来越混乱，使用this则不会。

``` javascript
function identify(object) {
  return object.name.toUpperCase()
}

identify(me) // DEMON
```

### 2. 误解

#### 2.1 指向自身

this并不是指向函数自身的。

``` javascript
function foo(num) {
  console.log(`foo: ${num}`)
  // 记录函数被调用的次数
  this.count++ 
}

foo.count = 0

for (var i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i)
  }
}

console.log(foo.count) // 0
```

这可能会让人困惑，实际上foo函数中的this指向并不是函数对象，而是全局的。这段代码会在全局中创建一个count变量，它的值为NaN。

可能很多人会通过`词法作用域`的方式解决这个问题。

``` javascript
function foo(num) {
  console.log(`foo: ${num}`)
  // 记录函数被调用的次数
  data.count++ 
}

const data = {
  count: 0
}

for (var i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i)
  }
}

console.log(foo.count) // 4
```

对于这个例子，我们可以使用foo标识符代替this来引用函数对象

``` javascript
function foo(num) {
  console.log(`foo: ${num}`)
  // 记录函数被调用的次数
  foo.count++ 
}

foo.count = 0

for (var i = 0; i < 10; i++) {
  if (i > 5) {
    foo(i)
  }
}

console.log(foo.count) // 4
```

但是这种方法回避了this的问题，依赖于foo的此法作用域，我们通过改变this指向来解决这个问题。

``` javascript
function foo(num) {
  console.log(`foo: ${num}`)
  // 记录函数被调用的次数
  this.count++ 
}

foo.count = 0

for (var i = 0; i < 10; i++) {
  if (i > 5) {
    // 使用call确保this执行函数本身
    foo.call(foo, i)
  }
}

console.log(foo.count) // 0
```

### 3. this到底是什么？

this的上下文取决于函数调用时的各种条件，this的绑定和函数的声明没有任何关系，只取决于函数的调用方式。

当函数被调用时，会创建执行上下文，执行上下文会包含函数在哪里被调用，函数的调用方式、传入的参数等信息。this就是记录的属性，会在函数执行的过程中用到。