## [`Rx.Observable.forkJoin(...args [resultSelector])`]()

![forkJoin](http://reactivex.io/documentation/operators/images/forkJoin.png)

并行运行所有observable，并收集所有observable最后的返回值。不管每个流会发射多少值，都只取最后一个返回值。

#### 参数
1. `args` *(`Arguments` | `Array`)*: 数组 或 Observable 或 Promises
2. `resultSelector`: *(`Function`)*: `resultSelector`应用到所有产生的值。如果没有指定，`forkjoin`将返回结果为数组。

#### 返回值
*(`Observable`)*: 一个可观察的序列，用数组收集所有输入流的最后一个元素，如果指定`resultSelector`，将返回`resultSelector`包装的结果。

#### [Example](http://jsbin.com/sudura/2/edit?js,console)

```js
/* Using observables and Promises */
var source = Rx.Observable.forkJoin(
    Rx.Observable.return(42),
    Rx.Observable.range(0, 10),
    Rx.Observable.fromArray([1,2,3]),
    RSVP.Promise.resolve(56)
);

var subscription = source.subscribe(
  x => console.log(`onNext: ${x}`),
  e => console.log(`onError: ${e}`),
  () => console.log('onCompleted'));

// => onNext: [42, 9, 3, 56]
// => onCompleted
```

#### [Example](http://jsbin.com/sudura/2/embed?js,console)

```js
var source = Rx.Observable.forkJoin(
  Rx.Observable.just(42),
  Rx.Observable.just(56),
  function (x, y) { return x + y; }
);

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

// => Next: 98
// => Completed
```
