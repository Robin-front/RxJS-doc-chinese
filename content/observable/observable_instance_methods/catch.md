## [`Rx.Observable.prototype.catch(second | handler)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/catch.js)

![catch](http://reactivex.io/documentation/operators/images/Catch.png)

一个可观察对象异常时，继续订阅其他的可观察对象的结果。有一个别名 `catchException` 用于低于IE9浏览器。

#### 参数
1. `second` *(`Observable`)*: 用于在第一个序列中发生错误时，产生结果的第二个可观察序列
1. `handler` *(`Function`)*: 异常处理函数，返回第一个observable中出现的错误

#### 返回值
*(`Observable`)*: 一个可观察的序列，包含第一个序列的元素，接着是异常发生时处理器序列的元素。

#### 例

##### Using a second observable

```js
/* Using a second observable */
var source = Rx.Observable.throw(new Error())
    .catch(Rx.Observable.return(42));

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
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

[](http://jsbin.com/paxiz/1/embed?js,console)

##### Using a handler function

```js
/* Using a handler function */
var source = Rx.Observable.throw(new Error())
    .catch(function (e) {
        return Rx.Observable.return(e instanceof Error);
    });

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
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

[](http://jsbin.com/nikay/1/embed?js,console)
