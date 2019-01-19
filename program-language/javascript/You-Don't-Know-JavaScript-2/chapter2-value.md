## 第2章 值

### 1. 数组

JavaScript数组中可以容纳任何类型的值；数组声明后即可向其中加入值，不需要预先设定大小；可能创建稀疏数组。

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