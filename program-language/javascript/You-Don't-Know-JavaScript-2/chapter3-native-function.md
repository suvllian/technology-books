## 第3章 原生函数

原生函数包括：`String()`、`Number()`、`Boolean()`、`Array()`、`Object()`、`Function()`、`RegExp()`、`Date()`、`Error()`、`Symbol()`。

### 1. 内部属性

所有`typeof`返回值为`object`的对象都包含一个内部属性`[[Class]]`，这个属性无法直接访问，可以通过`Object.prototype.toString()`来查看。

``` javascript
Object.prototype.toString.call(null) // "[object Null]"
```

### 2. 封装对象包装

基本类型值没有.length和.toString()等属性和方法，需要通过封装对象才能访问，此时Javascript会自动为基本类型包装一个封装对象。

``` javascript
var a = 'abc'
a.length = 3
```

一般不推荐直接使用封装对象，直接使用基本类型值，让js引擎自己决定什么时候应该用封装对象。

``` javascript
var a = new Boolean(false)

if (!a) {
 console.log('ops') // 不会执行到这里
}
```

### 3. 拆封

如果要获取封装对象中的基础类型值，可以使用`valueOf()`方法。

``` javascript
var a = new String('abc')
var b = new Number(42)
var c = new Boolean(true)

a.valueOf() // 'abc'
b.valueOf() // 42
c.valueOf() // true
```

在需要用到封装对象的基本类型值的时候，会发生隐式拆封，即强制类型转换。

``` javascript
var a = new String('abc')
var b = a + ''

typeof a // 'object'
typeof b // 'string'

```

### 4. 原生函数作为构造函数

对于数组、对象、函数、正则表达式，我们应尽量用字面量的形式去创建，避免使用构造函数。这样语法简单，执行效率也更高，JS引擎在代码执行前会对它们进行预编译和缓存。

#### 4.1 Array(...)

Array构造函数只有一个参数的时候，该参数会被作为数组的预设长度。

``` javascript
var a = new Array(3) // [empty * 3]
var b = [undefined, undefined, undefined]

a.join('-') // '--'
a.join('-') // '--'

a.map((value, index) => index) // [empty * 3]
b.map((value, index) => index) // [0, 1, 2]
```

`a.map(...)`之所以执行失败，是因为数组中不存在任何单元。
