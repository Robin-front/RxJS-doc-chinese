# 创建和订阅单一可观察序列 #

你不需要去实现 `Observable` 类去创建一个可观察序列。 同样的，你也不需要去实现 `Observer` 去订阅数据流。通过安装 Rx 库，你可以利用 `Observable`类型，它提供了许多操作符来根据零个，一个或多个元素去创建一个数据流。另外， RxJS 还提供 `subscribe` 方法允许你使用 `onNext`, `onError` 和 `onCompleted` 函数。

## 从零创建一个数据流 ##

在使用操作符之前，让我们看一看怎样使用 [`Rx.Observable.create`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/create.md) 方法从零创建 `Observable` 。

首先， 我们需要确认引用了 `rx.js` 核心文件。

```html
<script src="rx.js"></script>
```

如果我们使用 [Node.js](http://node.js)， 我们可以这样引入:

```js
var Rx = require('rx');
```

在这个例子中， 我们将只产生一个单一值42，然后标记为完成。 如果不需要清除，返回值是完全可选的。

```js
var source = Rx.Observable.create(observer => {
  // 产生一个单一值然后完成。
  observer.onNext(42);
  observer.onCompleted();

  // 任何清除的逻辑写在这里
  return () => console.log('disposed')
});

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 42
// => onCompleted

subscription.dispose();
// => disposed
```

对于大多数操作， 这完全是多余的，但这展示了非常基础的大部分 RxJS 操作符是如何工作的。

## 创建及订阅单一数据流 ##

接下来的例子使用 `Observable` 类的 [`range`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/range.md) 操作符来创建一个包含一些数字的单一数据流。观察者使用 `Observable` 类的 `Subscribe` 订阅这个数据流集合， 并且处理回调 `onNext`, `onError` and `onCompleted`。在我们的例子中，创建了一个从 x 开始的整数序列，然后接下来产生 y 个。

只要订阅了数据流，数据就会发送给观察者。`onNext`函数会打印出这个值。

```js
// 创建一个从 1 开始，包含 5 个整数的数据流
var source = Rx.Observable.range(1, 5);

// 打印每个值
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

当一个观察者订阅了一个数据流， `subscribe` 方法背后使用的异步操作取决于操作符。因些， `subscribe` 的调用是异步的，因为调用者在完成序列观察之前不会被阻塞。这篇文章 [Using Schedulers](schedulers.md) 提供了更多信息。

注意 [`subscribe`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/susbcribe.md) 方法返回一个 `Disposable`，所以你可以很容易地退订和销毁它。当你在可观察对象上调用 `dispose` 方法时，观察者将会停止监听数据流。正常来说，你不需要精确地调用 `dispose` 除非你需要提前退订，或者当数据流的生命周期比观察者的还长。 Rx 的订阅被设计成 `触发-丢弃` 的场景，并不需要终结者。注意到，可观察对象的操作符的默认表现是 只要有可能（比如，`onCompleted` 或 `onError` 消息被发送时），订阅就会被销毁。举个例子，下面的代码将会订阅 a 和 b 两个数据流。如果 a 抛出一个错误， x 会立即退订 b 。

```js
var x = Rx.Observable.zip(a, b, (a1, b1) => a1 + b1).subscribe();
```

还可以调整代码示例以使用观察者的 `Create` 运算符，创建并从指定的 `OnNext`, `OnError`, 和 `OnCompleted` 回调返回一个观察者。然后你可以传递 `observer` 给 `observable` 的 `subscribe` 方法。下面的例子展示了这种写法。

```js
// 创建包含 5个整数的数据流，从 1 开始
var source = Rx.Observable.range(1, 5);

// 创建观察者
var observer = Rx.Observer.create(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// 打印每个结果
var subscription = source.subscribe(observer);

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

另外，从零创建一个数据流，你也可以将已存在的 数据， 事件，回调以及 `promise` 转换成数据流。下一节的主题将会教你怎么做。

注意，这一节只展示了可以从零创建数据流的很少一部分操作符。学习更多其他的  `LINQ` 操作符， 可以查看 [Querying Observable Sequences](querying_observable_squences.md).
## Using a timer ##

The following sample uses the [`timer`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timer.md) operator to create a sequence. The sequence will push out the first value after 5 second has elapsed, then it will push out subsequent values every 1 second. For illustration purpose, we chain the [`timestamp`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timestamp.md) operator to the query so that each value pushed out will be appended by the time when it is published. By doing so, when we subscribe to this source sequence, we can receive both its value and timestamp.

First, we need to ensure we reference the proper files if in the browser.  Note that the RxJS NPM Package already includes all operators by default.

```html
<script src="rx.js"></script>
<script src="rx.time.js"></script>
```

Now on to our example

```js
console.log('Current time: ' + Date.now());

var source = Rx.Observable.timer(
  5000, /* 5 seconds */
  1000 /* 1 second */)
   .timestamp();

var subscription = source.subscribe(
  x => console.log(x.value + ': ' + x.timestamp));

/* Output may be similar to this */
// Current time: 1382560697820
// 0: 1382560702820
// 1: 1382560703820
// 2: 1382560704820
```

By using the `timestamp` operator, we have verified that the first item is indeed pushed out 5 seconds after the sequence has started, and each item is published 1 second later.

## Converting Arrays and Iterables to an Observable Sequence ##

Using the [`Rx.Observable.from`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/from.md) operator, you can convert an array to observable sequence.

```js
var array = [1,2,3,4,5];

// Converts an array to an observable sequence
var source = Rx.Observable.from(array);

// Prints out each item
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

You can also convert array-like objects such as objects with a length property and indexed with numbers.  In this case, we'll simply have an object with a length of 5.
```js
var arrayLike = { length: 5 };

// Converts an array to an observable sequence
var source = Rx.Observable.from(arrayLike, (v, k) => k);

// Prints out each item
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted

```


In addition, we can also use ES6 Iterable objects such as `Map` and `Set` using `from` to an observable sequence.  In this example, we can take a `Set` and convert it to an observable sequence.

```js
var set = new Set([1,2,3,4,5]);

// Converts a Set to an observable sequence
var source = Rx.Observable.from(set);

// Prints out each item
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

We can also do a `Map` as well by applying the same technique.

```js
var map = new Map([['key1', 1], ['key2', 2]]);

// Converts a Map to an observable sequence
var source = Rx.Observable.from(map);

// Prints out each item
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: key1, 1
// => onNext: key2, 2
// => onCompleted
```

The `from` method can also support ES6 generators which may already be in your browser, or coming to a browser near you.  This allows us to do such things as Fibonacci sequences and so forth and convert them to an observable sequence.

```js
function* fibonacci () {
  var fn1 = 1;
  var fn2 = 1;
  while (1){
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    yield current;
  }
}

// Converts a generator to an observable sequence
var source = Rx.Observable.from(fibonacci()).take(5);

// Prints out each item
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: 1
// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 5
// => onCompleted
```

## Cold vs. Hot Observables ##

Cold observables start running upon subscription, i.e., the observable sequence only starts pushing values to the observers when Subscribe is called. Values are also not shared among subscribers. This is different from hot observables such as mouse move events or stock tickers which are already producing values even before a subscription is active. When an observer subscribes to a hot observable sequence, it will get the current value in the stream. The hot observable sequence is shared among all subscribers, and each subscriber is pushed the next value in the sequence. For example, even if no one has subscribed to a particular stock ticker, the ticker will continue to update its value based on market movement. When a subscriber registers interest in this ticker, it will automatically get the latest tick.

The following example demonstrates a cold observable sequence. In this example, we use the Interval operator to create a simple observable sequence of numbers pumped out at specific intervals, in this case, every 1 second.

Two observers then subscribe to this sequence and print out its values. You will notice that the sequence is reset for each subscriber, in which the second subscription will restart the sequence from the first value.

First, we need to ensure we reference the proper files if in the browser.  Note that the RxJS NPM Package already includes all operators by default.

```html
<script src="rx.lite.js"></script>
```

And now to the example.

```js
var source = Rx.Observable.interval(1000);

var subscription1 = source.subscribe(
  x => console.log('Observer 1: onNext: ' + x),
  e => console.log('Observer 1: onError: ' + e.message),
  () => console.log('Observer 1: onCompleted'));

var subscription2 = source.subscribe(
  x => console.log('Observer 2: onNext: ' + x),
  e => console.log('Observer 2: onError: ' + e.message),
  () => console.log('Observer 2: onCompleted'));

setTimeout(() => {
  subscription1.dispose();
  subscription2.dispose();
}, 5000);

// => Observer 1: onNext: 0
// => Observer 2: onNext: 0
// => Observer 1: onNext: 1
// => Observer 2: onNext: 1
// => Observer 1: onNext: 2
// => Observer 2: onNext: 2
// => Observer 1: onNext: 3
// => Observer 2: onNext: 3
```

In the following example, we convert the previous cold observable sequence source to a hot one using the [`publish`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/publish.md) operator, which returns a `ConnectableObservable` instance we name `hot`. The [`publish`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/publish.md) operator provides a mechanism to share subscriptions by broadcasting a single subscription to multiple subscribers. The `hot` variable acts as a proxy by subscribing to `source` and, as it receives values from `source`, pushing them to its own subscribers. To establish a subscription to the backing source and start receiving values, we use the [`ConnectableObservable.prototype.connect`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/connect.md) method. Since `ConnectableObservable` inherits `Observable`, we can use `subscribe` to subscribe to this hot sequence even before it starts running. Notice that in the example, the hot sequence has not been started when `subscription1` subscribes to it. Therefore, no value is pushed to the subscriber. After calling Connect, values are then pushed to `subscription1`. After a delay of 3 seconds, `subscription2` subscribes to `hot` and starts receiving the values immediately from the current position (3 in this case) until the end. The output looks like this:

```
// => Current time: 1382562433256
// => Current Time after 1st subscription: 1382562433260
// => Current Time after connect: 1382562436261
// => Observer 1: onNext: 0
// => Observer 1: onNext: 1
// => Current Time after 2nd subscription: 1382562439262
// => Observer 1: onNext: 2
// => Observer 2: onNext: 2
// => Observer 1: onNext: 3
// => Observer 2: onNext: 3
// => Observer 1: onNext: 4
// => Observer 2: onNext: 4
```

First, we need to ensure we reference the proper files if in the browser.  Note that the RxJS NPM Package already includes all operators by default.

```html
<script src="rx.lite.js"></script>
```

Now onto the example!

```js
console.log('Current time: ' + Date.now());

// Creates a sequence
var source = Rx.Observable.interval(1000);

// Convert the sequence into a hot sequence
var hot = source.publish();

// No value is pushed to 1st subscription at this point
var subscription1 = hot.subscribe(
  x => console.log('Observer 1: onNext: %s', x),
  e => console.log('Observer 1: onError: %s', e),
  () => console.log('Observer 1: onCompleted'));

console.log('Current Time after 1st subscription: ' + Date.now());

// Idle for 3 seconds
setTimeout(() => {

  // Hot is connected to source and starts pushing value to subscribers
  hot.connect();

  console.log('Current Time after connect: ' + Date.now());

  // Idle for another 3 seconds
  setTimeout(() => {

    console.log('Current Time after 2nd subscription: ' + Date.now());

    var subscription2 = hot.subscribe(
      x => console.log('Observer 2: onNext: %s', x),
      e => console.log('Observer 2: onError: %s', e),
      () => console.log('Observer 2: onCompleted'));

  }, 3000);
}, 3000);

// => Current Time after connect: 1431197578426
// => Observer 1: onNext: 0
// => Observer 1: onNext: 1
// => Observer 1: onNext: 2
// => Current Time after 2nd subscription: 1431197581434
// => Observer 1: onNext: 3
// => Observer 2: onNext: 3
// => Observer 1: onNext: 4
// => Observer 2: onNext: 4
// => Observer 1: onNext: 5
// => Observer 2: onNext: 5
// => ...
```

**Analogies**

It helps to think of cold and hot Observables as movies or performances that one can watch ("subscribe").

- Cold Observables: movies.
- Hot Observables: live performances.
- Hot Observables replayed: live performances recorded on video.

Whenever you watch a movie, your run of the movie is independent of anyone else's run, even though all movie watchers see the same effects. On the other hand, a live performance is shared to multiple viewers. If you arrive late to a live performance, you will simply miss some of it. However, if it was recorded on video (in RxJS this would happen with a BehaviorSubject or a ReplaySubject), you can watch a "movie" of the live performance. A [`.publish().refCount()`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/refcount.md) live performance is one where the artists quit playing when no one is watching, and start playing again when there is at least one person in the audience.
