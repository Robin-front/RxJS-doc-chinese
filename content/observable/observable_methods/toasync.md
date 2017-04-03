## [`Rx.Observable.toAsync(func, [scheduler], [context])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/toasync.js)

![toAsync](http://reactivex.io/documentation/operators/images/toAsync.png)

将函数转换为异步函数。每次调用所生成的异步函数会导致指定的调度程序上的原始同步函数的调用。
Converts the function into an asynchronous function. Each invocation of the resulting asynchronous function causes an invocation of the original synchronous function on the specified scheduler.

#### 参数
1. `func` *(`Function`)*: 需要转换成异步函数的函数
2. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: 调用函数的调度程序。如果没有指定，则默认为 `Rx.Scheduler.timeout`
3. `[context]` *(`Any`)*: 函数执行上下文， 如果没有指定，则默认为 `undefined`

#### 返回值
*(`Function`)*: 异步函数.

#### 例

```js
var func = Rx.Observable.toAsync(function (x, y) {
    return x + y;
});

// Execute function with 3 and 4
var source = func(3, 4);

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

// => Next: 7
// => Completed
```

[](http://jsbin.com/zokawu/1/embed?js,console)
