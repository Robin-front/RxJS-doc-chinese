# 使用 Subjects #

[`Subject`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/subjects/subject.md) 继承了 [`Observable`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) 和 [`Observer`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observer.md), 在某种意义上说，它既是观察者 (observer), 也是被观察者（observable）。 你可以使用一个 subject 订阅所有观察者，然后再订阅接收后端数据的。通过这种方式， subject 可以充当一组观察者和源的代理。您可以使用subject来实现与高速缓存，缓冲和时移定制的观测。此外，您可以使用subject广播数据给多个用户。

默认情况下，subject 不进行跨线程任何同步。他们不采取调度程序，而是假设所有序列化和语法正确性处理是通过subject调用处理方法。subject 简单地广播到用户的线程安全列表中的所有订阅的观察者。这样做有减少开销和提高性能的优势。

## 使用 Subjects ##

在下面的例子中，我们创建了一个subject，订阅该subject，然后使用同一subject发送值给观察者。通过这样做，我们将发布和订阅合并到同一个来源。

除了采用 `Observer`,  [`subscribe`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#rxobservableprototypesubscribeobserver--onnext-onerror-oncompleted) 方法还可以采取onNext的函数，这意味着动作将在每一个项目发布的第一时间被执行。在我们的例子中，只要调用onNext，该结果将被写入控制台。

```js
var subject = new Rx.Subject();

var subscription = subject.subscribe(
	x => console.log('onNext: ' + x),
	e => console.log('onError: ' + e.message),
	() => console.log('onCompleted'));

subject.onNext(1);
// => onNext: 1

subject.onNext(2);
// => onNext: 2

subject.onCompleted();
// => onCompleted

subscription.dispose();

```

下面的示例将展示出`Subject`的代理和广播特性。我们首先创建一个每隔1秒产生的整数的源序列。然后，我们创建一个`Subject`, 并且将它作为一个观察者，这样它会接收这个源发送的所有值。之后，我们创建另外两个订阅，这次 subject作为发射源。`subSubject1` 和 `subSubject2` 两个订阅者将接收通过`Subject`传下来的任何值。

```js
// Every second
var source = Rx.Observable.interval(1000);

var subject = new Rx.Subject();

var subSource = source.subscribe(subject);

var subSubject1 = subject.subscribe(
	x => console.log('Value published to observer #1: ' + x),
	e => console.log('onError: ' + e.message),
	() => console.log('onCompleted'));

var subSubject2 = subject.subscribe(
	x => console.log('Value published to observer #2: ' + x),
	e => console.log('onError: ' + e.message),
	() => console.log('onCompleted'));

setTimeout(() => {
	// Clean up
	subject.onCompleted();
	subSubject1.dispose();
	subSubject2.dispose();
}, 5000);

// => Value published to observer #1: 0
// => Value published to observer #2: 0
// => Value published to observer #1: 1
// => Value published to observer #2: 1
// => Value published to observer #1: 2
// => Value published to observer #2: 2
// => Value published to observer #1: 3
// => Value published to observer #2: 3
// => onCompleted
// => onCompleted

```

## Subjects 的不同类型 ##

`Subject` 对象在 RxJS 库中是一个基本的实现, 但你也可以使用 [`Subject.create`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/subjects/subject.md#rxsubjectcreateobserver-observable) 方法创建你自己的 subject. 有一些通过不同函数实现的Subjects. 所有这些类型的存储通过onNext推到他们一些（或全部）值，并广播他们回到自己的观察者。通过这种方法，它们将冷门的可观察到的热点之一。这意味着，如果你订阅不止这一次变更（即认购- >退订- >再次订阅） This means that if you Subscribe to any of these more than once (i.e. subscribe -> unsubscribe -> subscribe again)，你会至少看到两次相同的值。有关冷热observables的信息，请查看最新的章节[Creating and Subscribing to Simple Observable Sequences](creating.md) .

[`ReplaySubject`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/subjects/replaysubject.md) 存储它发布过的所有值。因些，当你订阅它时，你将自动接收到它发布数据的整个历史，即使当你订阅的时候已经发布了一些数据了。

[`BehaviourSubject`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/subjects/behaviorsubject.md)类似于 `ReplaySubject`, 除了它仅存储它发布过的最后一个值。`BehaviourSubject`还需要初始化时的默认值。当还没有收到 subject的其他值的时候，这个默认值将会发送给它的观察者。 这意味着所有订阅者在订阅的时候会立即收到一个值。除非 `Subject` 已经结束了。

[`AsyncSubject`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/subjects/asyncsubject.md) 类似于 `ReplaySubject`和 `BehaviourSubject`。但它只会保存最后一个值，并且当序列完成（结束）发布时。当观察源很热并且可能结束之前，任何观察者可以订阅它，你可以使用 `AsyncSubject` 类型。这种情况，`AsyncSubject` 仍会提供最后一个值给将来的订阅者。
