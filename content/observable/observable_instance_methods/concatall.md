## [`Rx.Observable.prototype.concatAll()`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/concatall.js)

![concatAll](http://reactivex.io/documentation/operators/images/concatAll.png)

通过按顺序连接内部Observables，将高阶Observable转换为一阶Observable。

#### 返回值
*(`Observable`)*: 一个将所有内部的流连接起来的observable。

#### 例

[](http://jsbin.com/rigut/1/embed?js,console)


```js
var clicks = Rx.Observable.fromEvent(document, 'click');
var higherOrder = clicks.map(ev => Rx.Observable.interval(1000).take(4));
var firstOrder = higherOrder.concatAll();
firstOrder.subscribe(x => console.log(x));

// Results in the following:
// (results are not concurrent)
// For every click on the "document" it will emit values 0 to 3 spaced
// on a 1000ms interval
// one click = 1000ms-> 0 -1000ms-> 1 -1000ms-> 2 -1000ms-> 3
```
