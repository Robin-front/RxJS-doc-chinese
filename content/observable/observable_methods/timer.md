## [`Rx.Observable.timer(dueTime, [period], [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/timer.js)

![timer](http://reactivex.io/documentation/operators/images/timer.png)

返回一个可观察的序列，在到期时间之后产生第一个值，然后每一个周期产生一个值。注意，`rx.lite.js`则只支持相对时间。

#### 参数
1. `dueTime` *(Date|Number)*: 产生第一个值的绝对时间（指定为日期对象）或相对时间（指定为表示毫秒的整数）
2. `[period|scheduler=Rx.Scheduler.timeout]` *(Number|Scheduler)*: 产生后续值（指定为表示毫秒的整数）的周期。如果未指定，则生成的计时器不会重复发生。
3. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: 定时器的调度程序，如果没有指定，则使用`Rx.Scheduler.timeout`.

#### 返回值
*(`Observable`)*:一个可观察的序列，在到期时间之后产生第一个值，然后每一个周期产生一个值.

#### 例

```js
console.clear();
var source = Rx.Observable.timer(200, 100)
    .take(3);

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

// => Next: 200
// => Next: 100
// => Next: 100
// => Completed
```

[](http://jsbin.com/peleyayike/1/edit?html,js,console)
