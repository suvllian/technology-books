### 1. 内置类型
基本数据类型：null、undefined、boolean、number、string、object、symbol

**检测null值的类型**

``` javascript
var a = null
(!a && typeof a === 'object') // true
```

**`function(函数)`是object的子类型**

函数不仅是对象，还可以拥有属性。函数对象的length是其声明的参数个数。

``` javascript
function foo(a, b) {}

foo.length // 2
```

### 2. 值和类型
Js中变量是没有类型的，只有值才有。变量可以随时持有任何类型的值。

对变量执行`typeof`时，得到的结果是变量持有的值的类型，而不是变量的类型。

``` javascript
var a = 2
typeof a // number

a = 'foo'
typeof a // string

typeof typeof 42 // string
```

**undefined和undeclared是不同的**

``` javascript
var a
a // undefined
b // ReferenceError: b is not defined
```
undefined和undeclared是不同的，undefined是声明未赋值，undeclared是未声明。

``` javascript
var a
typeof a // undefined
typeof b // undefined
```

对于undeclared的变量，typeof照样返回undefined，是因为typeof有一种特殊的安全防范机制。

所以通过typeof的安全方法机制判断判断undefined变量和undeclared变量。

``` javascript
if (typeof DEBUG !== 'undefined') { /* do sth */  }

```
