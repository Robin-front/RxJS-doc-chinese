## [`Rx.Observable.combineLatest(...args, [resultSelector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/combinelatest.js)

{% if book.isPdf==true %}

![combineLatest](http://reactivex.io/documentation/operators/images/combineLatest.png)

{% endif %}

Merges the specified observable sequences into one observable sequence by using the selector function whenever any of the observable sequences produces an element. This can be in the form of an argument list of observables or an array. If the result selector is omitted, a list with the elements will be yielded.

#### Arguments
1. `args` *(arguments | Array)*: An array or arguments of Observable sequences.
1. `[resultSelector]` *(`Function`)*: Function to invoke whenever either of the sources produces an element. If omitted, a list with the elements will be yielded.

#### Returns
*(`Observable`)*: An observable sequence containing the result of combining elements of the sources using the specified result selector function.

{% if book.isPdf %}

#### [Example](http://jsbin.com/kewig/4/edit?js,console)

```js
/* Have staggering intervals */
var source1 = Rx.Observable.interval(100)
    .map(i => `First: ${i}`);

var source2 = Rx.Observable.interval(150)
    .map(i => `Second: ${i}`);

// Combine latest of source1 and source2 whenever either gives a value
var source = Rx.Observable.combineLatest(
        source1,
        source2
    ).take(4);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: First: 0, Second: 0
// => onNext: First: 1, Second: 0
// => onNext: First: 1, Second: 1
// => onNext: First: 2, Second: 1
// => onCompleted
```

{% else %}

#### Example
[](http://jsbin.com/kewig/4/embed?js,console)

{% endif %}

{% if book.isPdf %}

#### [Example](http://jsbin.com/kewig/2/edit?js,console)

```js
/* Have staggering intervals */
var source1 = Rx.Observable.interval(100)
  .map(function (i) { return 'First: ' + i; });

var source2 = Rx.Observable.interval(150)
  .map(function (i) { return 'Second: ' + i; });

// Combine latest of source1 and source2 whenever either gives a value
var source = Rx.Observable.combineLatest(
    source1,
    source2,
    function (s1, s2) { return s1 + ', ' + s2; }
  ).take(4);

var subscription = source.subscribe(
  function (x) {
    console.log('Next: %s', x);
  },
  function (err) {
    console.log('Error: %s', err);
  },
  function () {
    console.log('Completed');
  });

// => Next: First: 0, Second: 0
// => Next: First: 1, Second: 0
// => Next: First: 1, Second: 1
// => Next: First: 2, Second: 1
// => Completed
```

{% else %}

#### Example
[](http://jsbin.com/kewig/2/embed?js,console)

{% endif %}
