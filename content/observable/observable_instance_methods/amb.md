## [`Rx.Observable.prototype.amb(rightSource)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/ambproto.js)

![amb](http://reactivex.io/documentation/operators/images/amb.png)

<rx-marbles key="amb"></rx-marbles>


传播的可观察序列或Promise的最先返回值。“amb”代表[ambiguous](http://blogs.msdn.com/b/jeffva/archive/2009/11/18/amb-materialize-and-dematerialize.aspx)。

#### 参数
1. `rightSource` *(`Observable`)*: 第二个可观察序列.

#### 返回值
*(`Observable`)*: 一个可观察的序列，其表面为任何给定的序列，无论哪个首先返回。

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
