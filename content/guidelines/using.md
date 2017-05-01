# 使用 Rx 的奇淫技巧

### 尝试画珠宝图 ###

为你想创建的流画一个珠宝图。 通过画珠宝图，你将会很清楚你应该使用哪些操作符。

珠宝图就是每个珠宝表示当前的一个事件或状态。珠宝图需要包含输入和输出流。

<img src="https://raw.githubusercontent.com/Reactive-Extensions/RxJS/master/doc/designguidelines/images/throttleWithTimeout.png" alt="throttleWithSelector">

通过画珠宝图，我们可以看到，我们在异步调用事件回调之前，需要延迟检测用户的输入。在这个例图里展示的是`throttle`操作符的延迟。从一个流创建另一个流，我们会使用 `flatMap` 或 `selectMany` 操作符。然后就有了下面的代码：

```js
var dictionarySuggest = userInput.throttle(250).flatMap(input => serverCall(input));
```

#### 何时忽略这条指南 ####

如果你感觉你已经可以很熟练地编写出你想要的流，你可以省去画珠宝图这一步。不管怎样，就算是 Rx 团队的成员在写代码的时候也仍然会先画一画珠宝图。

### 调用 `subscribe` 时传递多个参数 ###

为了方便， Rx 提供一个`subscribe`方法来加载观察者的回调函数。

观察者只需要实现这三个方法（`onNext`, `onError` & `onCompleted`）。 `subscribe`方法的扩展允许开发人员使用这些方法的默认选项。

比如： 当调用`subscribe`方法时只有一个`onNext`参数，`onError`将捕获来自事件流的异常。`onCompleted`在这里什么也不会做。

大部分情况下，处理异常是很重要的（不管是对于恢复还是中断应用程序）。

知道事件流是否完成也经常是很重要的。举个例子，告诉用户他的操作是否完成了。

所以，最好提供完整的三个参数给 `subscribe` 操作符。

RxJS还提供了三种方便的方法，其仅订阅所期望的序列的一部分。其他的处理程序会默认为原来的行为。有三个这样的功能：
- `subscribeOnNext`: 只对应 `onNext` 消息
- `subscribeOnError`: 只对应 `onError` 消息
- `subscribeOnCompleted`: 只对应 `onCompleted` 消息.

#### 何时忽略这条指南 ####

- 当流确定不会有完成状态，比如 `keyup`事件。
- 当流确定不会抛出异常，比如一个事件，一个完全确定的流。
- 当默认行为是符合预期的时候。

### 考虑通过特定的调度程序并发引入操作符 ###

相比使用` observeon `操作符来改变可观察序列产生消息的执行上下文，更好的做法是在正确的地方开始创建并发。 通过正确的调度器将会减少 `ObserveOn`操作符的使用。

#### 例 ####

```js
Rx.Observable.range(0, 90000, Rx.Scheduler.requestAnimationFrame).subscribe(draw);
```

在这个例子中，来自`range`操作符的回调将会通过`window.requestAnimationFrame`传递。在这个例子中， `range`操作符的回调将被调用。默认情况下，当递归调用立即执行时，`range`过载将会代替 `onNext`在`Rx.Scheduler.currentThread`上的调用。 通过提供`Rx.Scheduler.requestAnimationFrame`调度程序， 所有来自observable的消息都将会在 `window.requestAnimationFrame`回调中产生。

#### 何时忽略这条指南 ####

当结合来自不同执行上下文的几个事件时，使用指南4.4将所有消息尽可能晚地放在特定的执行上下文。

### 尽可能少且尽可能迟地调用`observeOn` 操作符 ###

通过使用 `observeOn` 操作符， 一个预定的功能是通过原始的消息流来获取信息。这可能会改变时序信息以及对系统施加额外的压力。在查询中延迟使用这个操作符可以改善这两个问题。

#### Sample ####

```js
var result = xs.throttle(1000)
  .flatMap(x => ys.takeUntil(zs).sample(250).map(y => x + y))
  .merge(ws)
  .filter(x => x < 10)
  .observeOn(Rx.Scheduler.requestAnimationFrame);
```

这个例子合并了多个 运行在不同上下文的 observable 。这个查询筛选掉了大部分信息。将`observeOn`操作符放在查询中的前面会对筛选出来的消息做额外的工作。最后才调用 `observeOn` 将会最大限度地提高性能。

#### 何时忽略这条指南 ####

如果你使用的 observable 并没有指定不同的上下文环境。这种情况下可以不必使用 `observeOn` 操作符。

### 关注内存限制 ###

RxJS 有很多操作符和类可以在内存中创建 observable, 比如：`replay` 操作符。当这些内存存储着 observable 时，这些缓存的大小将取决于 observable 的操作。如果缓存过大，将会造成内存溢出。有许多缓冲操作符提供策略来限制缓冲区，不管是从时间方面还是大小。提供这个限制将解决内存压力问题。

#### 例子 ####

```js
var result = xs.replay(null, 10000, 1000 * 60 /* 1 hr */).refCount();
```

这个例子中，`replay` 操作符创建了一个 buffer. 我们有限制这个 buffer 最多包含 10,000 条信息以及最多保留这些信息1小时。

#### 何时忽略这条指南 ####

当 observable 创建了大量的信息只填充了一小块 buffer， 或者当 buffer本身有大小限制。

### 使用 `do`/`tap` 操作符的副作用很明显 ###

有很多 Rx 操作符使用函数作为参数，这可以在这些参数中传递任何有效的用户代码。这些代码可以改变全局状态（比如改变全局变量，读写硬盘等等）。

Rx 是通过每个操作符组合起来运行的（除了共享操作符，例如“publish”）。这将使副作用发生在每个订阅。

如果这种表现是期望的行为，最好弄清楚在 `do`/`tap` 操作符中有副作用的这部分代码。这些方法会过载，只能调用指定的方法，比如 `doOnNext`/`tapOnNext`，`doOnError`/`tapOnError`,`doOnCompleted`/`tapOnCompleted`

#### 例 ####

```js
var result = xs.filter(x => x.failed).tap(x => log(x));
```

这个例子中，过滤失败的消息。将它们分发到订阅observable的代码之前记录该消息。此记录有一个副作用（比如：将消息放置在计算机的事件日志中）并明确地通过调用`do`/`tap`操作符。

### 假设消息可以传达，直到退订完成 ###

由于RxJS 使用推模式，消息可以通过不同的上下文环境发送。 当退订的时候，消息可能还在路上。当退订还没有完成的时候，这些消息仍然可以被传达。当控制权被返回时，消息将不能再传达。退订过程可以是在一个不同的上下文环境中进行。

#### 何时忽略这条指南 ####

一旦 `onCompleted` 或 `onError` 方法被调用，RxJS语法可以保证订阅已结束。

### 使用 `publish` 操作符分享副作用 ###

因为许多 observable是冷门的[\(see cold vs. hot on Channel 9\)](http://channel9.msdn.com/Blogs/J.Van.Gogh/Rx-API-in-depth-Hot-and-Cold-observables), 每个订阅都有单独的副作用。 某些情况下，这些副作用只发生一次。`publish` 操作符通过向多个用户广播单个订阅来提供共享订阅的机制。

有几个过载`publish`运算符。最方便的过载是那些提供了一个函数封装 observable 共享的副作用的参数。

#### 例 ####

```js
var xs = Rx.Observable.create(observer => {
  console.log('Side effect');
  observer.onNext('hi!');
  observer.onCompleted();
});

xs.publish(sharedXs => {
  sharedXs.subscribe(console.log);
  sharedXs.subscribe(console.log);
  return sharedXs;
}).subscribe();
```

这个例子中，`xs` 是一个有副作用的（写入console） observable。正常情况下，每个单独的订阅都会触发这些副作用。 `publish` 操作符使用`xs`单独给所有订阅者 `sharedXs` 变量去订阅。

#### 何时忽略这条指南 ####

只有当 `publish` 操作符需要共享副作用时才使用这条指南。在大多数情况下，您可以创建单独的订阅，没有任何问题：不管是订阅没有副作用的或是副作用可以执行多次没有任何问题的。
