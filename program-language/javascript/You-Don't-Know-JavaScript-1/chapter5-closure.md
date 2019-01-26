## 第5章 作用域闭包

当函数可以记住并访问所在的词法作用域，即使函数是当前词法作用域之外执行，这时就产生了闭包。  
闭包使得函数可以继续访问定义时的词法作用域。只要使用了回调函数，实际上就是在使用闭包。

#### 闭包经典问题：

``` javascript
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}

// 输出5次6
```

#### 解决办法

##### IIFE

``` javascript
for (var i = 1; i <= 5; i++) {
  (function() {
    setTimeout(function timer() {
      console.log(i)
    }, i * 1000)
  })()
}
```

##### 变量替换

``` javascript
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j)
    }, j * 1000)
  })(i)
}
```

##### 块作用域

``` javascript
for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}
```