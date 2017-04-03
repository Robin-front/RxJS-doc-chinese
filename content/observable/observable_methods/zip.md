## [`Rx.Observable.zip(...args, [resultSelector])`](https://github.com/Reactive-Extensions/RxJS/blob/master/src/core/linq/observable/zip.js)

![zip](http://reactivex.io/documentation/operators/images/zip.png)

当所有可观察的序列在相应的索引上产生一个元素时，使用选择器函数将指定的可观察的序列或承诺合并到一个可观察的序列中。如果省略了结果选择器函数，则将得到相应索引上可观察序列元素的列表。

#### 参数
1. `args` *(`Array`|`arguments`)*: Observable 源.
2. `[resultSelector]` *(Function)*: 一个函数，它将输入到指定的索引中并将它们组合在一起。如果省略，将得到相应索引的可观察序列元素的列表

#### 返回值
*(`Observable`)*: 一个可观察的序列，包含使用指定的结果选择器函数组合源元素的结果。

#### 例

##### Without a result selector

```js
var range = Rx.Observable.range(0, 5);

var source = Observable.zip(
  range,
  range.skip(1),
  range.skip(2)
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

// => Next: 0,1,2
// => Next: 1,2,3
// => Next: 2,3,4
// => Completed
```

##### Using arguments

```js
var range = Rx.Observable.range(0, 5);

var source = Rx.Observable.zip(
  range,
  range.skip(1),
  range.skip(2),
  function (s1, s2, s3) {
    return s1 + ':' + s2 + ':' + s3;
  }
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

// => Next: 0:1:2
// => Next: 1:2:3
// => Next: 2:3:4
// => Completed
```

##### Using promises and Observables

```js
var range = Rx.Observable.range(0, 5);

var source = Rx.Observable.zip(
  Promise.resolve(0),
  Promise.resolve(1),
  Rx.Observable.return(2),
  function (s1, s2, s3) {
    return s1 + ':' + s2 + ':' + s3;
  }
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

// => Next: 0:1:2
// => Completed
```

[](http://jsbin.com/tuset/1/embed?js,console)
