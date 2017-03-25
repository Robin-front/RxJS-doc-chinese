## [`Rx.Observable.prototype.amb(rightSource)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/ambproto.js)

![amb](http://reactivex.io/documentation/operators/images/amb.png)

<rx-marbles key="amb"></rx-marbles>


从一系列流中，订阅最先发射的值的可观察对象并忽略其他的可观察对象。“amb”代表[ambiguous](http://blogs.msdn.com/b/jeffva/archive/2009/11/18/amb-materialize-and-dematerialize.aspx)。

#### 参数
1. `rightSource` *(`Observable`)*: 第二个可观察序列.

#### 返回值
*(`Observable`)*: 返回首先返回值的可观察对象

#### 例

```js
var first = Rx.Observable.timer(300).map(function () { return 'first'; });
var second = Rx.Observable.timer(500).map(function () { return 'second'; });

var source = first.amb(second);

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

// => Next: first
// => Completed
```

[](http://jsbin.com/joviwu/1/embed?js,console)


#### 应用

由于只取最先返回值的可观察对象。所以可以应用于任何抢答、秒杀等一对多竞态应用。
