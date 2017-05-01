## [`Rx.Observable.prototype.and(rightSource)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/and.js)

![and](http://reactivex.io/documentation/operators/images/and_then_when.C.png)


Propagates the observable sequence that reacts first.

#### 参数
1. `right` *(`Observable`)*: Observable sequence to match with the current sequence.

#### 返回值
*(`Pattern`)*: Pattern object that matches when both observable sequences have an available value.

#### 例

```js
// Choice of either plan, the first set of timers or second set
var source = Rx.Observable.when(
    Rx.Observable.timer(200).and(Rx.Observable.timer(300)).then(function (x, y) { return 'first'; }),
    Rx.Observable.timer(400).and(Rx.Observable.timer(500)).then(function (x, y) { return 'second'; })
);

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
// => Next: second
// => Completed   
```

[](http://jsbin.com/boyane/1/embed?js,console)
