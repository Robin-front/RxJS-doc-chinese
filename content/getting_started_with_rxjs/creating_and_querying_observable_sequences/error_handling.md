# RXJS中的错误处理 #

异步编程中最困难的任务之一就是处理错误。与交互式编程不同，我们不能简单地像我们在处理代码块时使用的try / catch / finally方法。

> One of the most difficult tasks in asynchronous programming is dealing with errors.  Unlike interactive style programming, we cannot simply use the try/catch/finally approach that we use when dealing with blocking code.

```js
try {
  for (var obj in objs) {
    doSomething(obj);
  }
} catch (e) {
  handleError(e);
} finally {
  doCleanup();
}
```

这些动作正好反映了我们的`Observer`类，它有以下契约，用于将零到无限项，`onNext`并且可选地处理`Error`与`onError`或成功完成的`onCompleted`。

> These actions mirror exactly our `Observer` class which has the following contract for handing zero to infinite items with `onNext` and optionally handling either an `Error` with `onError` or successful completion with `onCompleted`.

```typescript
interface Observer<T> {
  onNext(value: T) : void
  onError(error: Error) : void
  onCompleted() : void
}
```

但是try / catch / finally方法不能与异步代码一起使用。作为替代，我们有多种处理错误的方法，并确保妥善处理资源。

> But the try/catch/finally approach won't work with asynchronous code.  Instead, we have a myriad of ways of handling errors as they occur, and ensure proper disposal of resources.

例如，我们可能想要执行以下操作：
- 接受错误并切换到备份`Observable`以继续执行序列
- 接受该错误并发射一个默认值
- 接受该错误，并立即尝试重新启动失败的Observable
- 接受该错误，并尝试重新启动失败的Observable 在一些后台定时器之后

我们将在本节中介绍这每个场景和更多内容。

## 捕捉错误 ##

第一个主题是随着我们的数据流发生错误。在RXJS中，通过`onError`停止序列的通道传播的任何错误。我们可以通过`catch`在类和实例级别使用运算符来弥补这一点。

> The first topic is catching errors as they happen with our streams. In the Reactive Extensions, any error is propogated through the `onError` channel which halts the sequence.  We can compensate for this by using the `catch` operator, at both the class and instance level.  

使用类级别`catch`方法，我们可以捕获当前序列发生的错误，然后如果存在错误，则移动到下一个序列。例如，我们可以尝试从多个URL获取数据，因为它们都具有相同的数据，因此无论如何，如果失败，默认为缓存版本，因此错误不应该传播。需要注意的一点是，如果`get('url')`调用成功，那么它不会移动到列表中的下一个序列。

> Using the class level `catch` method, we can catch errors as they happen with the current sequence and then move to the next sequence should there be an error.  For example, we could try getting data from several URLs, it doesn't matter which since they all have the same data, and then if that fails, default to a cached version, so an error should never propagate.  One thing to note is that if `get('url')` calls succeed, then it will not move onto the next sequence in the list.

```js
var source = Rx.Observable.catch(
  get('url1'),
  get('url2'),
  get('url3'),
  getCachedVersion()
);

var subscription = source.subscribe(
  data => {
    // Display the data as it comes in
  }
);
```

我们还有一个实例版本`catch`可以使用两种方式。第一种方式就像上面的例子，我们可以在现有的流中捕获错误并移动到下一个流或`Promise`。

We also have an instance version of `catch` which can be used two ways.  The first way is much like the example above, where we can take an existing stream, catch the error and move onto the next stream or `Promise`.

```js
var source = get('url1').catch(getCachedVersion());

var subscription = source.subscribe(
  data => {
    // Display the data as it comes in
  }
);
```

另一个超载`catch`允许我们检查错误，因为它进来，所以我们可以决定采取哪条路线。例如，如果从我们的Web服务器返回错误状态代码500，我们可以假设它已经关闭，然后使用缓存版本。

> The other overload of `catch` allows us to inspect the error as it comes in so we can decide which route to take.  For example, if an error status code of 500 comes back from our web server, we can assume it is down and then use a cached version.

```js
var source = get('url1').catch(e => {
  if (e.status === 500) {
    return cachedVersion();
  } else {
    return get('url2');
  }
});

var subscription = source.subscribe(
  data => {
    // Display the data as it comes in
  }
);
```

这不是处理错误的唯一方式，因为有很多其他方法，如下所示。

## 使用 `onErrorResumeNext` 忽略错误 ##

在我们的RXJS设计中借鉴了多种语言。其中一个功能[`On Error Resume Next`](https://msdn.microsoft.com/en-us/library/5hsw66as.aspx)来自`Microsoft Visual Basic`。此操作指定当运行发生错误时，控件将转到发生错误的语句之后的语句，并从该点继续执行。有一些流处理的实例，您只想跳过产生错误的流并移动到下一个流。我们可以通过基于类和实例的`onErrorResumeNext`方法来实现。

> The Reactive Extensions borrowed from a number of languages in our design.  One of those features is bringing [`On Error Resume Next`](https://msdn.microsoft.com/en-us/library/5hsw66as.aspx) from Microsoft Visual Basic.  This operation specifies that when a run-time error occurs, control goes to the statement immediately following the statement where the error occurred, and execution continues from that point.  There are some instances with stream processing that you simply want to skip a stream which produces an error and move to the next stream.  We can achieve this with a class based and instance based `onErrorResumeNext` method.

基于类`onErrorResumeNext`将会使数据流继续直到正常终止或因为`Error`而执行下一个数据流或`Promise`。不同于`catch`，`onErrorResumeNext`将继续下一个序列，无论以前是否有错误。为了使这个更具体，让我们使用一个简单的例子来混合错误序列与正常的序列。

> The class based `onErrorResumeNext` continues a stream that is terminated normally or by an `Error` with the next stream or `Promise`.  Unlike `catch`, `onErrorResumeNext` will continue to the next sequence regardless of whether the previous was in error or not.  To make this more concrete, let's use a simple example of mixing error sequences with normal sequences.

```js
var source = Rx.Observable.onErrorResumeNext(
  Rx.Observable.just(42),
  Rx.Observable.throw(new Error()),
  Rx.Observable.just(56),
  Rx.Observable.throw(new Error()),
  Rx.Observable.just(78)
);

var subscription = source.subscribe(
  data => console.log(data)
);
// => 42
// => 56
// => 78
```

基于`onErrorResumeNext`类的实例类似于基于类的版本，唯一的区别是它附加到原型，但可以采用另一个序列或`Promise`继续。

> The instance based `onErrorResumeNext` is similar to the class based version, the only difference being that it is attached to the prototype, but can take another sequence or `Promise` and continue.

## 重试 ##

当捕获错误是不足够好时，我们想重试我们的逻辑，我们可以尝试`retry`或`retryWhen`操作符。使用`retry`操作符，我们可以在发生错误之前多次尝试一定的操作。当您需要从数据源获取数据，可能由于加载或任何其他问题而产生间歇性故障时，这很有用。

> When catching errors isn't enough and we want to retry our logic, we can do so with `retry` or `retryWhen` operators.  With the `retry` operator, we can try a certain operation a number of times before an error is thrown.  This is useful when you need to get data from a resource which may have intermittent failures due to load or any other issue.

我们来看一个简单的例子，试图从URL中获取一些数据，并在三次尝试后放弃。

> Let's take a look at a simple example of trying to get some data from a URL and giving up after three tries.

```js
// Try three times to get the data and then give up
var source = get('url').retry(3);

var subscription = source.subscribe(
  data => console.log(data),
  err => console.log(err)
);
```

在上面的例子中，它会在三次尝试后放弃，如果在第三次尝试后继续失败，则调用`onError`。我们可以通过添加`catch`使用备用来补救。

> In the above example, it will give up after three tries and thus call `onError` if it continues to fail after the third try.  We can remedy that by adding `catch` to use an alternate source.

```js
// Try three times to get the data and then return cached data if still fails
var source = get('url').retry(3).catch(cachedVersion());

var subscription = source.subscribe(
  data => {
    // 显示来自URL或缓存的数据 Displays the data from the URL or cached data
    console.log(data);
  }
);
```

上述情况在失败后立即重试。但是，如果你想控制重试的时间怎么办？我们有一个`retryWhen`操作符，允许我们深度控制下一次重试的时机。我们通过使用以下方法逐步退出重试：

> The above case retries immediately upon failure.  But what if you want to control when a retry happens?  We have the `retryWhen` operator which allows us to deeply control when the next try happens.  We incrementally back off trying again by using the following method:

```js
var source = get('url').retryWhen(
   attempts =>
    attempts
      .zip(Observable.range(1, 3), (_, i) => i)
      .flatMap(i => {
        console.log('delay retry by ' + i + ' second(s)');
        return Rx.Observable.timer(i * 1000);
      });
);

var subscription = source.subscribe(
  data => {
    // Displays the data from the URL or cached data
    console.log(data);
  }
);
// => delay retry by 1 second(s)
// => delay retry by 2 second(s)
// => Data
```

## 最后确保清理（释放资源） ##

我们已经覆盖了`try / catch` / `try / finally`的`try / catch`部分，最后呢？`finally`在数据源正常或异常终止之后，我们有一个调用函数的运算符。如果您使用外部资源或完成后需要释放特定变量，这将非常有用。

> We've already covered the try/catch part of try/catch/finally, so what about finally?  We have the `finally` operator which calls a function after the source sequence terminates gracefully or exceptionally.  This is useful if you are using external resources or need to free up a particular variable upon completion.  

在这个例子中，我们可以确保`WebSocket`一旦处理最后一个消息，我们确保它将被关闭。

> In this example, we can ensure that our `WebSocket` will indeed be closed once the last message is processed.

```js
var socket = new WebSocket('ws://someurl', 'xmpp');

var source = Rx.Observable.from(data)
  .finally(() => socket.close());

var subscription = source.subscribe(
  data => {
    socket.send(data);
  }
);
```
但是，如果需要使用`using`方法，我们可以在管理资源方面做得更好。

> But we can do a better job in terms of managing resources if need be by using the `using` method.

## 确保资源回收 ##

如上所述，`finally`可以用于确保适当地清理任何资源或根据需要执行任何副作用。我们可以通过使用`dispose`方法在我们的对象周围创建一次性包装来实现一个更干净纯粹的方法，以便当我们的上下文完成时，资源将通过`using`操作行自动进行处理回收。

> As stated above, `finally` can be used to ensure proper cleanup of any resources or perform any side effects as necessary.  There is a cleaner approach we can take by creating a disposable wrapper around our object with a `dispose` method so that when our scope is complete, then the resource is automatically disposed through the `using` operator.

```js
function DisposableWebSocket(url, protocol) {
  var socket = new WebSocket(url, protocol);

  // Create a way to close the WebSocket upon completion
  var d = Rx.Disposable.create(() => socket.close());

  d.socket = socket;

  return d;
}

var source = Rx.Observable.using(
  () => new DisposableWebSocket('ws://someurl', 'xmpp'),
  d =>
    Rx.Observable.from(data)
      .tap(data => d.socket.send(data));
  }
);

var subscription = source.subscribe();
```

## 使用 `mergeDelayError` 延迟错误 ##

可能会出现另一个问题，当您将扁平化序列处理为单个序列时，可能会有错误。我们想要一个平滑的处理方式，而不会被我们的一个数据源错误中断。这与另一个操作符`mergeAll`非常相似，但主要区别在于，它不会立即抛出错误，而是延迟直到最后。

> Another issue may arise when you are dealing with flattening sequences into a single sequence and there may be errors along the way.  We want a way to flatten without being interrupted by one of our sources being in error.  This is much like the other operator `mergeAll` but the main difference is, instead of immediately bubbling up the error, it holds off until the very end.

为了说明，我们可以创建这个小样本，当它试图展开整个序列时，中间夹有一个错误的序列。

> To illustrate, we can create this little sample that has an errored sequence in the middle when it is trying to flatten the sequences.

```js
var source1 = Rx.Observable.of(1,2,3);
var source2 = Rx.Observable.throwError(new Error('woops'));
var source3 = Rx.Observable.of(4,5,6);

var source = Rx.Observable.mergeDelayError(source1, source2, source3);

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => 1
// => 2
// => 3
// => 4
// => 5
// => 6
// => Error: Error: woops
```

## 进一步阅读 ##
- [使用Generators进行Try/Catch操作](./generators_and_observable_sequenes.md)
- [测试和调试您的RxJS应用程序](../testing_and_debugging.md)
