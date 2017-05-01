## [`Rx.Observable.merge([scheduler], ...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/merge.js)

![merge](http://reactivex.io/documentation/operators/images/merge.png)

合并多个 Observable 和 Promises，并返回一个新的 Observable.

#### 参数
1. `[scheduler]` *(Scheduler=Rx.Scheduler.timeout)*: 调度器，如果没有指定，则默认为 `Rx.Scheduler.immediate`.
1. `args` *(Array|arguments)*: 需要合并的多个 Observable。

#### 返回值
*(`Observable`)*: 合并后的 Observable

#### 例

```js
var source1 = Rx.Observable.interval(100)
    .timeInterval()
    .pluck('interval');
var source2 = Rx.Observable.interval(150)
    .timeInterval()
    .pluck('interval');

var source = Rx.Observable.merge(
    source1,
    source2).take(10);


var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 100
// => Next: 150
// => Next: 100
// => Next: 150
// => Next: 100
// => Completed
```
[](http://jsbin.com/yicit/1/embed?js,console)
