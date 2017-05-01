# Reactive Extensions(RxJS)到底是什么? #

The Reactive Extensions for JavaScript (RxJS) 是构成使用观察序列和异步和基于事件的程序库[LINQ风格的集成查询操作](http://en.wikipedia.org/wiki/LINQ)。使用RxJS，开发者用异步数据流 *__表示__* 可观察者，*__查询__* 使用异步数据流[LINQ操作](http://msdn.microsoft.com/en-us/library/hh242983.aspx)，和 *__参数__* 并发在使用异步数据流的[调度程序](http://msdn.microsoft.com/en-us/library/hh242963.aspx)。简单地说，的Rx = 可观察者 + LINQ + 调度程序。

无论您是创作基于Web的应用程序或用[Node.js](http://nodejs.org)创建服务器端应用程序，你必须不断地处理异步和基于事件的编程。Web应用程序和Node.js的应用程序在I/O操作和计算量上有大量的任务，可能需要很长的时间才能完成，可能阻塞主线程。此外，处理异常，取消和同步是困难的并且容易出错。

使用RxJS，你可以代表多个异步数据流（即来自不同的来源，例如，股票报价，tweets，计算机事件，Web服务请求等），并使用[`Observer`](../observer/index.html)订阅事件流。每当发生事件时，[`Observable`](../observable/index.html)通知该订阅[`Observer`](../observer/index.html)。

由于可观察序列是数据流，可以使用实现的可观察对象的扩展方法对它们进行查询。因此，你可以筛选，管理，汇总，撰写，非常容易使用这些标准查询操作对多个事件进行基于时间的操作。此外，还有一些其他特定的反应流的允许写入功能强大的查询。取消，异常和同步是也可以通过使用以Rx提供的扩展方法正常处理。

RxJS与同步数据流之间平滑兼容，例如数组，`Sets`、`Maps`和单值异步计算诸如`Promises`如下面的图：

<table style="display: table">
   <th></th><th>返回单个值</th><th>返回多个值</th>
   <tr>
      <td>拉/同步/交互式</td>
      <td>对象</td>
      <td>Iterables (Array | Set | Map | Object)</td>
   </tr>
   <tr>
      <td>推/异步/反应式</td>
      <td>Promise</td>
      <td>Observable</td>
   </tr>
</table>

## 数据的 `push`与`pull` ##

在交互式编程中, 应用程序通过频繁地查询数据源来获取数据。这种行为是由JavaScript的Arrays, Objects, Sets, Maps等的迭代器模式为代表，在数组中，要获取下一个值，必须先获取下一个值的索引，或通过ES6 iterators](http://wiki.ecmascript.org/doku.php?id=harmony:iterators)。

应用程序在数据检索过程中是活跃的: 它决定了检索的速度，通过在合适的时机调用 `next`。此枚举模式是同步的，这意味着您的应用程序在轮询数据源时可能会被阻塞。这种拉动模式类似于读取你的库和检出一本书。在你完成这本书后，你才再去看另一本书。

在另一方面，在响应式编程中，应用程序通过订阅一个数据流（在RxJS中被称为可观察序列observable）提供了更多的信息，以及任何更新从源传递给它。该应用程序是在数据检索过程中是被动的：除了订阅可观察源以外，它不会主动轮询源，而仅仅是响应推给它的数据。当事件完成时，源将向订阅者发送通知。这样一来，您的应用程序将不会被等待源的更新而阻塞。

这是JavaScript的Reactive Extension采用的推模式。这类似于加入一个书友会，你登记你的兴趣，并在它们发布你所感兴趣的书会自动发送给您。你不需要在排队等候去获取你想要的东西。使用推模式特别有助于在复杂UI的环境中，当应用程序在等待一些事件时，UI线程不能被阻塞。这在JavaScript环境中是必不可少的，因为它有自己的异步需求集。总之，通过rxjs，你可以让你的应用更加灵活。

Rx 是通过的[`Observable`](../observable/index.html)/[`Observer`](../observer/index.html) 观察者模式实现推模式的。当有任何状态发生改这变时，[`Observable`](../observable/index.html)将会自动通知所有观察者。你可以使用 [`Observable`](../observable/index.html) 的 [`subscribe`](../observable/observable_instance_methods/subscribe.html) 方法订阅你感兴趣的内容。该方法包含观察者并返回可销毁的对象。这使您能够跟踪订阅并能够处理订阅。可以将可观察序列（如一系列的mouseover事件）视为正常集合。RxJS对观测序列的内置查询的实现允许开发者组合基于推送的复杂事件，如事件、回调、Promises、HTML5 Geolocation API，以及更多..在这两个接口的更多信息，参见RxJS主要的概念探索。
