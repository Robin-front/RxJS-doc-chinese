## [`Rx.Observable.fromCallback(func, [scheduler], [context], [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromcallback.js)

![fromCallback](http://reactivex.io/documentation/operators/images/fromCallback.png)

将一个回调函数转换成`observable`工厂函数.

#### 参数
1. `func` *(`Function`)*: 需要转换为`Observable` 的回调函数
2. `[scheduler=Rx.Scheduler.timeout]` *(`Scheduler`)*: 执行回调函数的调度器。如果没有指定，则默认为`Rx.Scheduler.timeout`.
3. `[context]` *(`Any`)*: 第一个函数参数的执行上下文。如果没有指定，则默认为 `undefined`
4. `[selector]` *(`Function`)*: 它以第一个参数函数中返回的值作为本函数的参数，处理以产生下一个项。

#### 返回值
*(`Function`)*: 一个函数，当执行所需的参数，产生一个可观察到的序列，如果没有给定的选择器（第四个参数），将以一个包含单值的可观察序列的回调，否则会通过选择器函数创建的对象。

#### 例
```js
var fs = require('fs'),
    Rx = require('rx');

// Wrap fs.exists
var exists = Rx.Observable.fromCallback(fs.exists);

// Check if file.txt exists
var source = exists('file.txt');

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + result);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: true
// => Completed
```
