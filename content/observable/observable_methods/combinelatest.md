## [`Rx.Observable.combineLatest(...args, [resultSelector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/combinelatest.js)

![combineLatest](http://reactivex.io/documentation/operators/images/combineLatest.png)

当任一可观察对象返回值时，使用指定的函数将指定的可观察对象合并为一个可观察对象。这可以是参数列表或数组的形式。如果省略了第三个参数，则将直接生成包含最新返回值的列表。

#### 参数
1. `args` *(arguments | Array)*: 一个数组或是可观察对象列表
1. `[resultSelector]` *(`Function`)*: 最新结果的处理函数，如果省略，则直接返回结果列表。

#### 返回值
*(`Observable`)*: 由传入的可观察序列的结果，经过处理函数合并后，组成的可观察序列。

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


#### 联想与应用

看到 `combine` ，估计用过 `redux` 的童鞋都要跳起来了，这特么不就可以用于返回最新 `store`么。
每当有任一流返回最新值，都返回全部流的最新值，并通过处理函数合并。
