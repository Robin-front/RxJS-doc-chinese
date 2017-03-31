## [`Rx.Observable.onErrorResumeNext(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/onerrorresumenext.js)

![onErrorResumeNext](http://reactivex.io/documentation/operators/images/onErrorResumeNext.png)

当observable正常终止或抛出错误时，不会报错，继续观察下一个observable。

#### 参数
1. `args` *(Array|arguments)*: 需要连接的Observable

#### 返回值
*(`Observable`)*: 返回一个连接所有Observables 的 observable，就算其中某个observable异常终止。

#### 例

```js
var source1 = Rx.Observable.throw(new Error('error 1'));
var source2 = Rx.Observable.throw(new Error('error 2'));
var source3 = Rx.Observable.return(42);

var source = Rx.Observable.onErrorResumeNext(source1, source2, source3);

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

[](http://jsbin.com/zewox/1/embed?js,console)
