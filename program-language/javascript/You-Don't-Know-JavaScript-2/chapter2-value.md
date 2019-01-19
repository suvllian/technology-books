## 第2章 值

### 1. 数组

JavaScript数组中可以容纳任何类型的值；数组声明后即可向其中加入值，不需要预先设定大小；可以创建稀疏数组。

数组可以通过索引访问，也能通过键值访问。

需要注意的是，**如果字符串能够被强制类型转换成数字，它会被当作数字索引处理。**

``` javascript
var arr = []
arr[0] = 0 // [0]
arr['1'] = 1 // [1]
arr['a'] = 2 // [0, 1, a:2]
```

类数组可以通过`Array.prototype.slice.call(array)`或`Array.from(array)`转换成数组。

### 2. 字符串

**JavaScript中字符串是不可变的，而数组是可变的。**
字符串不可变指的是字符串的成员函数不会改变其原始值，而是创建并返回一个新的字符串。而数组的成员函数是在其原始值的基础上进行操作。

字符串和数组有些类似，也有length属性和indexOf、concat等方法。字符串也可以借用数组的方法处理字符串。

``` javascript
var a = 'abc'
var b = Array.prototype.join.call(a, '-') // 'a-b-c'
var c = a.split('').reverse().join('') // 'cba'
```

### 3. 数字

`toFixed`方法可以指定小数部分的显示位数；`toPrecision`方法指定有效位数的显示位数。

``` javascript
var a = 42.49
a.toFixed(4) // '42.5900'
toPrecision(4) // '42.59'
```

上述方法也适用于数字字面量。但是会优先识别数字字面量的一部分，然后才是对象属性访问运算符。

``` javascript
42.toFixed(3) // syntaxError
(42).toFixed(3) // 42.000
0.42.toFixed(3) // 0.420
42..toFixed(3) // 42.000
```

`42.toFixed(3)`是无效语法，因为`42.`会被视为常量，所以没有属性运算符访问toFixed方法。

**浮点数相加精度**

``` javascript
0.1 + 0.2 === 0.3 // false
```

**在处理小数的时候需要特别注意。**二进制浮点数相加，精度并不是很准确，所以等式不成立。

最常见的办法是设置一个误差范围值，通常称为**机器精度**，这个值通常是`2.220446049250313e-16`。

从ES6开始，该值定义在`Number.EPSILON`中，可以直接拿来使用。

``` javascript
if (!Number.EPSILON) {
	Number.EPSILON = Math.pow(2, -52)
}

function numbersCloseEnoughToEqual(n1, n2) {
	return Math.abs(n1, n2) < Number.EPSILON
}

var a = 0.1 + 0.2
var b = 0.3

numbersCloseEnoughToEqual(a, b) // true
```


### 特殊数值

#### NaN

我们通常用`window.isNaN`方法判断一个变量是否是`NaN`，但是这个方法是**检查参数是否不是NaN，或者也不是数字**，就会导致结果不准确，ES6中`Number.isNaN`已经修复这个问题。

``` javascript
var a = 2
var b = 'foo'
isNaN(a) // false
isNaN(a/b) // true
isNaN(b) // true
```

所以正确的判断NaN的方法是

``` javascript
Number.isNaN = function(n) {
	return typeof n === 'number' && isNaN(n)
}
```

或者利用NaN不等于自身的特点

``` javascript
Number.isNaN = function(n) {
	retur n !== n
}
```

#### 负零

``` javascript
var a = 0 / 3  // -0
0 === -0 // true
a.toString() // '0'
a + '' // '0'
String(a) // '0'

+'-0' // -0
Number('-0') // -0
JSON.parse('-0') // -0
```

对于上述不容易判断是否相等的特殊数值，我们可以用`Object.is`来判断两个值是否绝对相等。

``` javascript
var a = 2 / 'foo'
var b = -3 * 0
Object.is(a, NaN) // true
Object.is(b, 0) // false
Object.is(b, -0) // true
```