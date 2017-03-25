## [`Rx.helpers.identity(x)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L4)

一个传入什么就返回什么的函数

#### 参数
1. `x` *(Any)*: 需要返回的值.

#### 返回值
*(Any)*: 即传入的参数.

#### 例

```js
var identity = Rx.helpers.identity;

// Returns its value
var x = identity(42);
console.log(x);
// => 42
```
