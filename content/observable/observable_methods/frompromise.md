## [`Rx.Observable.fromPromise(promise)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/frompromise.js)

Converts a Promises/A+ spec compliant Promise and/or ES2015 compliant Promise to an Observable sequence.

#### 参数
1. `promise` *(`Promise`)*: Promises/A+ spec compliant Promise to an Observable sequence.

#### 返回值
*(`Observable`)*: An Observable sequence which wraps the existing promise success and failure.

#### 例

##### Create a promise which resolves 42

[](http://jsbin.com/riyar/1/embed?js,console)

##### Create a promise which rejects with an error

[](http://jsbin.com/zuyeyi/1/embed?js,console)