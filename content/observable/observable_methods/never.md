## [`Rx.Observable.never()`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/never.js)

![never](http://reactivex.io/documentation/operators/images/never.png)

返回一个 observable，但它不会发出任何值，（也不会调用 `onCompleted`）可以用来表示流一直持续无限的时间。(e.g. when using reactive joins)

> P.S. 它和 `empty` 的区别，`empty`不会发出任何值，但会立即调用 `onCompleted`。 `never` 单独是感觉没什么用处，但是当与其他 observable合并时，可以生成一个无限持续的流 

#### 返回值
*(`Observable`)*: An observable sequence whose observers will never get called.

#### 例

```js
// This will never produce a value, hence never calling any of the callbacks
var source = Rx.Observable.never();

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
```

[](http://jsbin.com/nuyawe/1/embed?js,console)
