## [`Rx.Observable.mergeDelayError(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/mergedelayerror.js)

将多个Observable抹平合并进一个Observable，通过这个方法，让观察者能够成功接收到所有Observable正常发出的值，而不会被其中的某个错误通知中断。（错误会在流结束后发出）

它的表现很像 `Observable.prototype.mergeAll`，除了当合并的 Observables 中的某个发出错误（`onError`）时，`mergeDelayError`会延迟错误通知，直到所有合并的 Observables完成之后。

#### 参数
1. `args` *(Array|arguments)*: 需要合并的 Observables

#### 返回值
*(`Observable`)*: An Observable

#### 例
```js
var source1 = Rx.Observable.of(1,2,3);
var source2 = Rx.Observable.throwError(new Error('woops'));
var source3 = Rx.Observable.of(4,5,6);

var source = Rx.Observable.mergeDelayError(source1, source2, source3);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

// => 1
// => 2
// => 3
// => 4
// => 5
// => 6
// => Error: Error: woops
```
