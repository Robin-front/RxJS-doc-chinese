## [`Rx.Observable.throw(exception, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/modular/observable/throw.js)

![throw](http://reactivex.io/documentation/operators/images/throw.c.png)

返回一个包含意外中止的observable，使用指定的调度器发送单个错误消息。

#### 参数
1. `dueTime` *(`Any`)*: 产生第一个值的绝对时间（指定为日期对象）或相对时间（指定为表示毫秒的整数）
2. `[scheduler=Rx.Scheduler.immediate]` *(`Scheduler`)*: 意外中止的调度程序。如果没有指定，则默认为`Rx.Scheduler.immediate`.

#### 返回值
*(`Observable`)*: 指定异常对象异常终止的可观察序列。

#### 例

```js
var source = Rx.Observable.return(42)
    .selectMany(Rx.Observable.throw(new Error('error!')));

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

// => Error: Error: error!
```

[](http://jsbin.com/luyaho/1/embed?js,console)
