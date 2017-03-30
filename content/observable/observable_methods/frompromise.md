## [`Rx.Observable.fromPromise(promise)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/frompromise.js)

将一个兼容Promises/A+规范或 ES2015规范的 Promise 转化为 Obseervable

#### 参数
1. `promise` *(`Promise`)*: 兼容Promises/A+规范或 ES2015规范的 Promise

#### 返回值
*(`Observable`)*: 一个Observable，封装现有的Promises成功和失败接口。

#### 例

##### Create a promise which resolves 42

[](http://jsbin.com/riyar/1/embed?js,console)

##### Create a promise which rejects with an error

[](http://jsbin.com/zuyeyi/1/embed?js,console)
