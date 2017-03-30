## [`Rx.Observable.interval(period, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/interval.js)

![interval](http://reactivex.io/documentation/operators/images/interval.png)


为 Observable 设置流产生值的间隔时间。

#### 参数
1. `period` *(`Number`)*: 产生结果序列中的值的时间（指定为表示毫秒的整数）。
2. `[scheduler]` *(Scheduler=Rx.Scheduler.timeout)*: 调度器，如果没有提供，则默认为`Rx.Scheduler.timeout`

#### 返回值
*(`Observable`)*: 具有指定周期的 Observable

#### 例

```js
var source = Rx.Observable
    .interval(500 /* ms */)
    .timeInterval()
    .take(3);

var subscription = source.subscribe(
    function (x) {
        console.log('Next:', x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: {value: 0, interval: 500}
// => Next: {value: 1, interval: 500}
// => Next: {value: 2, interval: 500}
// => Completed
```
[](http://jsbin.com/lozay/1/embed?js,console)
