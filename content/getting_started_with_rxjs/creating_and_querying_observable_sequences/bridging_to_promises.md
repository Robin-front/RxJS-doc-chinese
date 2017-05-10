# 桥接 Promises #

Promises 事实上是在JavaScript社区的标准，是ECMAScript标准的一部分。Promise 表示异步操作的最终结果.Promise 主要的调用方式是通过 `then` 方法，注册回调函数接受一个promise的最终值或无法履行 promise 的返回。你可以非常容易地创建它，通过包含 `resolve` 和 `reject` 两个函数的构造函数，分别代表给定的解析值或拒绝值。rxjs完全致力于标准和原生支持Promise为任何数量的方法，在那里他们可以与可观察序列的交替使用。

优点是，当你与之混合可观察序列的Promises不同的是，ES6 Promises 的标准，你可以取消语义意味着你可以无视后续的返回值，如果你不再有兴趣。其中 Promises 一个最大的问题就是取消，如取消操作，如一个XHR并不容易与现有的标准兼容，也不是只有最后一个值以确保没有出单的要求。通过可观察序列，您可以在多播行为中获得该行为，而不是 Promise 的单播行为。

下面这个操作符列表支持 Promises:
- [`Rx.Observable.amb`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/amb.md) | [`Rx.Observable.prototype.amb`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/ambproto.md)
- [`Rx.Observable.case`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/case.md)
- [`Rx.Observable.catch`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/catch.md) | [`Rx.Observable.prototype.catch`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/catchproto.md)
- [`Rx.Observable.combineLatest`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/combinelatest.md) | [`Rx.Observable.prototype.combineLatest`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/combinelatestproto.md)
- [`Rx.Observable.concat`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concat.md) | [`Rx.Observable.prototype.concat`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatproto.md)
- [`Rx.Observable.prototype.concatMap`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatmap.md)
- [`Rx.Observable.prototype.concatMapObserver`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatobserver.md)
- [`Rx.Observable.defer`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/defer.md)
- [`Rx.Observable.prototype.flatMap`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/selectmany.md)
- [`Rx.Observable.prototype.flatMapLatest`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/flatmaplatest.md)
- [`Rx.Observable.forkJoin`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/forkjoin.md) | [`Rx.Observable.prototype.forkJoin`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/forkjoinproto.md)
- [`Rx.Observable.if`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/if.md)
- [`Rx.Observable.merge`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/merge.md)
- [`Rx.Observable.prototype.mergeAll`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/mergeall.md)
- [`Rx.Observable.onErrorResumeNext`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/onerrorresumenext.md) | [`Rx.Observable.prototype.onErrorResumeNext`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/onerrorresumenextproto.md)
- [`Rx.Observable.prototype.selectMany`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/selectmany.md)
- [`Rx.Observable.prototype.selectSwitch`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/flatmaplatest.md)
- [`Rx.Observable.prototype.sequenceEqual`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/sequenceequal.md)
- [`Rx.Observable.prototype.skipUntil`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/skipuntil.md)
- [`Rx.Observable.startAsync`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/startasync.md)
- [`Rx.Observable.prototype.switch`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/switch.md)
- [`Rx.Observable.prototype.takeUntil`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/takeuntil.md)
- [`Rx.Observable.prototype.debounceWithSelector`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/debouncewithselector.md)
- [`Rx.Observable.prototype.timeoutWithSelector`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timeoutwithselector.md)
- [`Rx.Observable.while`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/while.md)
- [`Rx.Observable.prototype.window`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/window.md)
- [`Rx.Observable.withLatestFrom`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/withlatestfrom.md)
- [`Rx.Observable.zip`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/zip.md) | [`Rx.Observable.prototype.zip`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/zipproto.md)

基于这个，我们可以做一些有趣的事情，比如组合 Promises 和 数据流。

```js
var source = Rx.Observable.range(0, 3)
  .flatMap(x => Promise.resolve(x * x));

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 0
// => onNext: 1
// => onNext: 4
// => onCompleted
```

这只是提取 Promises 和rxjs可以一起做的事情，我们将一类单值和一类多值配合一起工作。

## 将 Promises 转换成数据流 ##

将Promise 对象转换成符合 ES6 标准，很容易实现 Promise 的行为一致。为了支持这个，我们提供 [`Rx.Observable.fromPromise`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/frompromise.md) 方法，调用 `then` 方法来处理成功和失败两种状态。

下面这个例子，我们使用 [RSVP](https://github.com/tildeio/rsvp.js) 库创建一个 Promise 对象。

```js
// Create a promise which resolves 42
var promise1 = new RSVP.Promise((resolve, reject) => resolve(42));

var source1 = Rx.Observable.fromPromise(promise1);

var subscription1 = source1.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted

// 创建一个包含错误 rejects 的 promise
var promise2 = new RSVP.Promise((resolve, reject) => reject(new Error('reason')));

var source2 = Rx.Observable.fromPromise(promise2);

var subscription2 = source2.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onError: reason
```

注意，这个例子中，这些 promises 变成数据流时，我们还可以做得更多。[Querying Observable Sequences](querying.md) 这篇主题将会给你展示如何将这个数据流映射到另一个，筛选它的内容，使您的应用程序只接收满足一定条件的值。

## 将可观察对象转换成 Promises ##

正如你可以将 Promise 转换成数据流，你也同样也可以将可观察对象转换成 Promise. 这要求原生支持 Promise, 或者添加了一个 Promise 库，比如 [Q](https://github.com/kriskowal/q), [RSVP](https://github.com/tildeio/rsvp.js), [when.js](https://github.com/cujojs/when) 或其他的。这些库必须确认适用于 ES6 标准，它提供 resolve 和 reject 两个函数。

```js
var p = new Promise((resolve, reject) => resolve(42));
```

我们可以使用 [`toPromise`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/frompromise.md) 方法，它允许你将一个可观察对象转换成 Promise. 这个方法接受一个 Promise 构造函数，如果不提供，将会实使默认的实现方式。 在下面第一个例子中，我们会使用 [RSVP](https://github.com/tildeio/rsvp.js) 去构造我们的 Promise 对象。

```js
// Return a single value
var source1 = Rx.Observable.just(1).toPromise(RSVP.Promise);

source1.then(
  value => console.log('Resolved value: %s', value),
  reason => console.log('Rejected reason: %s', reason));

// => Resolved value: 1

// Reject the Promise
var source2 = Rx.Observable.throwError(new Error('reason')).toPromise(RSVP.Promise);

source2.then(
  value => console.log('Resolved value: %s', value),
  reason => console.log('Rejected reason: %s', reason));

// => Rejected reason: Error: reason
```

如果一个实现没有提供构造函数给 [`toPromise`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/frompromise.md) 方法，它将会降级使用 `Rx.config.Promise` 设置的 Promise 实现方法。默认情况下这将被设置为运行时的ES6 Promise 实现，但可以很容易地通过指定配置信息重写。

```js
Rx.config.Promise = RSVP.Promise;

var source1 = Rx.Observable.just(1).toPromise();

source1.then(
  value => console.log('Resolved value: %s', value),
  reason => console.log('Rejected reason: %s', reason));

// => Resolved value: 1
```

如果你是一个纯 ES6 的环境，没有任何设置也应该能工作，它将使用运行时的 ES6 Promise 实现。

```js
var source1 = Rx.Observable.just(1).toPromise();

source1.then(
  value => console.log('Resolved value: %s', value),
  reason => console.log('Rejected reason: %s', reason));

// => Resolved value: 1
```

参考
- [Querying Observable Sequences](querying.md)
