## [`Rx.Observable.fromNodeCallback(func, [context], [selector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/fromnodecallback.js)

![fromNodeCallback](http://reactivex.io/documentation/operators/images/fromNodeCallback.png)

将一个 Node.js回调风格的函数转换为observable。这必须遵循`function (err, ...) `格式。

#### 参数
1. `func` *(`Function`)*: Function with a callback as the last parameter to convert to an Observable sequence.
2. `[context]` *(`Any`)*: 要执行的函数参数的上下文。如果没有指定，默认为`undefined`.
3. `[selector]` *(`Function`)*: 一个处理函数，接受第一个函数的结果作为该函数的参数，处理函数执行后返回的值作为最终输出值。A selector which takes the arguments from callback sans the error to produce a single item to yield on next.

#### 返回值
*(`Function`)*: 返回一个函数，当调用这个函数的时候返回一个observable。A function which when applied, returns an observable sequence with the callback arguments as an array if no selector given, else the object created by the selector function on success, or an error if the first parameter is not falsy.

#### 例
```js
var fs = require('fs'),
    Rx = require('rx');

// Wrap fs.exists
var rename = Rx.Observable.fromNodeCallback(fs.rename);

// 重命名文件，除了错误，不会返回参数。
var source = rename('file1.txt', 'file2.txt');

var subscription = source.subscribe(
    function () {
        console.log('Next: success!');
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: success!
// => Completed
```
