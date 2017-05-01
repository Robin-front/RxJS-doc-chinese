## [`Rx.Observable.wrap(fn)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/spawn.js)

将给定的generator函数封装到Observable中。

#### 参数
1. `fn` *(`Function`)*: 需要封装的 generator 函数.

#### 返回值
*(`Observable`)*: 返回一个函数，该函数执行后返回一个 Observable.

#### 例

```js
var Rx = require('rx');

var fn = Rx.Observable.wrap(function* (val) {
  return yield Observable.just(val);
});

fn(42).subscribe(
  function (x) { console.log('next %s', x); },
  function (e) { console.log('error %s', e); },
  function () { console.log('completed'); }
);

// => next 42
// => completed
```
