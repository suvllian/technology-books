## 第4章 提升

变量是现有声明后有赋值。函数赋值会先被提升，然后才是变量。

``` javascript
foo() // 1
var foo

function foo() {
  console.log(1)
}

foo = function() {
  console.log(2)
}
```

后面的函数声明可以覆盖前面的。

``` javascript
foo() // 3
var foo

function foo() {
  console.log(1)
}

foo = function() {
  console.log(2)
}

function foo() {
  console.log(3)
}
```