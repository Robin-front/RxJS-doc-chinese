# RXJS中的错误处理 #

异步编程中最困难的任务之一就是处理错误。与交互式样式编程不同，我们不能简单地像我们在处理代码块时使用的try / catch / finally方法。

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

## Ignoring Errors with `onErrorResumeNext` ##

The Reactive Extensions borrowed from a number of languages in our design.  One of those features is bringing [`On Error Resume Next`](https://msdn.microsoft.com/en-us/library/5hsw66as.aspx) from Microsoft Visual Basic.  This operation specifies that when a run-time error occurs, control goes to the statement immediately following the statement where the error occurred, and execution continues from that point.  There are some instances with stream processing that you simply want to skip a stream which produces an error and move to the next stream.  We can achieve this with a class based and instance based `onErrorResumeNext` method.

The class based `onErrorResumeNext` continues a stream that is terminated normally or by an `Error` with the next stream or `Promise`.  Unlike `catch`, `onErrorResumeNext` will continue to the next sequence regardless of whether the previous was in error or not.  To make this more concrete, let's use a simple example of mixing error sequences with normal sequences.

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

The instance based `onErrorResumeNext` is similar to the class based version, the only difference being that it is attached to the prototype, but can take another sequence or `Promise` and continue.

## Retrying Sequences ##

When catching errors isn't enough and we want to retry our logic, we can do so with `retry` or `retryWhen` operators.  With the `retry` operator, we can try a certain operation a number of times before an error is thrown.  This is useful when you need to get data from a resource which may have intermittent failures due to load or any other issue.

Let's take a look at a simple example of trying to get some data from a URL and giving up after three tries.

```js
// Try three times to get the data and then give up
var source = get('url').retry(3);

var subscription = source.subscribe(
  data => console.log(data),
  err => console.log(err)
);
```

In the above example, it will give up after three tries and thus call `onError` if it continues to fail after the third try.  We can remedy that by adding `catch` to use an alternate source.

```js
// Try three times to get the data and then return cached data if still fails
var source = get('url').retry(3).catch(cachedVersion());

var subscription = source.subscribe(
  data => {
    // Displays the data from the URL or cached data
    console.log(data);
  }
);
```

The above case retries immediately upon failure.  But what if you want to control when a retry happens?  We have the `retryWhen` operator which allows us to deeply control when the next try happens.  We incrementally back off trying again by using the following method:

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

## Ensuring Cleanup with Finally ##

We've already covered the try/catch part of try/catch/finally, so what about finally?  We have the `finally` operator which calls a function after the source sequence terminates gracefully or exceptionally.  This is useful if you are using external resources or need to free up a particular variable upon completion.  

In this example, we can ensure that our `WebSocket` will indeed be closed once the last message is processed.

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
But we can do a better job in terms of managing resources if need be by using the `using` method.

## Ensuring Resource Disposal ##

As stated above, `finally` can be used to ensure proper cleanup of any resources or perform any side effects as necessary.  There is a cleaner approach we can take by creating a disposable wrapper around our object with a `dispose` method so that when our scope is complete, then the resource is automatically disposed through the `using` operator.

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

## Delaying Errors with `mergeDelayError` ##

Another issue may arise when you are dealing with flattening sequences into a single sequence and there may be errors along the way.  We want a way to flatten without being interrupted by one of our sources being in error.  This is much like the other operator `mergeAll` but the main difference is, instead of immediately bubbling up the error, it holds off until the very end.

To illustrate, we can create this little sample that has an errored sequence in the middle when it is trying to flatten the sequences.
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

## Further Reading ##
- [Using Generators For Try/Catch Operations](generators.md)
- [Testing and Debugging Your RxJS Application](testing.md)
