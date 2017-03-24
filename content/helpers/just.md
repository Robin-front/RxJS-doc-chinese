## [`Rx.helpers.just(value)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L6)

传入参数时，返回函数，被调用时返回传入的参数。

#### 参数
1. `value` *(Any)*: 需要返回的值.

#### 返回值
*(Function)*: 一个被调用时返回传入值的函数.

#### 例

```js
var just = Rx.helpers.just;

Rx.Observable.timer(100)
  .map(just('foo'))
  .subscribe(console.log.bind(console));
// => foo
```
