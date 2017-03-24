## [`Rx.helpers.just(value)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/headers/basicheader.js#L6)

A function which takes an argument and returns a function, when invoked, returns the argument.

#### 参数
1. `value` *(Any)*: The value to return.

#### 返回值
*(Function)*: A function, when invoked, returns the value.

#### 例 

```js
var just = Rx.helpers.just;

Rx.Observable.timer(100)
  .map(just('foo'))
  .subscribe(console.log.bind(console));
// => foo
```