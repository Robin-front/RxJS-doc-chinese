## [`Observable.prototype.singleInstance()`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/singleinstance.js)

Returns a "cold" observable that becomes "hot" upon first subscription, and goes "cold" again when all subscriptions to it are disposed.

At first subscription to the returned observable, the source observable is subscribed to. That source subscription is then shared amongst each subsequent simultaneous subscription to the returned observable. 

When all subscriptions to the returned observable have completed, the source observable subscription is disposed of.

The first subscription after disposal starts again, subscribing one time to the source observable, then sharing that subscription with each subsequent simultaneous subscription.

#### 返回值
*(`Observable`)*: An observable sequence that stays connected to the source as long as there is at least one subscription to the observable sequence.

#### 例
```js
var interval = Rx.Observable.interval(1000);

var source = interval
    .take(2)
    .doAction(function (x) {
        console.log('Side effect');
    });

var single = source.singleInstance();

// two simultaneous subscriptions, lasting 2 seconds
single.subscribe(createObserver('SourceA'));
single.subscribe(createObserver('SourceB'));

setTimeout(function(){
    // resubscribe two times again, more than 5 seconds later,
    // long after the original two subscriptions have ended
    single.subscribe(createObserver('SourceC'));
    single.subscribe(createObserver('SourceD'));
}, 5000);

function createObserver(tag) {
    return Rx.Observer.create(
        function (x) {
            console.log('Next: ' + tag + x);
        },
        function (err) {
            console.log('Error: ' + err);
        },
        function () {
            console.log('Completed: ' + tag);
        });
}

// => Side effect
// => Next: SourceA0
// => Next: SourceB0
// => Side effect
// => Next: SourceA1
// => Next: SourceB1
// => Completed: SourceA
// => Completed: SourceB
// => Side effect
// => Next: SourceC0
// => Next: SourceD0
// => Side effect
// => Next: SourceC1
// => Next: SourceD1
// => Completed: SourceC
// => Completed: SourceD
```

