## [`Rx.Observable.prototype.debounceWithSelector(durationSelector)`, `Rx.Observable.prototype.throttleWithSelector(durationSelector)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/debouncewithselector.js)

Ignores values from an observable sequence which are followed by another value within a computed debounced duration.

#### 参数
1. `durationSelector` *(`Function`)*: Selector function to retrieve a sequence indicating the throttle duration for each given element.

#### 返回值
*(`Observable`)*: The throttled sequence.

#### 例
```js
var array = [
    800,
    700,
    600,
    500
];

var source = Rx.Observable.for(
    array,
    function (x) {
        return Rx.Observable.timer(x)
    })
    .map(function(x, i) { return i; })
    .debounceWithSelector(function (x) {
        return Rx.Observable.timer(700);
    });

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

// => Next: 0
// => Next: 3
// => Completed
```
