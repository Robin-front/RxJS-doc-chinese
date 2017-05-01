## [`Rx.Observable.startAsync(functionAsync)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/startasync.js)

![startAsync](http://reactivex.io/documentation/operators/images/startAsync.png)

调用异步函数，通过可观察的序列来显示结果。

> P.S. 我一开始也不明白，这和 `start` 方法有什么区别呢，原来一个是 `异步地`调用函数，一个是 调用`异步函数`。

#### 参数
1. `functionAsync` *(`Function`)*: 返回一个 Promise 异步函数

#### 返回值
*(`Observable`)*: 一个可观察的序列，暴露Promises函数的值或错误。

#### 例

```js
var source = Rx.Observable.startAsync(function () {
    return RSVP.Promise.resolve(42);
});

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

[](http://jsbin.com/jucoh/1/embed?js,console)
