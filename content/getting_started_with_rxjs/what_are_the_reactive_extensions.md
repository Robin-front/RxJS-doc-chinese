# JavaScript 的 Reactive Extensions(RxJS)到底是什么? #

The Reactive Extensions for JavaScript (RxJS) 是构成使用观察序列和异步和基于事件的程序库[LINQ风格的集成查询操作](http://en.wikipedia.org/wiki/LINQ)。使用RxJS，开发者用异步数据流 *__表示__* 可观察者，*__查询__* 使用异步数据流[LINQ操作](http://msdn.microsoft.com/en-us/library/hh242983.aspx)，和 *__参数__* 并发在使用异步数据流的[调度程序](http://msdn.microsoft.com/en-us/library/hh242963.aspx)。简单地说，的Rx = 可观察者 + LINQ + 调度程序。

无论您是创作基于Web的应用程序或用[Node.js](http://nodejs.org)创建服务器端应用程序，你必须不断地处理异步和基于事件的编程。Web应用程序和Node.js的应用程序在I / O操作和计算量上有大量的任务，可能需要很长的时间才能完成，可能阻塞主线程。此外，处理异常，取消和同步是困难的并且容易出错。

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

## 数据的 `推`与`拉` ##

在交互式编程中, 应用程序积极通过从表示源序列中检索数据来获得数据源的详细信息。这种行为是由JavaScript的Arrays, Objects, Sets, Maps等的迭代器模式为代表，在数组中，要获取下一个值，必须先获取下一个值的索引，或通过ES6 iterators](http://wiki.ecmascript.org/doku.php?id=harmony:iterators)。

The application is active in the data retrieval process: it decides about the pace of the retrieval by calling `next` at its own convenience. This enumeration pattern is synchronous, which means your application might be blocked while polling the data source. Such pulling pattern is similar to visiting your library and checking out a book. After you are done with the book, you pay another visit to check out another one.

在另一方面，在响应式编程中，应用程序通过订阅一个数据流（被称为在RxJS可观察序列）提供了更多的信息，以及任何更新从源传递给它。该应用程序是在数据检索过程中是被动的：除了订阅可观察源，它不主动轮询源，而仅仅是响应推给它的数据。当事件已经完成，源将通知发送给用户。这样一来，您的申请将不会被等待源更新受阻。这是JavaScript的Reactive Extension使用的推模式。这类似于加入其中您注册一个特定类型的兴趣读书俱乐部，并在它们发布你所感兴趣的书会自动发送给您。你不需要在排队等候，以获取你想要的东西。采用推模式是在其中，而应用程序正在等待某些事件，这是必要的，它有自己的一套要求异步JavaScript环境的UI线程无法阻止重UI环境下特别有用。总之，通过使用RxJS，你可以让你的应用程序的响应。
On the other hand, in reactive programming, the application is offered more information by subscribing to a data stream (called observable sequence in RxJS), and any update is handed to it from the source. The application is passive in the data retrieval process: apart from subscribing to the observable source, it does not actively poll the source, but merely react to the data being pushed to it. When the event has completed, the source will send a notice to the subscriber. In this way, your application will not be blocked by waiting for the source to update.

This is the push pattern employed by Reactive Extensions for JavaScript. This is similar to joining a book club in which you register your interest in a particular genre, and books that match your interest are automatically sent to you as they are published. You do not need to stand in a line to acquire something that you want. Employing a push pattern is especially helpful in heavy UI environment in which the UI thread cannot be blocked while the application is waiting for some events, which is essential in JavaScript environments which has its own set of asynchronous requirements. In summary, by using RxJS, you can make your application more responsive.

The push model implemented by Rx is represented by the observable pattern of [`Observable`](../observable/index.html)/[`Observer`](../observer/index.html). The [`Observable`](../observable/index.html) will notify all the observers automatically of any state changes. To register an interest through a subscription, you use the [`subscribe`](../observable/observable_instance_methods/subscribe.html) method of [`Observable`](../observable/index.html), which takes on an [`Observer`](../observer/index.html) and returns a [`Disposable`](../disposables/index.html) object. This gives you the ability to track your subscription and be able to dispose the subscription. You can essentially treat the observable sequence (such as a sequence of mouseover events) as if it were a normal collection. RxJS's built-in query implementation over observable sequences allows developers to compose complex event processing queries over push-based sequences such as events, callbacks, Promises,  HTML5 Geolocation APIs, and much much more.. For more information on these two interfaces, see Exploring The Major Concepts in RxJS.
