## [`Rx.helpers.defaultSubComparer(x, y)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L9)

默认比较器，返回一个对象是否大于，小于或等于另一个。

#### 参数
1. `x` *(任意)*: 第一个比较值
2. `y` *(任意)*: 第二个比较值

#### 返回值
*(Number类型)*: 返回 `1` 如果 `x` 大于 `y`, `-1` 表示 `y` 大于 `x`, and `0` 表示相等.

#### 例

```js
var comparer = Rx.helpers.defaultSubcomparer;

// Should return 0
var x = 42, y = 42
console.log(comparer(x, y));
// => 0

// Should return -1
var x = new Date(0), y = new Date();
console.log(comparer(x, y));
// => -1

// Should return 1
var x = 43, y = 42;
console.log(comparer(x, y));
// => 1
```
