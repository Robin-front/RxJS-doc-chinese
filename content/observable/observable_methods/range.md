## [`Rx.Observable.range(start, count, [scheduler])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/range.js)

![range](http://reactivex.io/documentation/operators/images/range.png)

在指定范围内生成一个可观察的整数序列，使用指定的调度程序发送观察者消息。

#### 参数
1. `start` *(`Number`)*: 序列中第一个整数的值
2. `count` *(`Number`)*: 生成的序列整数的个数.
3. `[scheduler=Rx.Scheduler.currentThread]` *(`Scheduler`)*: 执行生成循环的调度器。如果没有指定，默认为 `Scheduler.currentThread`.

#### 返回值
*(`Observable`)*: 包含一系列连续整数的可观察序列

#### 例

```js
var source = Rx.Observable.range(0, 3);

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

// => Next: 0
// => Next: 1
// => Next: 2
// => Completed
```

[](http://jsbin.com/bapay/1/embed?js,console)
