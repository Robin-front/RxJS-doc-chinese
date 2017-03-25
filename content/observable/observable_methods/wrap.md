## [`Rx.Observable.wrap(fn)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/spawn.js)

Wrap the given generator fn into a function that returns an Observable.

#### 参数
1. `fn` *(`Function`)*: A generator function to wrap.

#### 返回值
*(`Observable`)*: A function once executed, returns an Observable.

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
