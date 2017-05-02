# 探索 RxJS 的主要概念 #

本主题描述用于表示可观察序列并订阅它们的 RX 主要对象。

`Observable` / `Observer` 对象是 RxJS 的核心部分。

## `Observable` / `Observer` ##

RX 通过核心对象 [`Observable`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) 为抽象化的基于推送的可观察序列暴露异步和基于事件的数据源。它代表可以观察到的数据源，这意味着它可以发送数据给订阅者。

由于在 [What is RxJS](what_are_the_reactive_extensions.md) 已经描述了， push模型的另一半由[`Observer`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observer.md) 对象表示，观察者通过订阅进行注册特定的信息。信息随后推送给订阅可观察序列的观察者。

为了从可观察序列集合中接收通知，可以使用 `observable` 的 [`subscribe`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypesubscribeobserver--onnext-onerror-oncompleted) 方法去传递给 `observer` 对象。 作为观察者的返回值， [`subscribe`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypesubscribeobserver--onnext-onerror-oncompleted) 方法返回一个`Disposable`对象作为订阅的返回值。允许在完成订阅之后销毁它。在这个对象分离出来的观察者上调用 `dispose`，以便通知不再推送给这个观察者。可以推断，在RxJS你不需要显式地取消订阅事件中常见的JavaScript事件模型。

观察者支持三种事件发布，映射对象方法。[`onNext`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observer.md#rxobserverprototypeonnextvalue)可以被调用任意次，如果数据源可用的话。举个例子，用于鼠标移动事件的可观察数据源可以在每次鼠标移动时发送事件对象。其他两种方法用于指示完成或错误。

下面列出了`Observable` / `Observer` 除了`Disposable`对象。

```js
/**
 * 定义释放分配资源的方法.
 */
function Disposable() { }

/**
 * 执行与释放、释放或重置资源相关的应用程序定义的任务.
 */
Disposable.prototype.dispose =  () => { ... }

/**
 * 定义用于推送通知的数据源
 */
function Observable() { }

/**
 * 通知提供者，观察者将接收通知
 *
 * @param {Observer} observer 接收通知的对象
 * @returns {Disposable} 一种一次性的引用，它允许观察者在提供者完成发送之前停止接收通知。
 */
Observable.prototype.subscribe =  observer => { ... }

/**
 * 提供接收基于推送通知的机制。
 */
function Observer() { }

/**
 * 提供新的数据给观察者
 *
 * @param {Any} value 当前的通知信息.
 */
Observer.prototype.onNext = value => { ... };

/**
 * 通知观察者，提供程序处理异常的条件
 *
 * @param {Error} error 提供有关错误的附加信息的.
 */
Observer.prototype.onError = error => { ... };

/**
 * 通知观察者，提供程序已完成发送基于推送的通知
 */
Observer.prototype.onCompleted = () => { ... };
```

RxJS 也提供 `subscribe` 的能力，这样你就可以避免自己实现`Observer`对象。为每个发布事件（` OnNext `，` OnError `，` onCompleted `）可观察序列，你可以指定一个函数将被调用，如下面的示例所示。如果不指定事件的操作，将发生默认行为。

```js
// 创建从1开始、包含5个整数的可观察序列
var source = Rx.Observable.range(1, 5);

// 输出每个数据
var subscription = source.subscribe(
	x => console.log('onNext: ' + x),
	e => console.log('onError: ' + e.message),
	() => console.log('onCompleted'));

// => onNext: 1
// => onNext: 2
// => onNext: 3
// => onNext: 4
// => onNext: 5
// => onCompleted
```

可以将可观察序列（如鼠标事件的序列）视为正常集合。因此，您可以在集合中编写查询，以便进行筛选、分组、组合等操作。为了让可观察序列再有用，更易操作， RxJS 库提供许多工厂方法给操作者，这样就不需要自己去实现这些操作。这些都会在[Querying Observable Sequences](creating_and_querying_observable_sequences/README.md) 这个主题文章中提到。

### 注意:
你不需要自己实现 Observable/Observer 对象。Rx 为您提供这些接口的内部实现，并通过可观察的和观察者类型提供的各种扩展方法暴露它们。查看 [Creating and Querying Observable Sequences](creating_and_querying_observable_sequences/README.md) 文章以获取更多信息。

### 相关文章

概念
- [查询可观察序列](creating_and_querying_observable_sequences/querying_observable_sequences.md)

其他资源
- [创建和查询可观察序列](creating_and_querying_observable_sequences/README.md)
