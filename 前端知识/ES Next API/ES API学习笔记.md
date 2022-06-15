***ES API

*** String.prototype.replace

```javascript
str.replace(regexp|substr, newSubStr| replacer: function)
```

newSubStr - 可使用的参数

$$   $&  $`  $'  $n  $<Name>

replacer  (match, p1, p2, p3, offset, originString: striing)



####  块级作用域

- 暂时性死区

1. 在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
2. “暂时性死区”也意味着`typeof`不再是一个百分之百安全的操作。 



#### Promise的理解

Promise 在语义上代表了异步操作的主体。这种准确、清晰的定位极大推动了它在编程中的普及，因为具有单一职责，而且将份内事做到极致的事物总是具有病毒式的传染力。分离输入输出参数、错误冒泡、串行/并行控制流等特性都成为 Promise 横扫异步操作编程领域的重要砝码，以至于 ES6 都将其收录，并已在 Chrome、Firefox 等现代浏览器中实现。

1. 分离输入输出参数
2. 错误冒泡
3. 串行/并行控制流等特性