## [`Rx.Observable.spawn(fn)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/spawn.js)

演算一个generator函数, 也可以是Promises，可观察序列，数组、对象、Generators和 普通函数。

#### 参数
1. `fn` *(`Function`)*: 需要演算的函数.

#### 返回值
*(`Observable`)*: 一个包含最终结果的 Observable

#### 例
```js
var Rx = require('rx');

var thunk = function (val) {
  return function (cb) {
    cb(null, val);
  };
};

var spawned = Rx.Observable.spawn(function* () {
  var v = yield thunk(12);
  var w = yield [24];
  var x = yield Rx.Observable.just(42);
  var y = yield Rx.Observable.just(56);
  var z = yield Promise.resolve(78);
  return v + w[0] + x + y + z;
});

spawned.subscribe(
  function (x) { console.log('next %s', x); },
  function (e) { console.log('error %s', e); },
  function () { console.log('completed'); }
);

// => next 212
// => completed
```
