# Generators 与可观测序列 #

ES6更令人激动的功能之一是称为`generators`的新功能特性。他们已经在Firefox多年了，尽管它们现在终于在ES6中被标准化了，并将在您使用的其他浏览器普及。`generators`与正常功能的不同之处在于，正常功能（如下列操作）将直接运行直到完成，无论是否是异步的。

```js
function printNumberOfTimes(msg, n) {
  for (var i = 0; i < n; i++) {
    console.log(msg);
  }
}

printNumberOfTime('Hello world', 1);
// => Hello world

// 异步
setTimeout(() => console.log('Hello from setTimeout after one second'), 1000);
// => Hello from setTimeout after one second
```

`generator`不是运行到完成，而是允许我们通过引入具有暂停功能的`yield`关键字来中断功能的流程。如果没有外部消费者说他们需要下一个值，该功能就无法自行恢复。

要创建一个`generator`函数，你必须使用这个`function*`语法，然后就成为一个`generator`。在这个特殊的例子中，我们将产生一个单一的价值，即生命的意义。

```js
function* theMeaningOfLife() {
  yield 42;
}
```

要获取值，我们需要调用该函数，然后调用`next`获取下一个值。来自`next`调用的返回值将具有关于是否完成的标志，以及所产生的任何值。请注意，在我们开始调用之前，该函数不会执行任何`next`操作。

```js
var it = theMeaningOfLife();

it.next();
// => { done: false, value: 42 }

it.next();
// => { done: true, value: undefined }
```

我们还可以使用一些ES6的简写来获取来自`generator`的值,比如 `for..of`:

```js
for (var v of theMeaningOfLife()) {
  console.log(v);
}
// => 42
```

这当然只是模拟了`generators`能够做什么的表面现象，因为我们更侧重于产生值的简单性质。

由于RxJS相当重视标准，因此我们还会在新的语言特性标准化的同时寻找方法，以便您可以利用它们，并结合RxJS的功能。

## Async/Await 风格与 RxJS ##

JavaScript的一个常见吐槽是异步行为的回调。幸运的是，这可以通过类库方法很容易解决。为此，我们介绍一下[`Rx.Observable.spawn`](../../observable/observable_methods/spawn.md)可以直接编写代码的方式，不仅可以产生可观察序列，还可以产生`Promises`，回调，数组等。这样可以非常紧凑地编写代码，而无需任何回调，但也带来了是否要调用`timeout`，`retry`，`catch`这些RxJS的能力或与此有关的任何其他方法。请注意，这只产生一个值，但是在RxJS术语中，这仍然是非常有用的。

例如，我们可以从[Bing.com](Bing.com)获取HTML并将其写入控制台，超时时间为5秒，如果没有及时响应，则会发出错误。我们也可以添加类似的东西`retry`，`catch`以便我们可以例如尝试三次，然后如果失败，给出默认响应或缓存版本。

```js
var Rx = require('rx');
var request = require('request');
var get = Rx.Observable.fromNodeCallback(request);

Rx.Observable.spawn(function* () {
  var data;
  try {
    data = yield get('http://bing.com').timeout(5000 /*ms*/);
  } catch (e) {
    console.log('Error %s', e);
  }

  console.log(data);
}).subscribe();
```

## 操作符与 Generators 配合 ##

RxJS内的许多操作符也支持`generators`。例如，我们可以使用该[`Rx.Observable.from`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/from.md)方法来获取一个`generator`函数，在这种例子中，它是一个斐波那契序列，显示它其中的10个。

```js
function* fibonacci(){
  var fn1 = 1;
  var fn2 = 1;
  while (1) {
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;
    yield current;
  }
}

Rx.Observable.from(fibonacci())
  .take(10)
  .subscribe(x => console.log('Value: %s', x));

//=> Value: 1
//=> Value: 1
//=> Value: 2
//=> Value: 3
//=> Value: 5
//=> Value: 8
//=> Value: 13
//=> Value: 21
//=> Value: 34
//=> Value: 55
```

这只是一个开始，因为有几个运算符，如[`concatMap`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatmap.md)/[`selectConcat`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatmap.md) and [`flatMap`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/selectmany.md)/[`selectMany`](https://github.com/Reactive-Extensions/RxJS/tree/master/doc/api/core/operators/concatmap.md) ，它将iterables作为参数，以便我们进一步启用组合。例如，我们可以从一个`flatMap`操作项目中使用generators。

```js
var source = Rx.Observable.of(1,2,3)
  .flatMap(
    (x, i) => function* () { yield x; yield i; }(),
    (x, y, i1, i2) => x + y + i1 + i2
  );

var subscription = source.subscribe(
  x => console.log('onNext: %s', x),
  e => console.log('onError: %s', e),
  () => console.log('onCompleted'));

// => Next: 2
// => Next: 2
// => Next: 5
// => Next: 5
// => Next: 8
// => Next: 8
// => Completed
```

JavaScript的未来是令人兴奋的，generators为我们的应用程序添加了新的可能性，以允许他们混合和匹配我们的编程风格。

请注意，在此示例中，`move`将成为可观察的序列，我们可以在其中进一步操作。该[查询可观察序列](querying.md)的话题将告诉你如何预测这序列转换点类型的集合和过滤其内容，让您的应用程序将只接收满足特定的标准值。

## 相关阅读

参考
- [Querying Observable Sequences](querying.md)
