# RxJS 约定

### RxJS语法

订阅者实例接收三个参数， 第一个表示 `onNext` 的回调， 第二、第三参数可选，分别表示 `onError`、`onCompleted`回调。

此语法允许观察序列任意次调用`onNext`以发送消息到订阅的观察者实例，随后通过单个成功（`onCompleted`）或失败（`onError`）消息结束。

#### 例子 ####
```js
var count = 0;
xs.subscribe(
  () => count++,
  err => console.log('Error: %s', err.message),
  () => console.log('OnNext has been called %d times', count)
);
```

在此示例中，我们确认，一旦`OnCompleted`方法被调用，`OnNext`的将不会再被调用。

### 可以认为在`OnError`或`onCompleted`执行后，资源会被释放

`OnError`或`onCompleted`调用后就不会再接收信息流了。我们可以在`OnError`或 `onCompleted`回调中执行销毁订阅。必须要立即清理资源，以确保不会发生任何副作用。它也确保运行库可以回收这些资源。

#### 例 ####
```js
var fs = require('fs');
var Rx = require('rx');

function appendAsync(fd, buffer) { /* impl */ }

function openFile(path, flags) {
  var fd = fs.openSync(path, flags);
  return Rx.Disposable.create(() => fs.closeSync(fd));
}

Rx.Observable.
  using(
    () => openFile('temp.txt', 'w+'),
    fd => Rx.Observable.range(0, 10000).map(v => Buffer(v)).flatMap(buffer => appendAsync(fd, buffer))
  ).subscribe();
```

`using`操作符创建一个包含退订的资源。这个Rx 消毁约定是为了确保，当`onError` 或 `onCompleted`事件执行的时候，自动调用一次退订。

#### 何时忽略本指南 ####

还没发现，任何时候都应该尽量遵循本指南。

### 可以认为退订的时候Rx将会尽力停止所有的逻辑 ###

当订阅者退订的时候，可观察对象将会尽力停止当前的所有的工作。这意味着任何队列里未开始的逻辑将不会被执行。

某些未执行完的逻辑可能会继续执行，因为中断运行中的逻辑不总是安全的。但是这些(未执行完的)逻辑的结果不会再被推送到订阅者实例。

#### 例 1
```js
Observable.timer(2000).subscribe(...).dispose()
```

这个例子订阅了一个 由`Scheduler.timeout` 生成的定时器， 它将每2秒调用 `onNext` 来发送信息流。然后立即取消订阅。由于还未被调用，它将立即从调度程序中移除。

#### 例 2
```js
Rx.Observable.startAsync(() => Q.delay(2000)).subscribe(...).dispose();
```

这个例子中，`startAsync`操作符将会立即执行传入的函数，这个订阅会注册一个观察者实例去监听匿名函数的返回值。 由于销毁订阅的时候匿名函数已经开始运行了，所以它将会继续执行，但它返回的任何值都会被忽略。
