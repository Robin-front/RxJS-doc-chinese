## [`Rx.helpers.defaultComparer(x, y)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L8)

默认的等式比较器，不适用于函数。内部使用深度比较。

#### 参数
1. `x` *(任意值)*: 第一个比较值
2. `y` *(任意值)*: 第二个比较值

#### 返回值
*(Boolean)*: `true` if equal; else `false`.

#### 例

```js
var comparer = Rx.helpers.defaultComparer;

// Should return true
var x = 42, y = 42
console.log(comparer(x, y));
// => true

// Should return false
var x = new Date(0), y = new Date();
console.log(comparer(x, y));
// => false
```
