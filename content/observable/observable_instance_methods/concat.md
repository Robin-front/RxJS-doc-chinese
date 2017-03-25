## [`Rx.Observable.prototype.concat(...args)`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/concatproto.js)

{% if book.isPdf %}

![concat](http://reactivex.io/documentation/operators/images/concat.png)

{% else %}

<rx-marbles key="concat"></rx-marbles>

{% endif %}

Concatenates all the observable sequences.  This takes in either an array or variable arguments to concatenate.

#### 参数
1. `args` *(`arguments` | `Array`)*: An array or arguments of Observable sequences.

#### 返回值
*(`Observable`)*: An observable sequence that contains the elements of each given sequence, in sequential order. 

#### 例

[](http://jsbin.com/coyapo/1/embed?js,console)

```js
/* Using Promises and Observable sequences */
var source1 = Rx.Observable.return(42);
var source2 = RSVP.Promise.resolve(56);

var source = Rx.Observable.concat(source1, source2);

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
// => Next: 56
// => Completed
```