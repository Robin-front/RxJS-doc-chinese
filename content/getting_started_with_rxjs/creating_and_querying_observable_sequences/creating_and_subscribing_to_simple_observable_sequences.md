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

## 使用定时器 ##

接下来的例子使用 [`timer`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timer.md) 操作符去创建一个数据流。 这个数据流将在5秒后输出第一个值，接着每1秒输出后面的值。为了说明， 我们配合 [`timestamp`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timestamp.md) 操作符去查询，使每一个被推出来的值将在发布时追加时间戳。这样，当我们订阅这个数据源时，我们可以接收到值和时间戳。

首先，我们需要确认我们是否在浏览器引入了相关的文件。注意 RxJS NPM 包已经默认包含了所有操作符。

```html
<script src="rx.js"></script>
<script src="rx.time.js"></script>
```

下面是我们的例子：

```js
console.log('Current time: ' + Date.now());

var source = Rx.Observable.timer(
  5000, /* 5 秒 */
  1000 /* 1 秒 */)
   .timestamp();

var subscription = source.subscribe(
  x => console.log(x.value + ': ' + x.timestamp));

/* 输出可能像这样子 */
// Current time: 1382560697820
// 0: 1382560702820
// 1: 1382560703820
// 2: 1382560704820
```

通过使用 `timestamp` 操作符，我们可以证实，第一个值确实是开始5秒后输出，然后每1秒输出一个值。

## 将数组和可迭代对象转换为数据流 ##

使用 [`Rx.Observable.from`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/from.md) 操作符，你可以将一个数组转换为数据流。

```js
var array = [1,2,3,4,5];

// 将数组转换为数据流
var source = Rx.Observable.from(array);

// 输出每个值
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

你也可以转换类数组结构，比如包含 `length` 属性和数字索引的对象。这种情况下，我们只简单有一个包含长度为5的对象。

```js
var arrayLike = { length: 5 };

// 转换数组为数据流
var source = Rx.Observable.from(arrayLike, (v, k) => k);

// 输出每个值
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

另外，我们也可以使用 ES6 可迭代对象，如 `Map` 和 `Set` 使用 `from` 转换成数据流。下面这个例子，我们将获取一个 `Set` 对象，并且将它转换成数据流。

```js
var set = new Set([1,2,3,4,5]);

// 转换 Set 为数据流
var source = Rx.Observable.from(set);

// 转出每个值
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

我们也可以将它应用在 `Map` 对象上面。

```js
var map = new Map([['key1', 1], ['key2', 2]]);

// 将 Map 转换成数据流
var source = Rx.Observable.from(map);

// 打印每个值
var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => onNext: key1, 1
// => onNext: key2, 2
// => onCompleted
```

`from` 方法也支持 ES6 generators，可以你的浏览器已经支持，或将要支持。这允许我们实现一些像 `斐波那契序列` 等，并将它们转换成数据流。

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

// 将 generator 转换成数据流
var source = Rx.Observable.from(fibonacci()).take(5);

// 打印每个值
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

## 冷（惰性） vs. 热（非惰性） 数据流 ##

冷数据流的开始运行取决于订阅，比如，数据流只有当 `subscribe` 调用的时候才开始输出值。用户之间也没有共享值。这些是与热数据流的不同之处，热数据像流鼠标移动事件或股票代码这样的订阅时就已经不断输出值。当观察者订阅热数据流时，它将会获取流的实时值。热数据流是与所有订阅者共享的，每个订阅者按顺序推送下一个值。举个例子，就算没有人订阅一个特定的股票，股票市场也将继续根据市场动向更新其价值。当有注册者对这支股票感兴趣时，它会自动获得股票的最新值。

下面的示例演示了一个冷数据流。这个例子中，我们使用了 `Interval` 操作符去创建一个单一数据流并在特定的时间间隔输出值，这个例子中是间隔1秒。

两个观察者订阅这个数据流并打印输出值。你会注意到数据流会为每个订阅者重置，第二个订阅者也是从第1个值开始的。

首先，我们需要确保在浏览器中引入了相关文件。注意 RxJS NPM 包已经默认包含了所有操作符。

```html
<script src="rx.lite.js"></script>
```

然后是例子：

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

接下来的例子中，我们使用 [`publish`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/publish.md) 操作符将前面的冷数据源转换成热数据源，返回一个 `ConnectableObservable` 实例，我们称为 `hot`。[`publish`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/publish.md) 操作符通过向多个订阅服务器广播单个订阅来提供共享订阅的机制。`hot`变量作为代理订阅 `source`，因为它从 `source` 接收值，推到自己的用户. 我们使用 [`ConnectableObservable.prototype.connect`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/connect.md) 建立订阅的备份源，并开始接收值。因为 `ConnectableObservable` 继承自 `Observable`， 我们可以在它运行之前使用 `subscribe` 去订阅这个热数据流。 在这个例子中要注意，当 `subscription1`订阅它的时候热数据流还没有开始。因些，没有值输出给订阅者。只有调用 `Connect` 之后，输出值才会推送给 `subscription1`。3秒的延迟之后，`subscription2` 订阅了热数据流，并且立即开始接收当前输出值（当前值是3），一直到最后。输出结果看起来像这样：

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

首先，我们需要确认我们引入了相关文件。注意 RxJS NPM包已经默认包含了所有操作符。

```html
<script src="rx.lite.js"></script>
```

接下来是例子！

```js
console.log('Current time: ' + Date.now());

// 创建一个数据流
var source = Rx.Observable.interval(1000);

// 将数据流转换成热数据流
var hot = source.publish();

// 第一个订阅时没有值输出
var subscription1 = hot.subscribe(
  x => console.log('Observer 1: onNext: %s', x),
  e => console.log('Observer 1: onError: %s', e),
  () => console.log('Observer 1: onCompleted'));

console.log('Current Time after 1st subscription: ' + Date.now());

// 空闲 3 秒
setTimeout(() => {

  // 热数据源连接并开始输出值给订阅者
  hot.connect();

  console.log('Current Time after connect: ' + Date.now());

  // 又空闲 3 秒
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

**类推**

这有助于思考冷热数据源的关系，就像一个人看（`subscribe`）电影和表演。

- 冷数据流：电影。
- 热数据流：现场演出
- 重播热数据流：录播现场演出

你管你何时观看电影，你看电影和别人看都是独立的，尽管所有观看者看的都是相同的内容。另一方面，一个表演是与多个观看都一起分享的。如果你迟到了，你将会错过一部分。不管怎样，如果记录下来了（在 RxJS中可以使用 `BehaviorSubject` 或 `ReplaySubject`），你也可像看电影一样看现场演出。[`.publish().refCount()`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/refcount.md) 现场表演是艺术家在没有人观看的情况下退出比赛，当观众中至少有一人出现时，他会重新开始演奏。
