## [`Rx.Observable.fromPromise(promise)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/frompromise.js)

将一个兼容Promises/A+规范或 ES2015规范的 Promise 转化为 Obseervable

#### 参数
1. `promise` *(`Promise`)*: 兼容Promises/A+规范或 ES2015规范的 Promise

#### 返回值
*(`Observable`)*: 一个Observable，封装现有的Promises成功和失败接口。

#### 例

##### Create a promise which resolves 42

```js
// Create a promise which resolves 42
var promise = new RSVP.Promise(function (resolve, reject) {
    resolve(42);
});

var source1 = Rx.Observable.fromPromise(promise);

var subscription1 = source1.subscribe(
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
[](http://jsbin.com/riyar/1/embed?js,console)

##### Create a promise which rejects with an error

```js
// Create a promise which rejects with an error
var promise = new RSVP.Promise(function (resolve, reject) {
    reject(new Error('reason'));
});

var source1 = Rx.Observable.fromPromise(promise);

var subscription1 = source1.subscribe(
    function (x) {
        console.log('Next: ' + x);
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Error: Error: reason
```
[](http://jsbin.com/zuyeyi/1/embed?js,console)
