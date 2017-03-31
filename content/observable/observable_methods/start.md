## [`Rx.Observable.start(func, [scheduler], [context])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/start.js)

![start](http://reactivex.io/documentation/operators/images/start.png)

在指定的调度程序上异步地调用指定的函数，通过可观察的序列来显示结果。

#### 参数
1. `func` *(`Function`)*: 异步调用的函数.
2. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: 执行函数的调度程序。如果没有指定，则默认为 `Scheduler.timeout`.
3. `[context]` *(`Any`)*: 要执行的函数参数的上下文。如果未指定，则默认为 `undefined`.

#### 返回值
*(`Observable`)*: 显示函数的结果值或异常的可观察序列

#### 例

```js
var context = { value: 42 };

var source = Rx.Observable.start(
    function () {
        return this.value;
    },
    Rx.Scheduler.timeout,
    context
);

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

// => Next: 42
// => Completed
```

[](http://jsbin.com/xitili/1/embed?js,console)
