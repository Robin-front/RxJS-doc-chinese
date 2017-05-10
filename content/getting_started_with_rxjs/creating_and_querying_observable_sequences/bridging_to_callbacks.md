# 桥接回调 #

除了事件之外，还有其他的异步数据源存在在web端和服务端。其中之一就是 node.js 中频繁用到的回调模式。这种设计模式中，传递函数作为参数，然后回调通常是最后一个参数，当执行时，通过数据传递将内部作用域的控制权传给回调。Node.js 有标准的方式实现回调，回调被调用的时候如果有错误，将会首先返回 `Error` 对象，否则返回 null, 然后是回调附加额外的参数。

## 将回调转换成数据流 ##

Node.js 中的许多异步方法和 JavaScript APIs以这样一种方式编写，它有一个回调作为最后一个参数。这些标准的回调会带着传递给它的数据被调用。我们可以使用 [`Rx.Observable.fromCallback`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromcallback.md) 去封装这种回调。 注意，这不包括 Node.js 风格回调，`Error` 作为第一个参数。对于那种操作， 我们提供 [`Rx.Observable.fromNodeCallback`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromnodecallback.md) 去包含上面这种情况。

下面的例子，我们将会将 Node.js 的 [`fs.exists`](http://nodejs.org/api/fs.html#fs_fs_exists_path_callback) 函数进行转换。 这种函数传入一个 `path`，然后返回一个 `true` 或者 `false` 值来表示文件是否存在，这个例子中，我们会检测 'file.txt' 是否存在。 当被 `Rx.Observable.fromCallback` 封装返回的参数，将包含传递给回调的参数的数组。

```js
var Rx = require('rx'),
	fs = require('fs');

// 封装 exists 方法
var exists = Rx.Observable.fromCallback(fs.exists);

var source = exists('file.txt');

// 获取第一个参数 true/false
var subscription = source.subscribe(
	x => console.log('onNext: %s', x),
	e => console.log('onError: %s', e),
	() => console.log('onCompleted'));

// => onNext: true
// => onCompleted
```

## 将 Node.js 风格的 Callbacks 转换为数据流 ##

Node.js采用了公约在许多回调地方可能出现错误，像是文件 I/O, 网络请求等。RxJS 支持这个约定通过 [`Rx.Observable.fromNodeCallback`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/fromnodecallback.md) 方法处理错误，如果出现，错误会被捕获并通过 `onError` 发送通知。否则，`onNext`会发送回调参数，然后是 `onCompleted` 通知。

下面这个例子，我们将 Node.js [`fs.rename`](http://nodejs.org/api/fs.html#fs_fs_rename_oldpath_newpath_callback) 函数转换成数据流。

```js
var fs = require('fs'),
    Rx = require('rx');

// Wrap fs.rename
var rename = Rx.Observable.fromNodeCallback(fs.rename);

// 重命名文件，除了错误，没有参数返回
var source = rename('file1.txt', 'file2.txt');

var subscription = source.subscribe(
	x => console.log('onNext: success!'),
	e => console.log('onError: %s', e),
	() => console.log('onCompleted'));

// => onNext: success!
// => onCompleted
```

## 将数据流转换成 Callbacks ##

我们可以很轻易地将数据流转换成 callback. 这当然需要观察到的序列产生只有一个值，这是有道理的. 让我们将 [`timer`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/timer.md) 方法转换后来实现等待一段时间。`toCallback` 的实现看起来像下面这样，注意，这没有包含在 RxJS 中，但需要的话你可以很简单地添加它。

```js
Rx.Observable.prototype.toCallback = cb => {
  var source = this;
  return () => {
    var val, hasVal = false;
    source.subscribe(
      x=> { hasVal = true; val = x; },
      e => throw e, // Default error handling
      () => hasVal && cb(val)}
    );
  };
};
```

然后，我们可以执行我们的命令就像下面这样:

```js
function cb (x) { console.log('hi!'); }

setTimeout(
  Rx.Observable.timer(5000)
    .toCallback(cb)
  , 500);
```

## 将可观察对象转换成 Node.js 风格的 Callbacks ##

同样地你可能也希望使用 Node.js 风格的 callback。 同样的，就像上面那样，也限制有一个单一的值和结束。`toNodeCallback`的实现像下面这样。注意，这没有包含在 RxJS 中，但需要的话你可以很简单地添加它。

```js
Rx.Observable.prototype.toNodeCallback = cb => {
  var source = this;
  return () => {
    var val, hasVal = false;
    source.subscribe(
      x => { hasVal = true; val = x; },
      e => cb(e),
      () => hasVal && cb(null, val)}
    );
  };
};
```

我们可以这样用，例如如果我们有一个观察序列，调用时可以获取一个值，然后将其转换为Node.js的风格。

```js
getData().toNodeCallback((err, data) => {
	if (err) { throw err; }
	// Do something with the data
});
```
