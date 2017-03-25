## [`Rx.Observable.prototype.distinctUntilChanged([keySelector], [comparer])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/distinctuntilchanged.js)

{% if book.isPdf %}

![distinctUntilChanged](http://reactivex.io/documentation/operators/images/distinctUntilChanged.png)

{% else %}

<rx-marbles key="distinctUntilChanged"></rx-marbles>

{% endif %}

Returns an observable sequence that contains only distinct contiguous elements according to the keySelector and the comparer.

#### 参数
1. `[keySelector]` *(`Function`)*: A function to compute the comparison key for each element.
2. `[comparer]` *(`Function`)*: Equality comparer for computed key values. If not provided, defaults to an equality comparer function.

#### 返回值
*(`Observable`)*: An observable sequence only containing the distinct elements, based on a computed key value, from the source sequence.

#### 例
```js
/* Without key selector */
var source = Rx.Observable.fromArray([
        42, 42, 24, 24
    ])
    .distinct();

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: 42
// => Next: 24
// => Completed 

/* With key selector */
var source = Rx.Observable.fromArray([
        {value: 42}, {value: 24}, {value: 42}, {value: 24}
    ])
    .distinct(function (x) { return x.value; });

var subscription = source.subscribe(
    function (x) {
        console.log('Next: ' + x.toString());
    },
    function (err) {
        console.log('Error: ' + err);   
    },
    function () {
        console.log('Completed');   
    });

// => Next: { value: 42 }
// => Next: { value: 24 }
// => Completed 
```